# Customer Research — SAP SE

> Consolidated from three AI Council drafts: **10a (Gemini)**, **10b (ChatGPT)**, **10c (Claude)**.
> Key facts re-checked on 6 Nov 2026 against SEC filings, SAP quarterly statements, SAP News / SAP for Me and trade press.
> Labels used below: ✅ verified · ⚠️ conflict / needs validation · ❌ rejected (unsupported, wrong or out of scope).

**Scope note on the drafts.** The customer for this exercise is **SAP SE as a company** (SAP as buyer of process improvement, "SAP runs SAP"). Draft 10c addresses that scope. Draft 10b addresses it too, but without any source URLs. Draft 10a (Gemini) researched a *different customer* — Life Sciences / MedTech manufacturers that SAP would sell to — so its content (FDA warning letters, chargebacks, eMDR) is **rejected as out of scope** for SAP SE and used only as a reference for what an unsupported draft looks like (see G).

---

## A. Company overview

| Item | Content | Status |
|---|---|---|
| Industry | Enterprise application software (ERP, CX, HCM, procurement, BTP, Business Data Cloud, Business AI) | ✅ |
| Scale (FY2025) | Total revenue €36.80B (+8%); cloud revenue €21.02B (+23%); free cash flow €8.24B | ✅ SEC 6-K FY2025 (via StockTitan) |
| Scale (Q3 2026) | Total revenue €10.27B (first quarter above €10B); cloud revenue €6.67B (+26%, +25% cc); current cloud backlog (CCB) €24.144B (+28%, +26% cc) | ✅ SAP Q3 2026 Quarterly Statement (SEC 6-K, 21 Oct 2026) |
| Business model | Shift from on-prem licence + support to cloud subscription (RISE with SAP / Cloud ERP Private, Public Cloud, BTP). Software licence revenue −29% in 2025; support revenue declining | ✅ (licence decline: FY2025 6-K) |
| Known internal SAP landscape | SAP converted its own corporate ERP from ECC 6.0 to S/4HANA in 2020 (modifications reduced ~3,000 → ~1,500). In 2025 SAP stated it runs SAP Business Suite / Cloud ERP via RISE and is "customer zero" for Business Data Cloud | ⚠️ Source: SAP News 2020 and 2025 keynote (cited in 10c, not re-opened). Exact current edition, release and remaining custom code are **not public** |
| Customer-facing support channel | SAP for Me portal; Joule in SAP for Me rolled out from May 2026 at no extra cost; AI "agentic case resolution" in support workflows | ✅ SAP News, June 2026 |

**Rejected from A:**
- ❌ 10b: "SAP has 34 ERP/CRM systems running on on-prem infrastructure" — no source given, not found.
- ❌ 10b: "2023 email leak" as a governance driver — no source; removed.

---

## B. Business challenges

