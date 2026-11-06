# Executive Brief  

**Top 5 Pain-Point Hypotheses for SAP SE:**  (SAP uses SAP)  
1. **Fragmented legacy systems and slow innovation:** SAP SE’s historic reliance on on-premises ERP/CRM systems led to complex customizations and silos across departments. This increases operational friction and slows decision-making.  
2. **Low customer satisfaction and trust issues:** SAP’s Customer NPS fell to **9** in 2025 (below its 12–16 target). In user-group surveys, ~70–79% cite *cost, licensing and contract terms* as top challenges. This erodes customer loyalty and signals revenue risk.  
3. **Risky ERP transformation delays/costs:** Industry data show **60%+** of SAP S/4HANA projects exceed budget/schedule, with ~65% incurring major quality defects. High-profile failures (e.g. Revlon lost ~$64M sales, Haribo saw a 25% revenue drop) highlight the stakes of poor governance.  
4. **Inefficient order-to-cash and contract processes:** Complex subscription/licensing models without integrated quoting/billing can cause billing delays and revenue leakage. 28% of SAP customers are cutting budgets in 2026, suggesting cash-flow pressures.  
5. **Data silos and limited AI insights:**  Fragmented data across ERP, CRM and support systems hinder real-time analytics. SAP’s own case study reports shifting to a unified cloud enabled *“real-time data”* across finance, sales, and service; without it, SAP SE risks slower reporting and missed AI opportunities.  

**Why These Matter Now:**  SAP SE is accelerating its cloud transformation (e.g. via RISE) to stay competitive. Current pressures (economic headwinds, end-of-support deadlines) make agility crucial. Declining NPS and budget cuts signal immediate revenue risk. The 2027 ERP maintenance deadline looms, so resolving these issues before major go-lives is urgent.  

**Likely Board-Level KPI Impacts:**  Customer Net Promoter Score (NPS) and **customer churn/renewal rates** (already trending unfavorably), **revenue growth and operating margin** (at risk if projects overrun or customers defect), **time-to-market** (for new offerings, tied to IT agility), and **Days Sales Outstanding (DSO)** (if O2C is manual) are key. For example, improving complaint resolution could raise Customer Satisfaction; streamlining billing could reduce DSO.  

# Evidence Table  

