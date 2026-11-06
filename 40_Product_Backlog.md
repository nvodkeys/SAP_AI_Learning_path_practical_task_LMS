# Feature & User Stories — RISE Service-Request Intake & Routing (Pilot)

**[VERIFY]** = confirm on help.sap.com / SAP Business Accelerator Hub before sign-off · **[ASSUMPTION]** = our working assumption · **[FICTIONAL]** = inherited from the RFP

---

## 1. Feature (scoped process enhancement)

### F-01 — Duplicate flag for open system copy / refresh requests

**Short description**
When a customer creates a system copy / refresh case **[FICTIONAL category]**, a small service on SAP BTP checks whether the same customer already has an **open** case for the **same system ID** and the same category. If it finds one, the new case gets a "Possible duplicate" flag and a link to the existing case. The dispatcher sees the flag and decides. The customer is never blocked, nothing is closed automatically, and no message goes to the customer without a human.

**Why this feature first**
It is the one requirement where SAP standard does not fit (Similar Case Recommendation answers a different question), so it carries the most build risk. Proving it early tells us quickly whether the BTP extension pattern works in SAP's internal tenant.

**In scope:** pilot category only · rule-based check (customer + system ID + category + open status) · flag and link on the case · dispatcher confirm / reject · duplicate events in the KPI extract.
**Out of scope:** pre-submit warning inside Joule (see §2) · automatic closing or merging · semantic / AI duplicate detection · other categories.

**Blueprint links:** §1.1 step P3 · §2 row R3 · §3 I2, I4, I5 · §4.2 SoD theme 5 · §5.3 · §6 supporting fields · §7 E1, E3 · §9 row "CAP rule on BTP"

---

## 2. Feasibility check against SAP standard

| Need | What SAP standard offers | Verdict |
| --- | --- | --- |
| Find open cases of the same customer + system | Similar Case Recommendation searches **past categorised** cases (last 12 months, top 3) for solutions | **Not a fit** → CAP rule on BTP (E3). Keep similar cases as a hint only |
| Show the flag and link on the case | Custom fields via Service Cloud v2 Extensibility Administration (E1) | **Configuration** — feasible |
| Start the check when a case is created | Service Cloud v2 outbound events / notifications **[VERIFY]** | Feasible if events exist. Fallback: short scheduled poll — breaks the "no polling" principle (§3), needs architect approval |
| Query open cases by account, system ID and status | Case service API **[VERIFY]** — filtering on a *custom* field (system ID) is the main technical risk | Feasible only if filter is supported; else filter by account + status and match system ID in CAP |
| Write the flag back to the case | Case API update of custom fields **[VERIFY]** | Feasible if custom fields are exposed in the API |
| Warn the customer **before** submitting in Joule | Joule in SAP for Me UI cannot be changed (E6). A Joule Studio skill is an option (proposal §3.2) but not baseline | **Not feasible in baseline.** Flag goes to the dispatcher; the dispatcher contacts the customer |
| Dispatcher confirms / rejects the flag with a reason | Code list + custom field + validation (same pattern as override reason, R5) | **Configuration / in-app extension** — feasible |
| Customer never sees another customer's case | Rule filters by account; flag fields hidden in self-service **[VERIFY field visibility]** | Feasible |

**Correction to feed back into `31_Solution_Blueprint.md`:** §1.1 P3 output says "Warning to customer". Because the check runs *after* the case is created and the Joule UI is off-limits, change it to "Flag + link for the dispatcher; customer contacted by dispatcher if confirmed".

---

## 3. Duplicates removed

| Repeated item | Where it appeared | Kept as |
| --- | --- | --- |
| Duplicate rule definition | Proposal §3.1 gap note, blueprint §2 R3, §7 E3, §9 | Defined **once**, in US-03 acceptance criteria |
| "Warn, don't block" | Proposal §7 R7, blueprint §8 SIT negative cases | One acceptance criterion in US-01 |
| Customer isolation | Blueprint §4.1, §4.2 theme 5 | One acceptance criterion in US-02 |
| KPI event logging | Blueprint §3 I5, §6, §7 E4 | Only the duplicate events here (US-02); all other KPI events belong to a separate KPI feature |
| Mandatory intake fields | Proposal §1, blueprint §2 R2 | **Removed** from this feature — it is configuration (dynamic properties), not part of F-01. Only the edge case "system ID cleared" (E2) is handled in US-03 |

---

## 4. Priority order

