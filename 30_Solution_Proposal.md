# Solution Proposal — RISE Service-Request Intake & Routing (Pilot)
 **[VERIFY]** = check on help.sap.com / SAP Discovery Center before sending · **[ASSUMPTION]** = our working assumption · **[FICTIONAL]** = inherited from the RFP, not real data

---

## 1. Executive summary

We propose a **standard-first** pilot: configure the AI features SAP already ships for case management (Joule in SAP for Me, agentic case resolution, SAP Service Cloud Version 2 Case Categorization and Similar Case Recommendation), then close only the confirmed gaps with a **small side-by-side extension on SAP BTP**. The pilot covers one category (RISE system copy / refresh **[FICTIONAL]**), one language, ~50 customers, 6 dispatchers and 2 resolving teams, over ~10 weeks build + 8 weeks measured run.

Our expectation is that **most of the scope is configuration, not code**. The likely custom pieces are: category-specific mandatory-field validation, a deterministic duplicate rule (same customer + system ID + open status), the "standard catalogue task vs change request" flag, and the KPI extract. Explore will confirm or remove each of these.

The single biggest commercial and technical unknown is that **the target system is SAP's own internal support stack**. Whether an external vendor can configure or extend it at all must be settled before Realize (see §6, Q1).

---

## 2. Understanding of objectives and risks

### 2.1 What SAP wants

| Objective | How we read it |
| --- | --- |
| Raise Cloud CSAT (2026 target 75–76%) | Slow, re-worked RISE service requests are a detractor; fixing the front door of one category is a measurable lever |
| Complete requests | Customer cannot submit without system ID, component, request type, target date, approver |
| No duplicates | Same customer + same system + same need = one request |
| Right team first time | AI suggests the resolving team; a human dispatcher decides |
| Early commercial clarity | Flag "change request needing commercial approval" at intake, not weeks later |
| Small and on-strategy | Extend SAP's own Joule / agentic case resolution agenda; no new platform, clean core |

### 2.2 What we heard between the lines

- **Prove before scale.** The deliverable that matters most is the Run-phase KPI report and a go/no-go — not the software.
- **Avoid building what SAP's roadmap will ship.** SAP announced agentic case resolution (case analysis, duplicate detection, routing suggestions) in June 2026. Fit-to-standard is not a formality here; it is the main risk control.
- **Trust.** Dispatchers keep the final decision; override reasons are the feedback loop.

### 2.3 Key risks (summary — full table in §7)

Roadmap overlap · no baseline data · confidential/personal case data · access to SAP-internal systems · AI misrouting eroding dispatcher trust · scope creep into auto-replies or more categories.

---

## 3. Proposed SAP products

Product names below were checked against public SAP sources on 6 Nov 2026. Each still needs a licensing and availability check for SAP's internal tenant **[VERIFY]**.

### 3.1 Standard capabilities to reuse first

| Product / feature | Role in the pilot | Confidence |
| --- | --- | --- |
| **Joule in SAP for Me** | Customer entry point; conversational request creation; Joule can offer to create a case with details pre-filled | Confirmed public (rolling out from May 2026, no extra cost) |
| **Agentic case resolution** (SAP support) | AI agents that analyse new cases, detect duplicates, suggest routing (and draft replies — out of scope here) | Confirmed public; internal configurability **[VERIFY]** |
| **SAP Service Cloud Version 2** | Assumed case management behind SAP for Me | **[ASSUMPTION]** — RFP open question |
| **SAP Service Cloud Version 2, Case Categorization** (AI feature) | Trained on past cases + category catalogue; auto-proposes category when confidence threshold met → drives routing rules and the standard/CR flag | Confirmed product; requires AI Units **[VERIFY]** |
| **SAP Service Cloud Version 2, Similar Case Recommendation** (AI feature) | Semantic match on subject/description; returns top 3 similar categorised cases from last 12 months | Confirmed product; see gap note below |
| Service Cloud v2 routing / queue rules | Map category → resolving team; dispatcher accept/override | **[VERIFY]** exact config objects |