| Problem (Title + Source) | Evidence Summary | Why This Is a Problem (Business Impact) | Root-Cause Hypothesis | SAP-Led Solution Options | Expected Value | Confidence | Peer Analogs |
|:---|:---|:---|:---|:---|:---|:---|:---|
| **Legacy ERP/CRM Fragmentation** (SAP SE case)  | SAP’s own case study: “customizations, technical debt, and tightly coupled on-premises dependencies created obstacles that made it challenging to evolve quickly.” SAP has 34 ERP/CRM systems running on on-prem infrastructure. | Slow innovation cycles; high maintenance overhead; inability to leverage AI or cross-team processes. Fragmented data leads to silos between Finance, Sales, Service. | Decades of accumulated custom code and disjointed processes. Lack of unified cloud platform blocked holistic view of operations. | **SAP Cloud ERP (RISE with SAP)** to replace legacy ERP/CRM; Clean-core conversion to reduce custom code; adopt **SAP S/4HANA** modules for Finance, Sales, Service in cloud (Private/On-Premise). Use **SAP BTP** for integrations and extensions. | Faster month-end close and quoting; increased agility for product launches; foundation for embedded AI. SAP claims unified cloud enables connected, real-time data and faster processes. Qualitative: agility, scale. Quantitative: potential 20–30% faster cycle times (books close, quote approvals). | High (SAP’s own published case) | Other tech firms (e.g. Microsoft) also drive cloud-first ERP: Microsoft migrated HQ to Azure/S/4 (faster insights). |
| **S/4HANA Transformation Risk** | Industry survey: 60%+ of S/4HANA programs exceed budget/schedule; 65% report major quality gaps. High-profile cases: Revlon’s SAP go-live caused $64M lost sales; Haribo’s S/4 cut supply chain leading to 25% sales drop. | Cost overruns and revenue losses are direct hits to profitability and market trust. Governance failures also risk regulatory scrutiny (as Revlon faced). Delays stall new features. | Insufficient governance/oversight: scope creep, inadequate testing/data migration, weak change management. Heavy customization built in (e.g. as at Lidl) led to failures. | **SAP Activate methodology** with phased adoption; leverage **SAP Model Company** and **Best Practices** to reduce custom scope. Use **SAP S/4HANA Cloud** (fully activated appliance) for quicker ROI. Ensure **SAP Solution Manager** for transparency. | Mitigates risk of project failure: more on-time go-live, higher ROI. Avoids multi-million losses seen in peers. Qualitative: improved board visibility into transformation health. Quantitative: potentially 30–50% reduction in overrun risk (estimate based on typical improvement using best practices). | High (independent analysis) | Revlon, Haribo, Lidl cases as analogs. Gartner notes ~50% of digital ERP projects miss targets. |
| **High Cost & Complex Licensing** | SAP user surveys: 79% of DSAG (German SAP users) cite *cost-effectiveness* as a top challenge, 70% cite *licensing/contract design*. SAP NPS fell partly due to these issues. | Customer frustration can translate to lost renewals or budget cuts (28% are cutting SAP spend). Complex contracting means slow deal cycles and higher sales support costs. | Legacy licensing models (perpetual vs subscription), complicated cloud pricing, and rigid contract terms confuse customers. Lack of self-service quoting/renewal tools. | **SAP Subscription Billing and SAP CPQ:** modernize billing with flexible subscription models. Implement **SAP Contract Lifecycle Management (CLM)** or **Sales Cloud v2** for streamlined quotes. Use **SAP Sales Cloud** to enforce pricing policies. | Simplified billing reduces DSO and churn. Faster quote-to-cash increases revenue capture. Expected value: e.g. 10–15% faster order cycle; reduction in credit memos/overcharges. Improves NPS by addressing the #1 complaint source. | Medium (third-party reports) | Similar issues at other software companies (Oracle, Workday) where customers demand simpler subscription pricing. |
| **Customer Service/Complaint Handling Gaps** (Hypothesis) | *No direct source; inferred.* Large enterprises often have disconnected complaint/ticket systems (email, spreadsheets). SAP case shows need for unified service: “service teams work from connected, real-time data”. | Inefficient complaint processes cause long resolution times and dissatisfied customers. Missed SLA penalties or lost clients. Lack of root-cause tracking means repeated issues. | Multiple touchpoints (field, call center, email) without a single case repository; manual hand-offs between teams. Limited analytics on complaint trends. | **SAP Complaint Handling (S/4HANA)** to centralize end-to-end complaint workflows. **SAP Service Cloud v2** for omnichannel support and field service scheduling. Use **SAP Qualtrics** for feedback surveys. | Faster complaint resolution, higher first-contact resolution rates, better NPS. For example, integrated CRM could cut average resolution time by 30%. Qualitative: improved service consistency. | Low (assumption-driven) | Pharma/MedTech companies use SAP Complaint Handling to meet regulatory tracking (an analogous case). |
| **Data Silos / Poor Visibility** (Hypothesis) | *No direct source.* SAP SE’s complexity suggests data scattered in many systems. User survey: 27% of SAP customers worry about lacking cloud/data expertise. | Without unified data, finance and sales teams cannot quickly reconcile figures or forecast. It also inhibits AI initiatives (SAP touts “AI-driven capabilities” after integration). | Legacy systems kept separate; no common data lake. Over-customized reports. | **SAP Analytics Cloud** + **Data Warehouse Cloud** to unify reporting. **SAP Master Data Governance (MDG)** for customer/product data. **SAP BTP** for cross-system integration. | Single source of truth enables faster decisions and predictive insights. Expected improvement: e.g. 90% reduction in manual data reconciliations; improved forecast accuracy. | Medium | Many enterprises (e.g. BMW) build data lakes on SAP BTP to break silos; improved O2C analytics. |
| **Cloud Adoption Anxiety (Lock-In Concerns)** | Survey: Up to 49% of European SAP customers worry about dependence on one cloud vendor. Confidence in RISE programs is low (only 10–24% expect RISE to accelerate migration). | Slow adoption of cloud strategies leads to outdated tech and missed savings. Fear of vendor lock-in may cause some teams to hold off on new investments, delaying ROI. | Uncertainty about multi-cloud support and escape valves. Historical focus on SAP-only (vs. best-of-breed). | Promote **SAP BTP** and Open APIs to integrate third-party apps. Emphasize hybrid deployment options. Show success of unified platform from COSO perspective. | Increasing cloud adoption frees capital and reduces maintenance. A more open architecture can turn a 49% concern into trust; e.g. faster cloud rollouts. | Medium | Others like Microsoft promote Azure hybrid to address similar concerns. |