| Priority | Story | Type | MoSCoW | Reason |
| --- | --- | --- | --- | --- |
| 1 | US-02 | Integration / security / reporting | Must | Blocker: without API access, technical user and events nothing else can run. Depends on access model (proposal §6.2 Q1) |
| 2 | US-03 | Development | Must | The rule itself; needs US-02 in place |
| 3 | US-01 | Business process | Must | Delivers the value to dispatchers; tested in UAT once US-02 and US-03 work |

Value order is US-01 first; **build order** is the table above.

---

## 5. User stories

### US-01 — Business process: dispatcher handles a possible duplicate

**Blueprint link:** §1.1 P3 + P5 · §2 R3 · §7 E1

**As a** support dispatcher
**I want** a new system copy / refresh case that matches an open case of the same customer and system to be flagged "Possible duplicate" with a link to that case
**So that** I can merge the work into the existing request instead of sending two engineers to do the same job

**Acceptance criteria:**
- When a new pilot case has the same customer, system ID and category as an open case, the case in my queue shows "Possible duplicate" and the existing case ID before I start working on it (target: within 2 minutes of creation **[ASSUMPTION]**).
- I can either **confirm** (close the new case with reason "Duplicate", linked to the existing case) or **reject** (reason "Not a duplicate — new need"). The flag never blocks the customer and never closes a case by itself.
- A case with no match shows no flag and follows the normal dispatcher flow without any delay.

**Definition of done:**
- Custom fields "Possible duplicate of" and "Duplicate decision" plus the two reason codes are configured in Service Cloud v2 and transported to the pilot tenant.
- The dispatcher quick guide (one page) explains the flag and the two choices, and the 6 pilot dispatchers have seen it.
- UAT scenarios "match", "no match" and "flag rejected" are signed off by the support ops lead (blueprint §8).

---

### US-02 — Integration / security / reporting: safe access and measurable results

**Blueprint link:** §3 I2, I4, I5 · §4.1 technical users · §4.2 theme 5 · §5.3 · §6

**As a** support operations lead
**I want** the duplicate check to read and write cases only through released APIs with a least-privilege technical user, and to log every flag and dispatcher decision as an event
**So that** customer data stays protected and we can prove after 8 weeks whether the flag actually saves work

**Acceptance criteria:**
- The technical user can only read open pilot cases and write the two duplicate fields; any other read or write is denied (proven in the security test).
- The flag only ever points to a case of the **same** customer; customers in self-service cannot see the flag fields or another customer's case ID.
- For every flagged case, the events "flag set" and "dispatcher decision (confirmed / rejected)" are stored with case ID, codes and timestamps only — **no case free text** — and appear in the weekly extract.

**Definition of done:**
- The API filter, event and custom-field exposure items in §2 are checked on SAP Business Accelerator Hub / help.sap.com and the reviewer box in blueprint §10 is ticked.
- OAuth client credentials are stored in a BTP destination (no secrets in code); the technical user is reviewed by SAP security and the SoD matrix is clean.
- On the SIT sample, the extract reconciles 1:1 with case history, and the DPO confirms that no free text is stored on BTP.

---

### US-03 — Development: CAP duplicate-rule service

**Blueprint link:** §2 R3 · §7 E2, E3 · §8 unit test row · §9 row "CAP rule on BTP"

**As a** BTP / CAP developer
**I want** to build a CAP service that runs on the "case created" event, applies the duplicate rule and writes the result back to the case
**So that** the flag is exact, easy to explain, and easy to remove if SAP's agentic case resolution ships the same function (risk R1)

**Acceptance criteria:**
- A match means: same customer account **and** same system ID **and** same pilot category **and** status in the "open" status list **and** not the same case. The open status list is configuration, not hard-coded. If several cases match, the oldest open case is linked and the number of matches is noted.
- If the system ID is empty (it can be cleared after creation, E2) or the case API is unavailable, no flag is set, the call is retried up to 3 times, then an error is logged — the case still reaches the dispatcher normally (fail-open).
- If the same event arrives twice, the case is flagged only once.

**Definition of done:**
- Unit tests cover match, no match, several matches, empty system ID, API failure / retry and repeated event; tests confirm no case free text is persisted.
- Deployed through the pipeline with approval; developers have no access to production case data (SoD theme 4).
- A feature toggle switches the check off without a transport, and the steps to remove the service completely are documented.

---

## 6. Open items before Realize

1. Service Cloud v2: can the case API filter on a custom field (system ID)? **[VERIFY]**
2. Service Cloud v2: which outbound event fires on case creation, and can BTP subscribe to it? **[VERIFY]**
3. Confirm the list of "open" case statuses with support ops.
4. Confirm the 2-minute flag target with dispatchers **[ASSUMPTION]**.
5. Access model (proposal §6.2 Q1) — still the gate for US-02.