**Gap note — duplicates:** Similar Case Recommendation is designed to find *solutions* from *past categorised* cases. The RFP asks for a check against *open* requests from the same customer and system. That is probably a deterministic rule (customer + SID + category + open status), optionally combined with semantic similarity. Treat as a likely gap until Explore proves otherwise.

### 3.2 BTP extension stack (only for confirmed gaps)

| Product | Use | Notes |
| --- | --- | --- |
| **SAP Business Technology Platform (SAP BTP)** | Side-by-side runtime (Cloud Foundry or Kyma) | Sapphire 2026 announced **SAP Business AI Platform** as an umbrella over BTP, Business Data Cloud and Business AI — confirm current naming for the proposal **[VERIFY]** |
| **SAP Cloud Application Programming Model (CAP)** | Extension service: intake validation, duplicate rule, KPI event store | Programming model, not a licensed product |
| **SAP Integration Suite** (Cloud Integration; optionally **SAP Integration Suite, advanced event mesh**) | I1/I2/I4 — events or APIs between SAP for Me, case management and the extension | Event vs REST decided in Explore |
| **SAP AI Core** + **generative AI hub** + **SAP AI Launchpad** | I3 — LLM call for the standard-vs-CR flag *only if* Case Categorization cannot carry it | Model availability and data-residency **[VERIFY]** |
| **Joule Studio** (skill builder / agent builder) | Option to expose intake checks as a Joule skill instead of custom UI logic | Licensing: agent builder promo end date shown inconsistently (30 Sep vs Dec 2026) on SAP pages **[VERIFY]** |
| **SAP HANA Cloud** | KPI event table (I5); vector engine only if semantic duplicate check is needed | Keep minimal |
| **SAP Cloud Identity Services** | SSO for customers, dispatchers, admins; technical identities | |
| **SAP Cloud ALM** | Project, fit-gap backlog, test management, operations monitoring | Standard Activate tooling |

### 3.3 Explicitly not proposed

SAP Analytics Cloud (no new dashboards — out of scope), SAP Build Process Automation (commercial approval workflow stays as today), any modification of SAP for Me / Joule / case-management code.

---

## 4. Target solution (pilot)

```
Customer (basis admin)
   │  Joule in SAP for Me — guided intake for "system copy / refresh"
   ▼
[1] Mandatory fields enforced (standard form config first; BTP check if gap)
   ▼
[2] Duplicate check — open requests, same customer + SID  (deterministic rule)
    + similar-case hint (Similar Case Recommendation)
   ▼
[3] Classification — Case Categorization → resolving team + standard/CR flag
    (generative AI hub fallback only if standard cannot carry the flag)
   ▼
[4] Dispatcher in case management: accept / override (+ mandatory reason code)
   ▼
[5] ECS resolving engineer receives complete, correctly routed request
   ▼
[6] Event log → KPI extract (SC1–SC4)
```

Human-in-the-loop is mandatory at step 4. No message goes to the customer without a human.

---

## 5. Implementation approach — SAP Activate

| Phase | Duration | Key activities | Exit deliverable |
| --- | --- | --- | --- |
| **Discover** | Before contract | This proposal; demo of standard Joule in SAP for Me + Service Cloud v2 AI features; confirm vendor access model | Agreed scope + access model (Q1) |
| **Prepare** | 2 wks | Confirm systems behind SAP for Me; data-access & DPO approach; set up Cloud ALM; **take baseline for SC1–SC4**; choose pilot category from CSAT detractor data | Project plan, baseline report — **gate: no baseline, no Realize** |
| **Explore** | 3 wks | Fit-to-standard workshops: (a) Joule intake, (b) agentic case resolution, (c) Case Categorization & routing, (d) duplicate handling, (e) KPI data. Every gap gets a decision: *accept standard / configure / extend* | Fit-gap backlog with decision per item; solution design for gaps only |
| **Realize** | 4 wks (2 sprints) | Sprint 1: configuration, category catalogue, model training on anonymised history, routing rules. Sprint 2: BTP gap items, dispatcher override reasons, KPI extract; integration + UAT with anonymised/synthetic data | Tested configuration + extension; UAT sign-off |
| **Deploy** | 1 wk | Cutover, pilot user enablement (6 dispatchers, 2 teams), customer comms for ~50 pilot customers, confidence thresholds set conservatively | Pilot go-live sign-off |
| **Run** | 8 wks | Hypercare; weekly override review; threshold tuning; KPI measurement vs baseline | KPI report + go/no-go and rollout recommendation |

