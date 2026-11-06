# Solution Blueprint — RISE Service-Request Intake & Routing (Pilot)

**[VERIFY]** = confirm on help.sap.com before sign-off · **[ASSUMPTION]** · **[FICTIONAL]** = inherited from the RFP

---

## 1. Process overview

### 1.1 End-to-end flow (in scope)

| # | Step | Actor | System | Output |
| --- | --- | --- | --- | --- |
| P1 | Raise system copy / refresh request **[FICTIONAL category]** | RISE customer (basis admin) | Joule in SAP for Me | Draft request |
| P2 | Capture mandatory intake fields: system ID, component, request type, target date, approver | Customer | SAP for Me → case (Service Cloud v2 **[ASSUMPTION]**) | Complete case |
| P3 | Duplicate check: open case, same customer + system ID + category | System | BTP extension + Similar Case Recommendation (hint) | Warning to customer / link to existing case |
| P4 | Classification: resolving team + *standard catalogue task* vs *change request* | System | Case Categorization → routing rules | Proposed category + team |
| P5 | Dispatcher review: accept or override (+ reason code) | SAP support dispatcher | Service Cloud v2 | Confirmed assignment |
| P6 | Hand-over to resolving team | ECS resolving engineer | Service Cloud v2 team queue | Work starts |
| P7 | KPI event logging | System | BTP extension → extract | SC1–SC4 data |

### 1.2 Out of scope (from RFP §4)

Other categories and all incidents · auto-resolution, auto-replies, AI-drafted customer answers · the commercial approval workflow (flag only) · pricing, billing, credits, subscription conversion · EU-commitment contract changes · any modification of SAP for Me, Joule or case-management code · new SAC dashboards, CSAT survey changes · historic case migration or re-classification · rollout beyond pilot group · non-English requests.

### 1.3 Process boundary

Starts: customer opens Joule in SAP for Me. Ends: case sits in the correct ECS team queue with a dispatcher-confirmed assignment. Everything after (resolution, approval, closure) is unchanged.

---

## 2. Fit-to-standard vs configuration vs extension vs BTP

Classification after checking SAP Learning and SAP KBAs (see §10). Final decision per item is an Explore-phase deliverable.

| Requirement (RFP §3) | Standard (use as is) | Configuration | In-app extension (Service Cloud v2 Extensibility Administration) | BTP side-by-side |
| --- | --- | --- | --- | --- |
| R1 Conversational intake | Joule in SAP for Me | Case type for pilot category; self-service flag **[VERIFY]** | — | — |
| R2 Mandatory fields | — | **Dynamic properties per case type: "Mark as Required"** | Custom fields if system ID / approver don't exist as standard fields; validation to stop clearing fields after creation | — |
| R3 Duplicate check (open cases, same SID) | Similar Case Recommendation as *hint only* | — | — | **CAP service: deterministic rule** (customer + SID + category + open status) |
| R4a Team suggestion | Case Categorization (AI) | Category catalogue; **Case Routing to Team** rules on category | — | — |
| R4b Standard task vs CR flag | Case Categorization | Encode flag as a category level in the catalogue | — | generative AI hub **only if** catalogue approach fails in Explore |
| R5 Dispatcher accept / override | Team queue assignment | Override reason as code list | Custom field "override reason" + validation (mandatory on team change) | — |
| R6 KPI logging | — | — | — | **CAP + SAP HANA Cloud event table, scheduled extract** |

**Net change from the proposal:** mandatory fields and the standard/CR flag move from "probably BTP" to **configuration**. The BTP footprint shrinks to two items: the duplicate rule and the KPI extract.

---

## 3. Integrations

| # | From → To | Data | Blueprint decision | Status |
| --- | --- | --- | --- | --- |
| I1 | SAP for Me / Joule → case management | New request + intake fields | **Standard** — Joule creates the case; no extension in the path. BTP is notified by case event | SAP for Me ↔ case-system link **[VERIFY]** (SAP-internal) |
| I2 | BTP extension → case management | Query open cases (customer, SID, category, status) | Case service API via SAP Integration Suite or direct destination | API name and filter support **[VERIFY]** on SAP Business Accelerator Hub |
| I3 | BTP → SAP AI Core / generative AI hub | Request text + fields → flag | **Not built in baseline.** Contingency only (R4b); if used, orchestration with data masking on | Contingency |
| I4 | BTP → case management | Duplicate warning / note on case; dispatcher decision read back | Case service API (write note / custom field); dispatcher decision read from case history | **[VERIFY]** |
| I5 | BTP → KPI extract | Events: created, intake complete, AI proposal, confidence, dispatcher decision, override reason, re-assignments, first qualified response | Event-driven capture into HANA Cloud table; weekly CSV extract | **[FICTIONAL]** format |

