# SAP Change Request — RISE Service-Request Intake & Routing

**Request in one line:** add structured intake and AI-assisted classification for **one** RISE service-request category, so the request reaches the right resolving team first time.

**Traceability to A1:** process = A1 §D (support case triage and routing); improvement = A1 §E; evidence = A1 §B1, §B2; open questions = A1 §H.

## 1. Business problem

SAP's customer satisfaction is below target, and slow RISE service requests are part of the reason. Customer NPS fell to 9 in 2025 against a 12–16 target. From 2026 SAP measures Cloud CSAT instead (2025 baseline 75%, 2026 target 75–76%). Gartner reports RISE service levels below industry norm and SAP-side processes delaying infrastructure tasks (A1 §B1, §B2).

SAP wants service requests that arrive complete, are not duplicated, and land with the right team on the first try. It wants this as a small extension of its own Joule / agentic case resolution agenda, not a new support platform.

## 2. Current pain points

Pilot category: **RISE system copy / refresh requests** **\[FICTIONAL choice\]** — A1 §H says the real category must be picked from Cloud CSAT detractor data.

| Pain point | Effect today | Basis |
| --- | --- | --- |
| Requests arrive without system ID, component, target date or approvals | Request goes back to the customer; clock restarts | \[ASSUMPTION\] |
| Request lands in the wrong queue | Re-assignments; slower first qualified response | \[ASSUMPTION\] (A1 §F) |
| Customer opens the same request twice | Two teams work one need | \[ASSUMPTION\] |
| Unclear if it is a standard catalogue task or a change request | Commercial approval found late; work stalls | \[ASSUMPTION\] |
| Some RISE service tasks take over a month | Low satisfaction for RISE customers | Gartner client anecdote (A1 §B2) |

Illustrative baseline **\[FICTIONAL\]**: \~30% of requests returned for missing data, \~1.8 re-assignments per request. Real baselines must come from SAP.

## 3. In scope

One feature: **smart intake and routing for the pilot request category**, from request creation to assignment.

1. **Structured intake** in Joule in SAP for Me: mandatory fields for the category (system ID, component, request type, target date, approver).
2. **Duplicate check** against open requests from the same customer and system, using the standard similar-case capability.
3. **AI classification**: suggests the resolving team and flags *standard catalogue task* vs *change request needing commercial approval*.
4. **Human confirmation**: the dispatcher accepts or overrides the suggestion; override reason is logged.
5. **KPI logging** of the four success measures (section 8) as a simple data extract.

Language: English only **\[FICTIONAL\]**.

## 4. Out of scope

- Any other service-request category, and all incidents
- Automatic resolution, auto-replies or AI-drafted answers to customers
- The commercial approval workflow itself (we only flag; approval stays as today)
- Pricing, billing, credits, subscription conversion (A1 §C2)
- Contract changes from the EU commitments (A1 §C3 — legally sensitive)
- Changes to standard SAP for Me, Joule or case-management code (clean core)
- New SAC dashboards or CSAT survey changes
- Migration or re-classification of historic cases
- Rollout beyond the pilot group

## 5. SAP landscape

| System | Role in this request | Status |
| --- | --- | --- |
| SAP for Me + Joule (agentic case resolution) | Customer entry point; analyses cases, detects duplicates, suggests routing | Fact (SAP News, Jun 2026) |
| SAP Service Cloud v2 (Joule case classification, similar-case recommendation) | Internal case management behind SAP for Me | \[ASSUMPTION\] — validate in kickoff |
| ECS service-request tooling | Where resolving teams work the request | \[ASSUMPTION\] — system unknown |
| SAP BTP (incl. SAP AI Core / Generative AI Hub) | Side-by-side extension and classification logic | Target platform; AI Hub use \[ASSUMPTION\] |
| SAP Cloud Identity Services | Single sign-on for agents and customers | \[ASSUMPTION\] |
| SAP corporate ERP (S/4HANA, run via RISE) | Not touched | Fact (A1 §A) |

## 6. Integrations