**Team (indicative):** solution architect (Service Cloud / support processes), Service Cloud v2 functional consultant, BTP/CAP developer, integration consultant, AI/data consultant (part-time), project lead. SAP side: support ops lead, dispatcher SME, ECS team lead, DPO, BTP admin.

---

## 6. Assumptions and open questions

### 6.1 Assumptions

| # | Assumption |
| --- | --- |
| A1 | SAP Service Cloud Version 2 is the case system behind SAP for Me **[ASSUMPTION]** |
| A2 | The vendor gets a configuration role in SAP's internal tenants (or works in a co-delivery model with SAP's own support IT) |
| A3 | Enough labelled historic cases exist for the pilot category to train Case Categorization (volume threshold **[VERIFY]**) |
| A4 | Anonymised or synthetic data is acceptable for build and test; production data only in Run after DPO sign-off |
| A5 | English only; one category; pilot users as per RFP **[FICTIONAL]** sizes |
| A6 | AI Units / Joule licences are provided by SAP internally at no cost to the pilot budget |
| A7 | ECS resolving teams' tooling receives assignments through the existing case-management interface (no new integration to ECS tools) |
| A8 | "Standard catalogue task vs change request" can be derived from a maintained catalogue list plus classification — the catalogue exists and is current |

### 6.2 Open questions for kickoff

1. **Access model:** can an external partner configure or extend SAP's internal support systems (SAP for Me, agentic case resolution, internal Service Cloud tenant)? If not, what is our role — design + extension only, with SAP support IT doing configuration?
2. Which case system actually sits behind SAP for Me? (RFP open question)
3. Is agentic case resolution configurable per category (thresholds, routing targets), or is it a central SAP service we can only consume?
4. Baselines for SC1–SC4 — who extracts them and from which system?
5. Which category drives Cloud CSAT detractors? Is "system copy / refresh" a placeholder?
6. Where does the RISE service-request catalogue live, and who owns it?
7. Data-access rules: can model training use real historic case text, and in which region?
8. Is there an SAP roadmap date for any of the in-scope features that would make the build obsolete?

---

## 7. Risks and mitigations

| # | Risk | Likelihood / Impact | Mitigation |
| --- | --- | --- | --- |
| R1 | SAP's own agentic case resolution roadmap already covers routing and duplicates | High / High | Roadmap check in Prepare; fit-to-standard first; extension designed to be removable |
| R2 | Vendor cannot get access to SAP-internal systems | Medium / High | Settle access model in Discover; fallback co-delivery model |
| R3 | No baseline data — benefit cannot be proven | Medium / High | Baseline as hard gate before Realize |
| R4 | GDPR / confidentiality of customer case text | Medium / High | Anonymised/synthetic data; DPO sign-off; data stays in approved region; no case text in logs |
| R5 | Too few labelled cases for the pilot category → weak classification | Medium / Medium | Check volume in Prepare; conservative confidence threshold (fall back to dispatcher queue) |
| R6 | AI misroutes → dispatcher distrust | Medium / Medium | Human confirmation; mandatory override reason; weekly review; tune thresholds |
| R7 | Duplicate check produces false positives and blocks real requests | Medium / Medium | Warn-don't-block: customer can confirm it is a new need |
| R8 | Integration differs from RFP assumptions (I1–I4) | Medium / Medium | Confirm in Prepare; prefer standard APIs from SAP Business Accelerator Hub |
| R9 | AI licensing (AI Units, Joule Studio promo end) changes cost | Medium / Low | Confirm commercial terms in Prepare **[VERIFY]** |
| R10 | Scope creep (auto-replies, more categories) | Medium / Medium | Change control; RFP §4 out-of-scope list as baseline |

---

## 8. Expected benefits

Qualitative now; quantified against baseline at end of Run.