| # | Challenge | Evidence | Drafts | Status |
|---|---|---|---|---|
| B1 | **Customer satisfaction shortfall.** Customer NPS fell 3 points to 9 in 2025, below the 12–16 target, mainly due to on-prem customers not yet on cloud. From 2026 SAP replaced NPS with **Cloud CSAT** as its customer-experience KPI (2025 baseline 75%, 2026 target 75–76%) | SEC 6-K FY2025; SAP FY2025 results presentation (29 Jan 2026) | 10b, 10c | ✅ Agreed between drafts. 10c's claim that Cloud CSAT is in Executive Board **compensation** comes from the 20-F (not re-opened) → ⚠️ |
| B2 | **Support / service-request experience in RISE.** Gartner (via The Register, May 2025): RISE costs higher than expected, service levels below industry standard, standard uptime SLA 99.7% vs 99.9% industry norm, SAP internal processes delaying infrastructure tasks | The Register, 8 May 2025; SAP Learning confirms 99.7% application SLA | 10c | ✅ SLA verified. "Tasks taking over a month" is a Gartner client anecdote → ⚠️ |
| B3 | **Cloud gross margin compression.** Non-IFRS cloud GM 74.3% in Q3 2026 vs 75.1% (−0.8pp; −0.7pp cc); IFRS 73.9% vs 74.6%. Cost of cloud growing faster than cloud revenue | SAP Q3 2026 statement; Finsee / FourWeekMBA | 10c only | ✅ Figures verified. Drivers (AI, hardware, sovereign cloud) are management commentary → ⚠️. "1pp ≈ €260M" is 10c's own arithmetic → Assumption |
| B4 | **Backlog-to-revenue conversion.** CCB growing faster than recognised revenue; SAP guides CCB growth to "slightly decelerate" in 2026 | SAP Q3 2026 statement | 10c | ✅ Facts. "Delivery throughput is the bottleneck" → Assumption |
| B5 | **EU antitrust commitments (on-prem support).** Commission opened proceedings Sept 2025; commitments made legally binding on 9 July 2026, apply globally, 10 years, independent monitoring trustee; customers may split landscapes and use different support providers; no infringement finding | PYMNTS, Concurrences, Forrester, Diginomica | 10c only | ✅ Verified. Impact on SAP's contract / billing systems → Assumption |
| B6 | **ECC 2027 migration cliff.** Gartner: 39% (~14,000) of ~35,000 ECC customers migrated by end-2024; ~17,000 projected holdouts in 2027 | CIO.com, 4 Jun 2025; CIO.de | 10b, 10c | ✅ Verified. ⚠️ 10b says "2027 ERP maintenance deadline" without nuance — deadline applies to standard mainstream maintenance; extended options exist |
| B7 | **Pricing / licensing friction.** DSAG: 79% cite cost-effectiveness, 70% licensing/contract design | 10b (no URL) | 10b | ⚠️ Plausible, consistent with 10c's e3mag sources, but numbers **not verified** |
| B8 | **US public-sector pricing probe (DOJ civil, SAP/Carahsoft)** | Yahoo Finance / CIO.com (10c) | 10c | ⚠️ Unproven, ongoing. Kept only as background; **do not raise in kickoff** |

**Conflicts between drafts in B**
- **10a vs 10b/10c — different customer.** 10a's five pain points (FDA complaint handling, CAPA, GTN chargebacks, field service, SaMD) are pain points of *SAP's MedTech customers*, not of SAP SE. ❌ Rejected for this report.
- **10b "60%+ of S/4HANA projects exceed budget; 65% have major quality gaps"** — no source in the draft. A trade-press item reports "nearly 60%" over budget/behind schedule, so the order of magnitude exists, but 65% is unsupported. Also this describes SAP's *customers'* projects, not SAP SE's internal process. ⚠️/❌
- **10b Revlon ($64M) / Haribo (25% drop)** — real public cases but not about SAP SE; no URLs given. ❌ Not used.
- **10b "28% of SAP customers cutting budgets in 2026"** — no source. ❌

---

## C. Candidate processes

