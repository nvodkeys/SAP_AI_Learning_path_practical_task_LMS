# Incident Resolution — INC-2026-0117 (Incident bank #3, Integration failure)

**Ticket:** `60_Production_Incident.md` · **Feature:** F-01 Duplicate flag · **Blueprint refs:** §1.1 P3, §2 R3, §3 I2/I4/I5, §7 E2/E3, §8
**Resolution status:** Workaround active 05.11.2026 · Permanent fix deployed 06.11.2026 · Retest passed → ticket ready to close

**[VERIFY]** = confirm with SAP / help.sap.com · **[ASSUMPTION]** = working assumption · all IDs synthetic

---

## 1. Support process and ownership

| Level | Owner | What they did |
| --- | --- | --- |
| L1 Service desk | Service desk agent | Logged INC-2026-0117, classified *Integration / F-01*, P3, collected examples and screenshots. |
| L2 Application support | SAP support consultant (me) | Owned the investigation: functional checks (config, statuses, fields), case history, log analysis, workaround. |
| L3 BTP / CAP development | F-01 dev team (US-03 owner) | Code-level confirmation, fix, deployment through the pipeline with approval (SoD theme 4 — devs had no access to production case data; I provided log extracts). |
| SAP product support | Component for Service Cloud v2 / Joule case creation **[VERIFY component]** | Informational question raised: is the two-step case creation from Joule intended behaviour? Not blocking the fix. |
| Change approval | Support ops lead + architect | Approved the change to retry timing; flagged a possible later scope change (see §6). |

Escalation from L2 to L3 happened after hypothesis H4 was confirmed by evidence (§2), because the fix needs a code/config change in the CAP service which L2 does not own.

---

## 2. Investigation trail

### Symptom (restated)
Some new pilot cases that match an open case of the same customer + system ID + category get **no flag**. Others are flagged correctly. No error is visible to dispatchers.

### Hypotheses and evidence (in the order checked)

| # | Hypothesis | Evidence checked | Result | Conclusion |
| --- | --- | --- | --- | --- |
| H1 | The "case created" event did not reach the CAP service (subscription broken, event not fired). | CAP application log for case 4812 and 4806. BTP event subscription status. | Log shows the event **received** for 4812 at 09:14:03 and 4806 at 16:03:41, acknowledged HTTP 200. Subscription active. | **Rejected.** The case did reach the service. |
| H2 | Technical user / OAuth failure (expired or changed secret, missing authorization) — would match TC-US02-09. | Destination `SC2_CASE_API`, token calls in log, security log. | No HTTP 401/403 in the period. Case 4815 (same day, same customer) was queried and updated successfully by `TU_DUPCHK`. | **Rejected.** An auth problem would hit all cases, not some. |
| H3 | Rule data mismatch: open case 4790 not found (status not in open status list, different category, system ID spelling — like PK-DQ-02 / PK-DQ-03). | Case 4790 data; CAP open status list config; category codes. | 4790: status *In Process* (in list), category identical, system ID `PRD`. 4815 matched 4790 correctly, so 4790 is findable. | **Rejected** for the open case side. |
| H4 | The **new** case did not have all data the rule needs at the moment the check ran (system ID empty → rule skips, fail-open per US-03 AC 2 / E2). | (a) CAP log entries for 4812 and 4806; (b) change history of 4812 and 4806; (c) all "skipped" log entries since go-live, grouped by channel. | (a) Both cases: `system ID empty`, 3 tries within ~6 s, then error logged, no flag. (b) 4812 created 09:14:02 by the Joule channel with **empty** system ID; system ID `PRD` written **17 s later** (09:14:19) by the same channel user. 4806: same pattern, 22 s gap. (c) 15 `system ID empty` errors since 26.10. — **all 15 on Joule-created cases**; 0 on form-created cases. | **Confirmed.** |

### Conclusion
The case reaches the duplicate check, but for cases created through Joule the system ID is written in a **second update a few seconds after creation**. The check runs on the "case created" event, finds an empty system ID, retries 3 times within about 6 seconds, gives up and logs an error. The case then follows the normal flow **without a flag** (fail-open, as designed). Nobody is alerted about the logged errors, so the failure is silent.