# Kickoff Preparation Pack  

## 1. Key Assumptions (to test)  
- **ERP/CRM Cloud Migration:** SAP SE has largely moved core ERP and CRM to SAP Cloud ERP via RISE, but some legacy modules/custom code likely remain.  
- **Complex Licensing:** The company faces internal debates over licensing/pricing strategy (matching the 70%+ dissatisfaction). Current quote/billing processes involve multiple systems.  
- **Data/Analytics Maturity:** Even with cloud ERP, internal reporting is lagging due to remaining data silos. They may lack unified analytics dashboards across Sales, Finance, and Service.  
- **Customer Service Tools:** Support may rely on SAP Service Cloud and ticketing tools, but they might not yet use the latest SAP Complaint Handling suite – possible gaps in cross-department case management.  
- **Quality Feedback Loop:** SAP SE likely collects customer feedback (e.g. via SuccessFactors or Qualtrics) but may not close the loop automatically into engineering or supply chain (common in software firms).  
- **Order-to-Cash Process:** Subscription-based revenue requires sophisticated handling – assume current O2C is partially manual (for example, manual invoice adjustments for promotions or maintenance).  
- **Skilled Resources:** In-house SAP skills may be strong, but there might be gaps in emerging areas (cloud architect, data science) as indicated by 22–27% customer concern on lack of cloud expertise.  
- **Regulatory Compliance:** As a German company, GDPR and data privacy are strictly managed, but incident (2023 email leak) may have raised awareness. This might influence governance on data handling.  
- **Business Pressure:** Given NPS drop and budget cuts, the board is likely pushing for visible ROI projects (fast wins) and cost control.  

## 2. Discovery Questions (by area)  
**Business:**  
- *Customer & Market:* “How has the recent decline in NPS been explained internally? Are there specific service or pricing issues at play?” – *target:* Head of Customer Experience. *Purpose:* Identify root causes of customer dissatisfaction (Expect insight into complaint volumes or licensing disputes).  
- *Financial:* “What KPIs (e.g. DSO, churn rate, contract renewal rate) are underperforming? What is the strategy to address budget cuts?” – *target:* CFO/Head of Finance. *Purpose:* Understand revenue leakage or cash-flow bottlenecks in O2C (insight into invoicing issues or payment delays).  
- *Strategic:* “What is the timeline and scope for completing the SAP S/4HANA migration? Have any go-lives been delayed or re-scoped?” – *target:* CIO/Transformation Lead. *Purpose:* Assess transformation status and risk (insight: whether governance issues exist).  
- *Customer Complaints:* “What channels do customers use to lodge complaints or requests? Are these captured in a unified system?” – *target:* Customer Service Director. *Purpose:* Clarify current complaint handling flows (insight: gaps SAP Complaint Handling could fill).  