| # | From → To | Data | Method | Status |
| --- | --- | --- | --- | --- |
| I1 | SAP for Me / Joule → BTP extension | New request + structured intake fields | Event or REST API | \[ASSUMPTION\] |
| I2 | BTP extension → case management | Duplicate lookup (similar cases) | Standard API | \[ASSUMPTION\] |
| I3 | BTP extension → AI Core / Generative AI Hub | Request text + fields → team + standard/CR flag | REST | \[ASSUMPTION\] |
| I4 | BTP extension → case management | Suggested assignment; dispatcher decision back | OData / REST | \[ASSUMPTION\] |
| I5 | BTP extension → KPI extract | Routing events, timestamps, overrides | Scheduled file / table | \[FICTIONAL\] |

Principle: reuse standard Joule and case-management interfaces first; build only what fit-to-standard shows is missing.

## 7. Users

| User | What they do with the solution | Pilot size \[FICTIONAL\] |
| --- | --- | --- |
| RISE customer (basis / system admin) | Raises the request through Joule in SAP for Me; fills structured intake | \~50 customers |
| SAP support dispatcher | Reviews AI suggestion, accepts or overrides | 6 dispatchers |
| ECS resolving engineer | Receives correctly assigned, complete requests | 2 teams |
| Support operations lead | Reads KPI extract; decides on rollout | 1–2 people |
| BTP / support IT admin | Runs and monitors the extension | 1–2 people |

## 8. Success criteria

Measured over the first 8 weeks after go-live, against an SAP baseline taken before build. All targets are hypotheses **\[FICTIONAL\]** until SAP provides baselines (A1 §E, §H).

| # | Measure | Target |
| --- | --- | --- |
| SC1 | Requests routed to the correct team first time | ≥ 85% |
| SC2 | Average re-assignments per request | ≤ 1.2 |
| SC3 | Requests returned to customer for missing information | ≤ 10% |
| SC4 | Time from creation to first qualified response | −30% vs baseline |

## 9. Risks

| Risk | Impact | Mitigation |
| --- | --- | --- |
| SAP's own Joule / agentic roadmap already covers this | Duplicate build, wasted spend | Fit-to-standard first; build only the gap |
| No baseline data for routing and re-work | Success cannot be proven | Baseline measurement is a gate before Realize |
| Customer case data is personal / confidential (GDPR) | Pilot blocked or delayed | Anonymised or synthetic test data; DPO sign-off |
| AI suggests the wrong team | Slower handling, low trust | Human confirmation step; override reasons reviewed weekly |
| Internal case system differs from assumption | Integration redesign | Confirm systems in Prepare phase |
| Scope creep to more categories or auto-replies | Timeline slips | Change control; out-of-scope list in section 4 |

## 10. Timeline assumptions

About 10 weeks to go-live, then 8 weeks of measured pilot. Dates are **\[FICTIONAL\]**, assuming a January 2027 start.

1. **Prepare** — 2 weeks: confirm systems, data access, baseline extract.
2. **Explore** — 3 weeks: fit-to-standard workshops; backlog of gaps only.
3. **Realize** — 4 weeks (2 sprints): intake fields, classification, dispatcher step, KPI extract; testing with anonymised data.
4. **Deploy** — 1 week: pilot go-live for one category and pilot users.
5. **Run** — 8 weeks: hypercare and KPI measurement; go / no-go for wider rollout.

## 11. Delivery expectation

SAP expects delivery with **SAP Activate** and a **fit-to-standard** approach.

- Run fit-to-standard on Joule in SAP for Me, agentic case resolution and Service Cloud v2 classification **before** any custom design.
- Configure standard first; extend only on SAP BTP, side-by-side, with no modifications (clean core).
- Each gap gets a documented decision: accept standard, configure, or extend.
- Deliverables per phase: Prepare — project plan and baseline; Explore — fit-gap backlog; Realize — tested extension; Deploy — cutover and pilot sign-off; Run — KPI report and rollout recommendation.
- Vendor proposals should state which SAP standard capabilities they reuse and which they build.

**Open questions for kickoff (A1 §H):** which case system sits behind SAP for Me; baseline figures for SC1–SC4; which category drives Cloud CSAT detractors; data-access rules for the pilot.