---

## 3. Root cause

**Primary cause — timing gap between case creation and system ID population (integration sequencing):**
Joule-created cases are created first and enriched with the system ID shortly afterwards **[VERIFY with SAP: intended behaviour of Joule case creation]**. The F-01 design (US-03) assumed all rule fields are present at the "case created" event. The retry policy (3 immediate retries) was built for short API outages (TC-US03-06), not for data that arrives 15–30 s later.

**Contributing causes:**
1. **Test gap:** TC-US01-08 (Joule end-to-end) passed in SIT because event delivery there took 30–60 s — by then the system ID was already filled. In production the event arrives in under 2 s. TC-US03-05 tested an empty system ID only as a manual clearing edge case.
2. **Silent fail-open:** errors were logged correctly, but there was no alert or KPI on "check skipped". 15 skips over 10 days went unnoticed.
3. **KPI blind spot:** the weekly extract counts "flag set" events, not "check skipped", so the pilot numbers looked plausible.

Impact check (done with support ops lead): of the 15 affected cases, 4 were real duplicates of an open case; for 1 of them (4812) two engineers had started preparation. No refresh was executed twice. No case was linked to a wrong customer.

---

## 4. AI-assisted troubleshooting

AI (Claude) was used as a sparring partner. What it suggested, and what I did with it:

| AI suggestion | Used? | Note |
| --- | --- | --- |
| Start with "did the event arrive at all?" before looking at the rule — split the path into event → auth → query → write-back. | **Yes** | Gave the H1–H4 order. |
| Check for expired OAuth secret / missing scope of the technical user. | Checked, rejected | Reasonable first suspect; evidence (other cases OK) ruled it out. |
| The case API might not support filtering on the custom field system ID. | Checked, rejected | Would have failed for **all** cases; 4815 proved filtering works. |
| Look for IDoc / qRFC errors and partner profiles. | **Not applicable** | Classic ECC integration pattern; F-01 uses events + REST API on BTP. Discarded. |
| "Intermittent + some channels" pattern → compare successful vs failed cases by channel and by field timestamps (race condition). | **Yes** | This led directly to the change-history comparison that confirmed H4. |
| Propose to switch the trigger from "case created" to "case updated". | Partly | Would conflict with PK-REG-02 ("no new flag when the case is edited") and with idempotency — treated as a scope change, not as hotfix (§6). |

AI output was never taken as evidence. Every conclusion above is based on logs, case history and config I checked myself.

---

## 5. Human validation

| Check | What I verified | Result |
| --- | --- | --- |
| CAP application log (BTP) | Entries for 4806, 4812, 4815; all skip/error entries since go-live, grouped by channel | Pattern: all 15 skips = Joule channel, reason `system ID empty` |
| Case change history (Service Cloud v2) | Field-level timestamps on creation vs system ID update for 4806, 4812, 4815 | Joule cases: system ID set 15–30 s after creation; form cases: set at creation |
| Destination & security log | `SC2_CASE_API` config, token calls, 401/403 entries | No auth errors |
| Event subscription | Subscription active, events received and acknowledged | OK |
| CAP configuration | Open status list, feature toggle, retry settings | Status list OK; toggle ON; retry = 3 attempts with ~2 s interval (immediate) |
| SAP Note / KBA search (SAP for Me) | Keywords: Service Cloud v2, Joule, case creation, custom field, event, empty field | No Note found describing the two-step behaviour **[VERIFY — repeat search before closure; raised question to SAP product support]** |
| Event store / KPI extract | "flag set" events vs open-case pairs for week 44–45 | Confirmed 4 missed real duplicates |
| Data protection | Log extracts shared with L3 contain only IDs, codes and timestamps | No case free text shared (US-02 AC 3) |

---

## 6. Final resolution

### Workaround (active from 05.11.2026, 14:00)
- Dispatchers: for pilot cases **created via Joule**, search open pilot cases by customer + system ID before assigning (added to the one-page dispatcher quick guide).
- Support ops lead: reviewed the 15 skipped cases by hand; the 4 real duplicates were handled (case 4812 confirmed as duplicate of 4790, second engineer released).