| # | Process | Source draft | Evidence strength for SAP SE |
|---|---|---|---|
| C1 | **Customer support case / service-request triage and routing** (SAP for Me → SAP support / ECS) | 10b (as "complaint handling" hypothesis), 10c (pain #3) | Strong: NPS 9, Cloud CSAT KPI, Gartner RISE findings, SAP's own Joule/agentic launch |
| C2 | Support-to-subscription conversion: pricing, credits, billing reconciliation (invoice exception handling) | 10c (pain #5) | Medium: support revenue decline is a fact; leakage rate is **not public** |
| C3 | EU-commitment contract changes (landscape split, termination, reinstatement) | 10c (pain #4) | Medium: commitments verified; internal implementation unknown; legally sensitive |
| C4 | Quote-to-cash / CPQ for subscription contracts | 10b, 10c | Weak–medium: generic; no SAP-SE-specific evidence |
| C5 | Complaint handling with CAPA (QM-based) | 10a | ❌ Not applicable to SAP SE (manufacturing / FDA context) |

---

## D. Selected process

**Selected process:** Customer support case triage and routing for SAP for Me cases and RISE service requests (from case creation to assignment to the correct resolving team).

**Why selected:** It is the pain point with the best public evidence for SAP SE itself — a falling NPS (9 vs 12–16 target), a new Cloud CSAT KPI, and Gartner-reported RISE service-level and service-request delays — so the business case does not depend on unverifiable internal numbers. SAP is already investing in exactly this area (Joule in SAP for Me, agentic case resolution that analyses cases, detects duplicates and suggests routing), which makes an SAP-led extension realistic and politically safe ("accelerate SAP's own agenda", not "tell SAP how to run SAP"). The scope can be kept small — one case category, one hand-off — which suits Tasks A2–A6. It is also a process I know from the customer side (raising and following SAP incidents/service requests on projects), so I can sanity-check AI suggestions against real experience.

**Why not the alternatives:** C2 and C3 would need internal financial/contract data that is MNPI-sensitive and has no public baseline; C3 is also legally sensitive (EU trustee). C4 is too generic. C5 belongs to a different customer.

---

## E. SAP opportunity

**One realistic improvement (not a transformation):**
**Structured intake + AI-assisted classification for one RISE service-request category, before the case reaches a human queue.**

- **What changes:** When a customer opens a RISE technical service request through Joule in SAP for Me, the intake step collects the mandatory structured data up front (system ID, component, request type, desired date, approvals), checks for duplicates, and a classification step assigns the request to the right resolving team and flags whether it is a standard catalogue task or a change request needing commercial approval.
- **SAP building blocks (verified to exist):** Joule in SAP for Me and agentic case resolution (SAP News, June 2026); SAP Service Cloud v2 Joule agents for case classification and similar-case recommendation (SAP product pages, SAPinsider). Extension logic would sit on SAP BTP.
- **Not verified:** which system SAP uses *internally* behind SAP for Me for case management, and whether Service Cloud v2 agents are what SAP's own support runs. → validate in kickoff.
- **Pilot KPIs (targets are hypotheses, baselines must come from SAP):** % cases routed correctly first time; number of re-assignments per case; time from creation to first qualified response; share of cases returned to the customer for missing information.
- **EPAM role:** BTP extension and integration, prompt/knowledge-base engineering, test data and measurement framework — framed as capacity for SAP's agenda.

---

## F. Facts vs assumptions

| Statement | Fact / Assumption | Source |
|-----------|-------------------|--------|
| Q3 2026 current cloud backlog €24.144B, +28% (+26% cc) | Fact | https://www.sec.gov/Archives/edgar/data/0001000184/000110465926118617/tm2628113d1_ex99-1.htm |
| Q3 2026 total revenue €10.27B; cloud revenue +26% (+25% cc) | Fact | https://www.sec.gov/Archives/edgar/data/0001000184/000110465926118617/tm2628113d1_ex99-1.htm |
| Q3 2026 non-IFRS cloud gross margin 74.3% vs 75.1% (−0.8pp) | Fact | https://finviz.com/news/405805/sap-quarterly-statement-q3-2026 (PR Newswire copy of SAP statement) |
| SAP guides CCB growth to slightly decelerate in 2026 | Fact | https://finviz.com/news/405805/sap-quarterly-statement-q3-2026 |
| FY2025 cloud revenue €21.02B; total revenue €36.80B; FCF €8.24B | Fact | https://www.stocktitan.net/sec-filings/SAP/6-k-sap-se-current-report-foreign-issuer-67ac1f3702b9.html |
| Customer NPS 9 in 2025, below 12–16 target, mainly on-prem customers | Fact | https://www.stocktitan.net/sec-filings/SAP/6-k-sap-se-current-report-foreign-issuer-67ac1f3702b9.html |
| From 2026 SAP uses Cloud CSAT as customer KPI; 2025 = 75%, 2026 target 75–76% | Fact | SAP FY2025 results presentation (29 Jan 2026); https://www.earningswhispers.com/article/SAP/2604232538 |
| Cloud CSAT is part of Executive Board bonus | Fact per 10c, not re-checked | https://www.sec.gov/Archives/edgar/data/1000184/000110465926020058/sap-20251231x20f.htm — validate |
| RISE standard uptime SLA 99.7% (industry 99.9%) | Fact | https://www.theregister.com/2025/05/08/gartner_research_rise_with_sap/ ; https://learning.sap.com/learning-journeys/applying-the-rise-with-sap-methodology/introducing-rise-with-sap-infrastructure-and-preferred-success-services_f1933236-a4cc-4e60-b5bf-d880e7d8115e |
| RISE infrastructure tasks / service requests take "over a month" | Assumption (Gartner client anecdote) | The Register (above) — validate in kickoff |
| Joule in SAP for Me rolled out from May 2026, no extra cost; agentic case resolution analyses cases, detects duplicates, suggests routing, drafts replies | Fact | https://news.sap.com/2026/06/sap-launches-joule-in-sap-for-me/ |
| SAP Service Cloud v2 offers Joule case classification and similar-case recommendation | Fact | https://www.sap.com/canada/products/crm/service-cloud-version-2-similar-case-recommendation.html ; https://sapinsider.org/map/agentic-ai-sap-service-cloud-customer-support/ |
| EU commitments binding 9 July 2026, global, 10 years, monitoring trustee, no infringement finding | Fact | https://www.pymnts.com/cpi-posts/eu-makes-sap-antitrust-commitments-binding-in-erp-support-case/ ; https://www.forrester.com/blogs/saps-eu-settlement-shifts-erp-customer-leverage/ |
| Gartner: 39% of ~35,000 ECC customers migrated by end-2024; ~17,000 holdouts by 2027 | Fact (analyst estimate) | https://www.cio.com/article/4000543/ ; https://www.cio.de/article/4006923/ |
| SAP converted its own ERP from ECC to S/4HANA in 2020 | Fact per SAP News (cited in 10c) | https://news.sap.com/africa/2020/08/sap-runs-sap-our-transformation-to-sap-s-4hana/ |
| SAP's internal support case system is SAP Service Cloud / SAP-native | Assumption | validate in kickoff |
| Misrouted / incomplete cases are a material driver of resolution time | Assumption | validate in kickoff (needs SAP baseline data) |
| 10–30% faster resolution from agentic triage | Assumption (vendor / analyst figure, not SAP SE data) | validate in pilot |
| "1pp cloud GM ≈ €260M gross profit" | Assumption (10c arithmetic on ~€26B cloud revenue) | validate with SAP finance |
| DSAG: 79% cost-effectiveness, 70% licensing as top challenges | Assumption (unsourced in 10b) | not verified |
| "SAP has 34 ERP/CRM systems on-prem" | Rejected — no source | — |
| 10a: SAP Complaint Handling auto-generates FDA eMDR HL7 files for ESG submission | Rejected — out of scope and unsupported (only aggregator blogs cited) | — |

---

## G. AI reflection

**What AI helped with**
- Fast breadth: three drafts covered financials, regulation, customer sentiment and SAP products in minutes.
- 10c gave traceable SEC / SAP sources and its own "facts vs hypotheses" split, which made verification quick; almost every hard number in 10c checked out.
- Useful structure for discovery questions and the contradiction check (10b, 10c).

**What looked suspicious or wrong**
- **10a (Gemini) researched the wrong customer** (MedTech manufacturers instead of SAP SE). Inside that draft: footnote numbers glued to sentences, an "unknown_url" source, very precise improvement figures (70% faster cycle, >85% FTFR, "100% compliance", "zero Warning Letters") with no basis, and product statements that are wrong or overstated — e.g. "SAP S/4HANA Field Service Management" (SAP FSM is a separate cloud product, not part of S/4HANA) and native eMDR HL7 generation in Complaint Handling (unverified).
- **10b (ChatGPT)** has **no URLs at all**; several numbers are unsourced (34 systems, 60%/65% overruns, 28% budget cuts, 2023 email leak). It mixes up SAP SE with SAP's customers (Revlon, Haribo), uses an outdated product name ("SAP Data Warehouse Cloud", now SAP Datasphere) and proposes "SAP Solution Manager" where Cloud ALM is the current direction. Its "expected value" figures (20–30%, 30–50%, 90%) are invented.
- **10c (Claude)** is mostly solid but contains its own reasoned estimates (€80–260M margin, 1–3 months ramp pull-forward, 0.5–2% billing leakage) that read like facts unless you notice the "heuristic" labels. Some sources are secondary (FourWeekMBA, X posts, aggregator blogs).

**What I validated manually on the web / SAP sources**
- Q3 2026 CCB, revenue, cloud GM and outlook → SEC 6-K / SAP statement ✅
- FY2025 NPS 9 vs 12–16, Cloud CSAT replacing NPS (75% baseline, 75–76% target) ✅
- EU commitments: date, global scope, 10 years, trustee, no infringement finding ✅
- Gartner ECC 39% / 17,000 figures ✅
- RISE 99.7% SLA (The Register + SAP Learning) ✅
- Joule in SAP for Me and agentic case resolution capabilities (SAP News); Service Cloud v2 case classification / similar-case recommendation (SAP product pages) ✅

**Accepted / rejected / needs validation (summary)**
| Finding | Decision |
|---|---|
| 10c financials, NPS, EU, Gartner ECC, Joule launch | Accepted (verified) |
| 10b + 10c NPS / CSAT pain point | Accepted, used as basis for selected process |
| 10c value estimates, internal architecture statements | Kept as assumptions |
| 10c DOJ probe | Kept as background only, not for kickoff |
| 10b unsourced statistics, Revlon/Haribo, email leak | Rejected |
| 10a entire scope (MedTech/FDA) | Rejected as out of scope |

**What I would not trust AI to decide alone**
- Which process to pick and how to position EPAM toward SAP — that is a commercial and political judgement.
- Any quantified business case (savings, %, €) — needs SAP baseline data.
- Statements about SAP's *internal* systems, and whether a specific SAP product feature exists or works as described — needs SAP Help / product team confirmation.
- Anything touching legal matters (EU trustee, DOJ probe).

---

## H. Confidence and open validation

- **High confidence:**
  - SAP's Q3 2026 financials: CCB €24.1B, cloud GM 74.3% (down 0.8pp), CCB growth guided to slightly decelerate — SEC 6-K / SAP Quarterly Statement.
  - Customer NPS 9 in 2025 below target, and Cloud CSAT as the 2026 customer KPI — SEC 6-K FY2025 / SAP FY2025 results presentation.
  - Joule in SAP for Me and agentic case resolution exist and target case analysis, duplicate detection and routing — https://news.sap.com/2026/06/sap-launches-joule-in-sap-for-me/
  - EU commitments binding since 9 July 2026, global, 10 years — PYMNTS / Forrester / Concurrences.

- **Medium / needs validation:**
  - Gartner anecdotes on slow RISE service requests (single analyst source, client-reported).
  - Cloud CSAT being linked to Executive Board compensation (20-F cited by 10c, not re-opened).
  - Drivers of cloud margin decline (AI, hardware, sovereign cloud) — management commentary.
  - DSAG survey percentages on cost and licensing (10b, unsourced).
  - SAP's own internal ERP / support landscape (SAP News 2020 and 2025; current details not public).

- **What still needs customer or kickoff validation:**
  - Which system SAP's support organisation uses internally for case management behind SAP for Me, and how ECS service requests are handed over.
  - Baseline metrics for the selected process: first-time-right routing rate, re-assignments per case, time to first qualified response, share of cases returned for missing information.
  - Current adoption and measured results of agentic case resolution — and where SAP itself sees gaps.
  - Which case categories drive Cloud CSAT detractors (onboarding, service requests, incidents, renewals) — to choose the pilot category.
  - Data-access constraints for a pilot (customer case data, GDPR, clean-room or synthetic data).
