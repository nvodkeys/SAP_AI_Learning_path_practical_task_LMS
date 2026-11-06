# Production Incident — F-01 Duplicate Flag (RISE Service-Request Intake & Routing, Pilot)

**Incident bank #: 3 — Integration failure**
Bank symptom: *"The document did not reach the downstream system."*
Adapted to this process: the new case did not reach the duplicate check on SAP BTP (the "downstream system" for F-01), so no flag came back to the case.

| Field | Value |
| --- | --- |
| Ticket ID | INC-2026-0117 *(synthetic)* |
| Reported | Thu 05.11.2026, 10:40 CET |
| Reported by | Support operations lead, RISE pilot (on behalf of the 6 pilot dispatchers) |
| Feature | F-01 — Duplicate flag for open system copy / refresh requests (US-01, US-02, US-03) |
| Status | New → assigned to L2 Application Support |
| Go-live of F-01 | Mon 26.10.2026 (pilot tenant) |

> Root cause is **not** part of this ticket. It is worked out in `61_Incident_Resolution.md`.

---

## 1. Problem description (customer language)

> "The new case did not reach the duplicate check. This morning a customer opened a system refresh case for their production system, and we already had an open refresh case for the same customer and the same system. The new case came into our queue **without** the 'Possible duplicate' flag. Two of my dispatchers assigned the two cases to two different engineers, and both engineers started preparing the same refresh.
>
> It does not happen every time — yesterday another case was flagged correctly. I can't see what is different. Since we can't trust the flag any more, my team is now checking every pilot case by hand, which is exactly the work the feature was supposed to remove."

---

## 2. Severity

**Priority: P3 — Medium** (upgrade to **P2** if a duplicate refresh is actually executed on a customer system, or if the share of missed flags rises above ~25 % of pilot cases)

| Impact area | Assessment |
| --- | --- |
| Customers | Not blocked. Cases are created and reach the queue normally. No wrong data shown to customers. |
| Support operations | Duplicates are missed **silently** → two engineers can be assigned to the same refresh (wasted effort, several hours per system copy). Risk of conflicting work on the same customer system if both engineers proceed. |
| Pilot evaluation | The weekly KPI extract under-counts duplicates → the 8-week pilot decision (does the flag save work?) is based on wrong numbers. |
| Data protection / security | No indication of data exposure: the problem is a *missing* flag, not a wrong link to another customer. |
| Workaround | Manual: dispatchers search open pilot cases by customer + system ID before assigning. Works, but costs ~3–5 min per case. |
| Users affected | 6 pilot dispatchers; all pilot-category cases since go-live potentially affected. |

---

## 3. Reproduction steps

Reproduced by the reporter on the pilot tenant; **intermittent** (failed on 2 of 3 examples provided).

1. Make sure an **open** pilot case exists: customer ACME GmbH (1000123), system ID `PRD`, category *System copy / refresh*, status *In Process* (example: case **4790**).
2. As a customer user of ACME, create a new case with category *System copy / refresh* and system ID `PRD` (example: case **4812**, created 05.11.2026 09:14).
3. As a dispatcher, open the dispatcher queue and wait at least 2 minutes after creation.
4. Open the new case and look at the fields **Possible duplicate of** and **Duplicate decision**.

Examples provided by the reporter:

| New case | Created | Channel (from case header) | Open case same customer + system | Flag shown? |
| --- | --- | --- | --- | --- |
| 4812 | 05.11. 09:14 | Joule (SAP for Me) | 4790 | **No** |
| 4815 | 05.11. 09:52 | SAP for Me form | 4790 | Yes → 4790 |
| 4806 | 04.11. 16:03 | Joule (SAP for Me) | 4790 | **No** |

---

## 4. Expected vs actual

| | Result |
| --- | --- |
| **Expected** (US-01 AC 1, TC-US01-01 / TC-US01-08) | New case 4812 shows *Possible duplicate of* = 4790 within 2 minutes of creation **[ASSUMPTION target]**; dispatcher confirms or rejects. |
| **Actual** | Case 4812 shows no flag; *Possible duplicate of* is empty even 60+ minutes after creation. No error is visible to the dispatcher. Case otherwise behaves normally. |

---

## 5. System context (synthetic only)

| Item | Value *(synthetic)* |
| --- | --- |
| Service Cloud v2 tenant | `SC2-PILOT-EU` (pilot / production) |
| Customer channel | SAP for Me (form + Joule) |
| BTP subaccount | `rise-dupchk-prod` (region eu10) |
| CAP application | `dupchk-srv` v1.0.3, feature toggle `DUPLICATE_CHECK_ENABLED = true` |
| Destination | `SC2_CASE_API` (OAuth2ClientCredentials, technical user `TU_DUPCHK`) |
| Trigger | "Case created" outbound event → CAP service **[VERIFY event name]** |
| Test customer in examples | ACME GmbH, account 1000123 *(test data from `50_Test_Cases.csv`)* |
| Last change in pilot tenant | None since go-live 26.10.2026 (per reporter) |

---

## 6. Attachments note (evidence to attach)

- Screenshots: case 4812 and 4815 in dispatcher view (duplicate fields visible / empty).
- Case change history export for 4812, 4815, 4806 and 4790 (field changes with timestamps and user).
- CAP application log extract for `dupchk-srv`, 04.11.–05.11.2026, filtered by case ID (no case free text).
- BTP event store query: "flag set" events for 04.11.–05.11.
- Weekly KPI extract of week 45 (for impact on pilot numbers).
- Names of the two engineers' assignments on 4790 / 4812 (to judge duplicate work done).