**Process:**  
- *Order-to-Cash:* “Describe the end-to-end order-to-cash workflow. Where do manual touches or overrides occur?” – *target:* Order Management Lead/Head of Sales Operations. *Purpose:* Identify inefficiencies or errors in billing process (insight: need for CPQ or Subscription Billing).  
- *Complaint Handling:* “How do you currently route and prioritize customer complaints internally? Are SLAs met?” – *target:* Service Operations Manager. *Purpose:* Find process bottlenecks (insight: delays or rework points).  
- *Cross-Dept Collaboration:* “How are sales, service, and finance processes integrated? For example, does a service incident automatically trigger a customer invoice?” – *target:* Business Process Owner. *Purpose:* Uncover hand-off issues (insight: integration points needing improvement).  

**IT Architecture:**  
- *Landscape:* “Can you map out the current SAP landscape (ERP, CRM, on-prem, cloud, acquired systems)? Which modules are still on ECC vs S/4, on-prem vs cloud?” – *target:* Enterprise Architect. *Purpose:* Pinpoint any legacy pockets (insight: focus areas for migration or integration).  
- *Integrations:* “Which systems are integrated with SAP? Are any critical ones bespoke or unmanaged?” – *target:* Integration Lead/IT Manager. *Purpose:* Identify technical debt (insight: potential quick wins via integration rationalization).  
- *Custom Code:* “What percentage of your current ERP/CRM functionality is custom versus standard? Any retirements planned?” – *target:* SAP Basis/Development Lead. *Purpose:* Gauge upgrade complexity (insight: how clean the core is).  

**Data:**  
- *Data Quality:* “Do you have duplicate customer records or inconsistent data across systems? How is master data managed?” – *target:* Data Governance Lead. *Purpose:* Assess data risks (insight: need for MDG).  
- *Analytics:* “What tools are used for reporting and analytics? How long does it take to generate key reports (e.g. sales pipeline, complaint backlog)?” – *target:* BI Manager/Head of Reporting. *Purpose:* Spot manual effort (insight: value of unified data/analytics).  

**Governance:**  
- *Stakeholders:* “Who is on the steering committee for the SAP programs? How are priorities decided?” – *target:* CIO or Program Sponsor. *Purpose:* Understand decision-making (insight: alignment or silos).  
- *Change Management:* “How have past projects managed user adoption? Were there formal training or incentive programs?” – *target:* HR/Change Manager. *Purpose:* Gauge readiness for new tools (insight: risk of resistance or skill gaps).  

## 3. Day-1 Value: Quick Wins (30–90 days)  
- **Deploy a Unified Helpdesk:** Implement or optimize **SAP Service Cloud v2** for a single view of all customer complaints/incidents. Even enabling a trial of SAP Complaint Handling cockpit can cut manual case logging.  
- **Automate Simple O2C Tasks:** Introduce **SAP CPQ** for standard quotes and **Subscription Billing** for recurring fees. Start with one product line to speed up quotes and invoicing.  
- **Dashboard for Visibility:** Use **SAP Analytics Cloud** to create live dashboards (e.g. complaint backlog, quote-to-cash cycle times) by connecting current ERP/CRM data. This uncovers immediate bottlenecks.  
- **Clean Up Pricing Data:** Rapidly review and simplify top 10 contract templates and price books in SAP SD to remove deprecated options, reducing quoting errors.  
- **Process Mapping Workshop:** Hold a workshop to document current complaint and order-to-cash processes; identify 3–5 pain points where SAP tools (e.g. workflow alerts) can be applied immediately.  

