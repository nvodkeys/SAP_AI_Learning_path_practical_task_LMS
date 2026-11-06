# KB: Duplicate flag missing on system copy / refresh cases created via Joule

**Applies to:** F-01 Duplicate flag (CAP duplicate-rule service on SAP BTP + Service Cloud v2) · **Category:** Integration · **Source incident:** Incident bank #3 · **Fixed in:** `dupchk-srv` v1.0.4

---

## Symptom
- A new pilot case (system copy / refresh) matches an open case of the same customer and system ID, but **Possible duplicate of** stays empty.
- Intermittent: other cases from the same customer are flagged correctly.
- No error visible to dispatchers; the case follows the normal flow.

## Cause
Cases created through **Joule** get the system ID in a second update a few seconds after creation. The duplicate check runs on the "case created" event, finds the system ID empty and skips the case (fail-open, by design). In version 1.0.3 the retries were immediate (~6 s total), so the data was still missing on every try. Skips were logged but not alerted.

## How to confirm
1. CAP application log for the case ID → entry `system ID empty`.
2. Case change history → system ID written 15–30 s **after** the creation timestamp, by the Joule channel.
3. If instead you see HTTP 401/403 → see "Expired OAuth credentials" (TC-US02-09), not this article.

## Resolution
Version 1.0.4: when rule data is missing, the check retries at **30 s / 60 s / 120 s** (configurable). Joule cases are flagged within ~3.5 min. Skipped checks are logged as a separate event, alerted above 3 per day and shown in the weekly KPI extract.

## Workaround (if the fix is not active or the alert fires)
Before assigning a pilot case created via Joule, search open pilot cases by **customer + system ID**. If a match exists, handle it as a duplicate manually (reason *Duplicate*, link the existing case).

## Retest after any change to F-01 (`50_Test_Cases.csv`)
TC-US01-08 (run with production-like event latency) · TC-US03-05 · TC-US03-06 · TC-US03-07 · TC-US03-12 · TC-US03-13 · PK-REG-02

## Related
- Blueprint §7 E2 — system ID can be empty before first population, not only after clearing.
- Open: SAP confirmation of Joule two-step case creation **[VERIFY]**; re-check on "case updated" is a change request, not part of this fix.