**Integration principles:** event-first (case created / updated) → no polling · one technical user per direction, OAuth client credentials · no case free text stored on BTP (IDs, codes and timestamps only) · all calls logged in SAP Cloud ALM / BTP audit log.

---

## 4. Security and roles

### 4.1 Business roles (concept level)

| Role (concept name) | System | Can do | Cannot do |
| --- | --- | --- | --- |
| `Z_RISE_CUSTOMER_SELFSERVICE` | SAP for Me / self-service | Create and view **own company's** cases for pilot case type | See other customers' cases; change team or category |
| `Z_SUPPORT_DISPATCHER` | Service Cloud v2 | View pilot queue; accept / override team; enter override reason | Change routing rules, catalogue, model settings |
| `Z_ECS_RESOLVER` | Service Cloud v2 | Work cases in own team queue; return case to dispatcher | Re-route to another team directly **[ASSUMPTION — confirm process]** |
| `Z_SUPPORT_OPS_LEAD` | KPI extract | Read KPI extract | Edit cases, events or configuration |
| `Z_SVC_CONFIG_ADMIN` | Service Cloud v2 | Case types, dynamic properties, routing rules, catalogue | Process cases as dispatcher |
| `Z_AI_MODEL_ADMIN` | Service Cloud v2 AI settings | Train / activate Case Categorization, set confidence threshold | Change routing rules; read KPI extract alone (paired review) |
| `Z_BTP_EXT_ADMIN` | SAP BTP | Deploy and monitor extension, manage destinations | Read case content; process cases |
| Technical users | BTP ↔ case system | Read open cases, write note / field, receive events | Anything else (least privilege) |

Service Cloud v2 business roles are built from business services with read/write access levels and restriction rules (e.g. "Myself"); the customer role uses a restriction to own account **[VERIFY]**.

### 4.2 Segregation of duties (SoD) themes