- **Customers:** fewer round-trips for missing data; faster first qualified response; clearer early signal when a task needs commercial approval.
- **Dispatchers:** pre-sorted queue, suggestion with reason, less manual triage.
- **ECS engineers:** fewer re-assignments; requests arrive complete.
- **SAP support management:** hard data (SC1–SC4) to decide rollout; reusable pattern for other categories.
- **Strategic:** a real-world proof point for Joule / agentic case resolution on RISE, built clean-core.

RFP targets (≥85% first-time-right, ≤1.2 re-assignments, ≤10% returned, −30% time to first qualified response) are treated as hypotheses **[FICTIONAL]** until baseline exists.

---

## 9. Where Joule / embedded SAP AI helps — and where it does not

| Step | AI fit | Why |
| --- | --- | --- |
| Conversational intake | **Yes** — Joule in SAP for Me | Guides the customer, pre-fills from context |
| Mandatory-field enforcement | **No** | Deterministic validation; AI adds risk, no value |
| Duplicate detection (open requests, same SID) | **Mostly no** | A rule is exact and explainable; semantic similarity only as a hint |
| Suggesting the resolving team | **Yes** — Case Categorization / agentic case resolution | Pattern learning from historic routing |
| Standard task vs change request | **Partly** — classification + catalogue lookup | Catalogue match is deterministic; AI helps with free-text requests |
| Final assignment decision | **No** — human | RFP requirement; accountability and trust |
| Drafting replies / auto-resolution | **Not in scope** | Explicitly excluded (RFP §4) |
| Commercial approval | **No** | Stays in today's process |
| KPI calculation | **No** | Plain event logging and counting |
| Override analysis in Run | **Optional** — LLM summary of override reasons via generative AI hub | Useful once volumes grow; not needed for 6 dispatchers |

---

## 10. Verify on help.sap.com / SAP Discovery Center before sending

1. **Joule in SAP for Me** and **agentic case resolution** — whether either is customer- or partner-configurable, or SAP-internal only (support.sap.com / SAP Community).
2. **SAP Service Cloud Version 2, Case Categorization** — minimum training data, supported languages, confidence threshold settings, AI Units requirement.
3. **SAP Service Cloud Version 2, Similar Case Recommendation** — whether it can search *open* cases and filter by customer/system (public page says last 12 months of categorised cases, top 3).
4. Service Cloud v2 routing / queue configuration and dispatcher override with reason code — standard or extension?
5. Available Service Cloud v2 APIs/events for I1, I2, I4 (SAP Business Accelerator Hub).
6. **Joule Studio** (skill builder / agent builder) — licensing after the promo period (SAP pages show both 30 Sep 2026 and Dec 2026), and whether "classic" vs new edition matters for a new build.
7. **SAP Business AI Platform** vs **SAP BTP** — which name to use in the proposal after Sapphire 2026.
8. **Generative AI hub** in SAP AI Core — model list, region/data-residency, and data-processing terms for case text.
9. **SAP Integration Suite, advanced event mesh** vs other eventing options — current recommended product for SAP for Me / Service Cloud v2 events.
10. **SAP Activate** — current methodology version in the Roadmap Viewer (phase names and deliverables) and Cloud ALM support for fit-to-standard.

---

## Sources

- [SAP News — SAP Launches Joule in SAP for Me (Jun 2026)](https://news.sap.com/2026/06/sap-launches-joule-in-sap-for-me/)
- [SAP Support — Joule AI Assistant in SAP for Me](https://support.sap.com/content/s4m/help/joule-ai.html)
- [SAP — Service Cloud Version 2, Case Categorization](https://www.sap.com/uk/products/crm/service-cloud-version-2-case-categorization.html)
- [SAP — Service Cloud Version 2, Similar Case Recommendation](https://www.sap.com/westbalkans/products/crm/service-cloud-version-2-similar-case-recommendation.html)
- [SAP — Joule studio, agent builder](https://www.sap.com/germany/products/financial-management/joule-studio-agent-builder.html)
- [SAP Developers — Extend with AI (AI Core, generative AI hub, Joule Studio)](https://developers.sap.com/ai/)
- [The New Stack — Managed Joule Studio at Sapphire 2026](https://thenewstack.io/sap-joule-studio-managed-agents/)