### Permanent fix (deployed 06.11.2026 through the pipeline with approval — CAP `dupchk-srv` v1.0.4)
1. **Delayed retry for missing rule data:** when the system ID (or account) is empty, the check is re-scheduled at **30 s, 60 s and 120 s** instead of 3 immediate retries. API errors (HTTP 5xx) keep the existing short retry. Intervals are configuration, not hard-coded (same principle as the open status list).
   - Worst case for a Joule case: flag within ~3.5 min. This exceeds the 2-minute target **[ASSUMPTION]** — accepted by support ops lead for Joule cases; target to be confirmed (backlog open item 4).
2. **Idempotency kept:** delayed retries use the original event ID, so a case is still flagged only once (TC-US03-07).
3. **Monitoring:** new "check skipped" event in the BTP event store (codes only) and an alert to the support ops mailbox when skipped checks exceed **3 per day**. The weekly KPI extract now shows *flag set / no match / skipped* separately.

### Not done as hotfix — raised as change request
- Re-running the check on "case updated" when the system ID changes from empty to a value. It changes the agreed behaviour in PK-REG-02 and needs formal change impact analysis (Task B3). Parked until SAP answers the [VERIFY] question on Joule case creation.

---

## 7. Prevention and retest (link to Task A5 — `50_Test_Cases.csv`)

| Retest | Why | Result 06.11.2026 |
| --- | --- | --- |
| **TC-US01-08** Joule case flagged end-to-end | Failing scenario. Re-run in SIT with **production-like event latency (< 2 s)**. | Pass — flagged after 2nd delayed retry (~60 s) |
| **TC-US03-05** System ID empty at check time | Behaviour changed: delayed retries, then error. Expected result updated. | Pass — no flag if system ID stays empty; error after 3rd delayed try; fail-open |
| **TC-US03-06** Case API unavailable | Confirm API-error retry path is unchanged. | Pass |
| **TC-US03-07** Same event delivered twice | Delayed retries must not cause double flags. | Pass |
| **TC-US01-01 / TC-US01-05** Match / no match (form channel) | Regression of the main path. | Pass |
| **TC-US02-07 / TC-US02-08** Events and KPI extract | New "skipped" event; codes only, extract reconciles. | Pass — DPO confirmed no free text |
| **PK-REG-02** No new flag on edit | Fix must not introduce update-trigger behaviour. | Pass |
| **NEW TC-US03-12** Joule two-step creation | Case created with empty system ID, system ID written 20 s later → flagged once, within ~3.5 min. Added to `50_Test_Cases.csv` and to the regression set. | Pass |
| **NEW TC-US03-13** Skip alert | 4 skipped checks in one day → alert mail to support ops. | Pass |

**Prevention summary**
- Test with production-like timing and **per channel** (form, Joule, email if added) — added to SIT entry criteria.
- Every fail-open path must have a visible counter and an alert; "fails safely" must not mean "fails silently".
- Blueprint feedback for `31_Solution_Blueprint.md` §7 E2: system ID can be empty not only when cleared after creation, but also **before it is first populated** (Joule channel).

---

## 8. Customer closure note

> **INC-2026-0117 — resolved**
>
> Some new system copy / refresh cases created through Joule did not get the "Possible duplicate" flag. The cases did reach the duplicate check, but the system ID was added to the case a few seconds after creation, and the check ran before that. The check now waits and tries again, so these cases are flagged — for Joule cases it can take up to about 3–4 minutes instead of 2.
>
> We reviewed all 15 affected cases since go-live: 4 were real duplicates and have been handled; no refresh was run twice and no case was linked to another customer. Skipped checks are now monitored and reported in the weekly extract, so the pilot numbers are complete again.
>
> Your team can stop the manual check for form-created cases now, and for Joule cases from Monday 09.11.2026 after one more day of monitoring. Please reopen this ticket if a matching case arrives without a flag after 5 minutes.