## 4. Mid-Term Transformation Themes (6–18 months)  
- **Full S/4HANA Cloud Adoption:** Complete migration of remaining ECC modules to SAP S/4HANA (preferably cloud edition). Leverage the “clean core” strategy used internally. This unifies the foundation.  
- **Integrated Complaint Management:** Implement **SAP Complaint Handling (S/4HANA QM)** module to formalize end-to-end complaint resolution, linking it to Quality and Service modules. Embed **SAP Build Workzone** for a user-friendly ticket UI.  
- **Advanced Order-to-Cash:** Roll out **SAP Sales Cloud v2** with subscription management for all contract lines. Integrate fully with S/4 for automated revenue recognition (using SAP RAR or Finance SD-CO integration).  
- **Intelligent Analytics & AI:** Centralize data in **SAP Data Warehouse Cloud** and deploy AI-driven insights (as SAP highlights AI-first use). Examples: predictive maintenance on support assets, AI chatbots in Service Cloud.  
- **Process Governance and Training:** Establish a formal Center of Excellence for SAP processes to oversee change. Conduct comprehensive training for new apps (Service Cloud, Complaint Handling) and foster cross-functional ‘process owners’ culture.  
- **Licensing Model Modernization:** Based on user feedback, pilot a more flexible licensing approach using SAP Subscription Billing to bundle cloud services, addressing the 70% licensing complaint.  

## 5. Risks, Dependencies & Change Considerations  
- **Integration Complexity:** Many legacy interfaces may need rework during S/4 migration. Mitigation: prioritize high-impact integrations first, use SAP Integration Suite accelerators.  
- **User Adoption Resistance:** Users accustomed to old workflows (e.g. manual spreadsheets) might resist new complaint system. Mitigation: involve power users early, show demos of efficiency gains.  
- **Data Quality Hurdles:** Poor master data can cause errors in new systems. Mitigation: run parallel data-cleanup initiatives (customer/vendor deduplication) before go-lives.  
- **Budget Constraints:** Finance may push to limit scope. Mitigation: emphasize low-risk pilots and cloud OPEX vs. CAPEX trade-offs; propose phased rollout.  
- **Change Fatigue:** If many projects run concurrently (e.g., S/4 and Service Cloud), staff may be overwhelmed. Mitigation: stagger deployments, ensure visible quick wins.  

# SAP Fit Map  

| Pain Point (P) | SAP Capability (C) | Integration Touchpoints | Expected KPI Improvement (Example) |
|---|---|---|---|
| P1. ERP/CRM fragmentation and slow processes | C1. SAP S/4HANA Cloud (Private) + BTP | Master data replication between Sales, Finance, Service; workflows across departments via Business Events | **Invoice cycle time ↓** (e.g. 30% faster close); **Innovation lead time ↓** (faster new module deployments) |
| P2. Customer complaint backlog and slow resolution | C2. SAP Complaint Handling & Service Cloud v2 | Sync Complaint tickets to CRM, trigger Quality notifications; integrate with Field Service routing | **Case resolution time ↓** (target ~30%); **Customer Satisfaction ↑** (NPS points) |
| P3. Complex quoting and billing (licensing) | C3. SAP Sales Cloud v2 (CPQ) + Subscription Billing | Quote details ↔ ERP pricing master; automated billing documents generation | **Quote-to-cash cycle ↓** (days); **Revenue leakage ↓** (fewer billing adjustments) |
| P4. Data silos limiting insight | C4. SAP Analytics Cloud + Data Warehouse Cloud | Connectors to S/4HANA, Service Cloud, Sales Cloud | **Reporting speed ↑**; **Decision accuracy ↑** (e.g. forecast variance ↓) |
| P5. Governance and collaboration inefficiencies | C5. SAP Build Apps and Integration Suite | Low-code apps for approval flows; integrated alerts across apps | **Process compliance ↑** (audit findings ↓); **Cross-team alignment** (fewer hand-off delays) |

# Contradiction Check & Validation Plan  