1. **Configure vs operate** — whoever changes routing rules or the catalogue must not dispatch cases.
2. **Tune vs measure** — the person who sets model thresholds should not be the only person reporting SC1–SC4 (avoids grading own work).
3. **Decide vs audit** — dispatchers cannot edit or delete KPI events; events are append-only.
4. **Build vs run** — BTP developers have no production case access; production deploy via transport/pipeline with approval.
5. **Customer isolation** — no cross-customer visibility in self-service or in duplicate warnings (warning shows only the customer's own case ID).

Identity: SAP Cloud Identity Services for SSO; named users only; quarterly role review during pilot.

---

## 5. Data considerations

### 5.1 Master and configuration data touched

| Object | Change | Owner |
| --- | --- | --- |
| Account (RISE customer) + contacts | Read only — must exist and be linked to SAP for Me users | SAP support ops |
| System ID / installed base / registered product | Read only — needed for intake and duplicate rule; source system **[ASSUMPTION]** | SAP support ops |
| Case type for pilot category | **New** | Config admin |
| Category catalogue (team + standard/CR levels) | **New / extended** | Config admin + ECS leads |
| RISE service catalogue (standard tasks list) | Read — basis for standard/CR flag; must be current | ECS service owner |
| Service and support teams | Read — 2 pilot teams + dispatcher team | Support ops |
| Routing rules | **New** for pilot category | Config admin |
| Override reason code list | **New** (~6 codes) | Support ops lead |
| KPI event table (BTP) | **New** | BTP admin |

### 5.2 Migration

**None.** RFP §4 excludes migration and re-classification of historic cases. Historic cases are **read-only training input** for Case Categorization; they are not changed.

### 5.3 Data protection

- Case text may contain personal data and customer-confidential system details → DPO sign-off before any training on real data (gate in Prepare).
- Build and SIT on anonymised or synthetic data; production data only from go-live.
- BTP stores IDs, codes, timestamps — **no case free text**.
- If generative AI hub contingency is activated: orchestration data masking (anonymisation/pseudonymisation) mandatory, model region approved by DPO.
- Retention of KPI events: pilot duration + 6 months **[ASSUMPTION]**, then delete.

---

## 6. Reporting

One extract, four measures, no dashboard (SAC is out of scope).

| KPI | Definition (blueprint) | Data source |
| --- | --- | --- |
| SC1 First-time-right routing | % of pilot cases where first confirmed team = team that resolved the case | Events: first assignment, final team |
| SC2 Re-assignments | Avg. number of team changes after first confirmed assignment | Case team-change history |
| SC3 Returned for missing info | % of cases set to "customer action — information missing" (status/reason code) | Case status history |
| SC4 Time to first qualified response | Creation → first resolver response that is not a request for missing info | Timestamps; response type flag **[VERIFY definition with SAP]** |

Supporting fields (for override review, not KPIs): AI confidence, accepted/overridden, override reason, duplicate warning shown / accepted.

Cadence: weekly extract to support ops lead during Run; final 8-week report for go/no-go. Baseline uses the **same definitions** on pre-pilot data (Prepare gate).

---

## 7. Extensibility decisions

| # | Gap | Decision | Where | Clean-core note |
| --- | --- | --- | --- | --- |
| E1 | Fields missing on case (system ID, approver, override reason) | **Custom fields** via adaptation | Service Cloud v2 in-app | Released extensibility, no modification |
| E2 | Mandatory fields can be cleared after creation / via inline edit | **Validation** rule on case entity | Service Cloud v2 in-app | Known behaviour per SAP KBAs — test explicitly |
| E3 | Duplicate check against open cases | **CAP service** on BTP, event-triggered, writes note/flag back | BTP side-by-side | Uses public API only |
| E4 | KPI event capture and extract | **CAP + SAP HANA Cloud** | BTP side-by-side | No change to standard |
| E5 | Standard/CR flag | **Configuration first** (catalogue level); generative AI hub only if Explore proves catalogue + Case Categorization insufficient | Config / contingency BTP | Contingency must pass DPO |
| E6 | Changes to Joule in SAP for Me UI | **Not allowed** | — | RFP §4 clean-core |

---

## 8. Testing approach (headline)

| Level | What | Data | Who | Exit criterion |
| --- | --- | --- | --- | --- |
| Unit | CAP duplicate rule, event capture, extract | Synthetic | Vendor developers | All rules covered; no case free text persisted |
| Model evaluation | Case Categorization on hold-out historic cases | Anonymised historic | AI model admin + vendor | Agreed accuracy at chosen threshold **[target set in Explore]**; low-confidence → dispatcher queue |
| SIT | P1–P7 end-to-end across I1–I5, incl. negative cases: missing field, duplicate false positive, low confidence, override without reason, API failure / retry | Synthetic / anonymised | Vendor + SAP support IT | No open priority-1/2 defects; events reconcile with case history |
| Security test | Role checks per §4, customer isolation, SoD conflicts | Test users | Vendor + SAP security | No cross-customer visibility; SoD matrix clean |
| UAT | Real pilot scenarios (6 dispatchers, 2 ECS teams, sample customers) | Anonymised | SAP business users | Sign-off by support ops lead |
| Regression | Standard SAP for Me / Joule case creation for **non-pilot** categories unchanged | Synthetic | SAP support IT | No change in behaviour outside pilot |

Test management and defect tracking in SAP Cloud ALM.

---

## 9. SAP capability reasoning

| Capability | Why selected | Alternative considered | Why not selected |
| --- | --- | --- | --- |
| Service Cloud v2 **dynamic properties ("Mark as Required") per case type** | Covers all five RFP mandatory fields as configuration for one case type; zero code; matches fit-to-standard | BTP CAP validation service called from intake | Adds integration I1 logic and a runtime for five field checks; standard already enforces it per case type |
| **Case Categorization** + catalogue level for standard/CR flag | Trained on SAP's own past cases; output drives standard Case Routing to Team rules; dispatcher still decides | LLM classifier via generative AI hub on BTP | Extra runtime, prompt governance, case text leaves case system (GDPR review); not justified for one category in a 4-week Realize |
| **CAP rule on BTP** for duplicate check | RFP asks for *open* cases from *same customer + system*; exact rule is explainable to dispatchers and customers | Similar Case Recommendation alone | It searches *past categorised* cases (last 12 months, top 3) for solutions — wrong question for open-duplicate detection; kept as a hint only |
| **CAP + HANA Cloud event table, CSV extract** for KPIs | RFP asks for "simple data extract"; 1–2 readers; append-only events support SoD | SAP Analytics Cloud dashboard | Explicitly out of scope (RFP §4); a dashboard adds licence and build effort for an 8-week pilot |

---

## 10. Validation of AI-generated content

Sources were checked on 6 Nov 2026. **Note:** direct help.sap.com pages did not return in search; the checks below use SAP Learning, SAP KBAs (support.sap.com) and sap.com product pages. Each row still needs a **human reviewer to open the help.sap.com page** and tick the last column.

| AI-generated content (topic) | Human validation performed (source checked) | Result | Reviewer ✓ |
| --- | --- | --- | --- |
| Product name "SAP Service Cloud Version 2, Case Categorization"; needs AI Units | sap.com product page (Discovery listing) | Confirmed — learns from past cases + catalogue; auto-fills category above confidence threshold; AI Units required | ☐ help.sap.com Service Cloud v2 → Intelligent Service |
| Similar Case Recommendation can do duplicate check (RFP §3.2) | sap.com product page | **Corrected** — scans categorised cases of last 12 months, returns top 3 for solutions → not a duplicate check on open cases | ☐ |
| Routing to team is configuration | SAP Learning "Configuring Case Routing" | Confirmed — admin path Settings → Cases → Case Routing to Team; attributes combinable | ☐ |
| Mandatory fields per case type are configuration | SAP KBA 3587077; KBA 3584803 (category levels) | Confirmed — Extensibility Administration → Case → dynamic properties "Mark as Required" by case type | ☐ |
| Mandatory fields stay mandatory after creation | SAP KBAs 3744700, 3706324 | **Corrected** — fields can be cleared after creation / via inline edit; validation or UI restriction needed → E2 | ☐ |
| Case Type Determination could set pilot category automatically | sap.com product page | **Not applicable** — works during email-to-case conversion, not portal intake | ☐ |
| Generative AI hub supports PII masking | SAP Learning "Discovering the Orchestration Service" | Confirmed — orchestration data masking (anonymisation / pseudonymisation); extended AI Core plan typically needed | ☐ |
| Self-service role restricted to own cases | SAP KBA 3714154 | Confirmed pattern — business services with restriction rule; exact restriction for account-level visibility **[VERIFY]** | ☐ |
| SAP Best Practices scope item for this process | sap.com / SAP Learning search | **No scope item found** — scope items are an S/4HANA concept; Service Cloud v2 uses implementation steps instead. Note in proposal, do not cite a scope item | ☐ |

---

## 11. Corrections to feed back into 30_Solution_Proposal.md

1. Mandatory intake fields → configuration (dynamic properties), not BTP.
2. Standard/CR flag → configuration via catalogue level; generative AI hub becomes contingency only.
3. Add E2 (validation for fields cleared after creation) as a known standard behaviour.
4. Remove any implied SAP Best Practices scope item; reference Service Cloud v2 implementation steps instead.
5. Case Type Determination: do not propose (email-to-case only).

---

## Sources

- [SAP — Service Cloud Version 2, Case Categorization](https://www.sap.com/uk/products/crm/service-cloud-version-2-case-categorization.html)
- [SAP — Service Cloud Version 2, Similar Case Recommendation](https://www.sap.com/westbalkans/products/crm/service-cloud-version-2-similar-case-recommendation.html)
- [SAP — Service Cloud Version 2, Case Type Determination](https://www.sap.com/france/products/crm/service-cloud-version-2-case-type-determination.html)
- [SAP Learning — Configuring Case Routing](https://learning.sap.com/courses/configuring-service-operations-in-sap-service-cloud-version-2/configuring-case-routing)
- [SAP Learning — Using Extensibility (Service Cloud v2)](https://learning.sap.com/courses/implementing-sap-service-cloud-version-2-essentials/using-extensibility)
- [SAP Learning — Integrating AI into Service Cloud v2](https://learning.sap.com/courses/sap-service-cloud-version-2-consultant-course/integrating-ai-into-sap-service-cloud-version-2)
- [SAP Learning — Orchestration Service (generative AI hub)](https://learning.sap.com/courses/discovering-sap-s-generative-ai-hub/discovering-the-orchestration-service)
- [SAP KBA 3587077 — Field properties via Extensibility Administration](https://userapps.support.sap.com/sap/support/knowledge/en/3587077)
- [SAP KBA 3584803 — Category levels as required](https://userapps.support.sap.com/sap/support/knowledge/en/3584803)
- [SAP KBA 3744700 — Subject can be cleared after creation](https://userapps.support.sap.com/sap/support/knowledge/en/3744700)
- [SAP KBA 3706324 — Mandatory field cleared via inline edit](https://userapps.support.sap.com/sap/support/knowledge/en/3706324)
- [SAP KBA 3714154 — Self-service business role](https://userapps.support.sap.com/sap/support/knowledge/en/3714154)