- **Contradictions Found:** SAP’s success story (moving to RISE cloud) contrasts with user-group reports that customers doubt RISE’s benefits (only ~10–24% believe RISE accelerates migration) and worry about lock-in. Also, SAP’s NPS target vs. actual (12–16 goal vs. 9 result) shows a gap between goals and reality.  
- **Likely Reasons:** SAP’s internal narrative (positive, forward-looking) vs. broad customer experiences differ because SAP may highlight net benefits, while users experience transition pains and cost shock. Additionally, surveys mix regions and company sizes – SAP’s pilot (Walldorf HQ) might not reflect global customer diversity.  
- **Kickoff Validation Items:** 
   1. **Cloud Program ROI:** “Have projected benefits of the RISE initiative materialized (e.g. cost savings, fewer IT incidents)?” – to gauge actual vs. promised.  
   2. **NPS Drivers:** “What specific factors drove down NPS last year?” – to see if it was pricing, support quality, or other issues.  
   3. **Licensing Complexity:** “What contract/licensing scenarios cause the most confusion or complaint from customers?” – validate scope of pricing pain.  
   4. **Data Visibility:** “Where do you still need manual reconciliations or reports?” – to confirm data-silo pain points.  
   5. **Transformation Experience:** “What were the key challenges or surprises during your S/4 or cloud ERP rollout?” – to uncover governance gaps or user resistance.  

# Facts vs. Hypotheses  

**Facts (with evidence):**  
- SAP SE’s internal complexity and move to cloud (RISE) is documented by SAP.  
- SAP’s Customer NPS fell to 9 in 2025 (below target).  
- Survey: 79% of SAP users cite cost-effectiveness as a top challenge, 70% cite licensing/contract issues.  
- Significant overruns in SAP S/4HANA projects are common (60% over budget/schedule, quality problems); public cases (Revlon, Haribo) show severe revenue impact.  

**Hypotheses (inferred):**  
- SAP SE’s operations still suffer from some process silos (based on industry norms and SAP’s legacy) causing inefficiency.  
- Complex licensing models internally contribute to slower sales and billing cycles (inferred from external dissatisfaction stats).  
- Complaint handling processes may be partly manual or disjointed, leading to slower resolution times and lower customer satisfaction.  
- Data from disparate systems is not fully unified, limiting advanced analytics and AI use (inferred from the emphasis on moving to real-time integrated data).  

# Top 10 Kickoff Questions (Ranked)  

1. **ERP Strategy (CIO/CTO):** “What’s the current status and timeline of your S/4HANA/Rise cloud implementation? Have you hit any roadblocks?” *(Purpose: Assess project health; insight: governance or customization issues.)*  
2. **Customer Experience (Customer Service Dir.):** “What are the main pain points in your current complaint handling process? How do delays or hand-offs occur?” *(Purpose: Identify process gaps; insight: unmet service expectations.)*  
3. **Sales & Contracts (Sales VP):** “Can you describe your order-to-cash cycle end-to-end, especially how complex products or subscriptions are handled?” *(Purpose: Spot inefficiencies; insight: need for CPQ or billing tools.)*  
4. **Finance (CFO):** “Which financial KPIs (DSO, invoice error rate, billing cycle time) are currently off target? What processes cause those delays?” *(Purpose: Pinpoint revenue leakage; insight: quantify O2C friction.)*  
5. **IT Landscape (Enterprise Architect):** “What major systems (SAP or non-SAP) are still on-prem or legacy? How do they integrate today?” *(Purpose: Map technical debt; insight: integration priorities.)*  
6. **Data & Analytics (BI Manager):** “Do you have issues with data consistency (e.g. duplicate customers)? How long does it take to produce key reports?” *(Purpose: Understand data pain; insight: need for MDG/analytics.)*  
7. **Support & Training (HR/Change Lead):** “How are end-users trained on new systems (like Service Cloud)? Any feedback on user adoption so far?” *(Purpose: Uncover adoption risk; insight: need for more change support.)*  
8. **Licensing and Pricing (Commercial Ops):** “What customer feedback have you received about SAP’s pricing or contract terms in the past year?” *(Purpose: Confirm external pain; insight: guide quoting system improvements.)*  
9. **Quality Feedback Loop (Product Manager):** “How does customer feedback (complaints or surveys) feed back into product development or process improvement?” *(Purpose: Check feedback integration; insight: traceability gaps.)*  
10. **Governance (Program Sponsor):** “Who makes go/no-go decisions on IT projects? How do you balance strategic initiatives versus short-term needs?” *(Purpose: Understand decision governance; insight: identify potential roadblocks or champions.)*  

