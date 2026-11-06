# AI Adoption Reflection — SAP AI Adoption Simulation

**Author:** Konstantin, Senior ABAP Developer
I'm a developer, not a functional consultant, so I approached the process side of this simulation mostly out of curiosity.

## 1. Where did AI help most?
- AI turned the blueprint into a structured backlog, stories and test cases very quickly. That gave me a usable picture of a support process I don't work in every day.
- It suggested edge cases systematically: repeated events, an empty system ID, customer isolation, fail-open.
- In A6, it kept the investigation in the order symptom → hypothesis → evidence.

## 2. Where did AI fail?
- It sounds equally confident about facts and guesses. Uncertainty only became visible once I asked for `[VERIFY]` tags.
- It defaulted to ECC patterns, suggesting IDoc/qRFC checks for a BTP event + REST integration. My ABAP background helped me spot this as the wrong layer.
- It missed contradictions between documents ("warning to customer" was impossible) until I asked it to check explicitly.

## 3. What hallucinations did you discover?
- Unconfirmed product details: Similar Case Recommendation limits, case-creation events, filtering on custom fields, field visibility in self-service.
- Plausible process fiction presented as SAP behaviour, such as the "two-step Joule case creation" in A6.
- Lesson: SAP Note numbers and component names are high-risk. I treat them as false until I find them in SAP for Me.

## 4. What required SAP validation?
- Released APIs: endpoints, filterable and exposed fields, OAuth scopes.
- Eventing: which event fires, whether BTP can subscribe, and delivery timing.
- Fit-to-standard claims, plus the authorization and SoD design.

## 5. What tasks should never rely fully on AI?
- Root cause statements in real incidents. Only logs and change history can confirm them.
- Security, authorization and data-protection decisions.
- Scope decisions and fit-to-standard verdicts.
- Production changes and customer communication.
- Merging generated ABAP/CAP code without review, especially locking, commit handling and SQL performance.

## 6. How would AI change my SAP role?
- Less boilerplate (CDS, RAP, CAP, test scaffolding). More reviewing, testing and owning what gets merged.
- Faster ramp-up outside my core area, as this simulation showed.
- More work on exposing SAP data safely to AI agents through released APIs.
- Deep ABAP knowledge stays valuable as the way to judge whether AI output is correct.

## 7. What risks exist in enterprise SAP AI adoption?
- Confident hallucinations ending up in blueprints, tickets or code.
- Customer data leaking into AI tools or AI-driven integrations.
- Over-privileged technical users for agents.
- Silent fail-open behaviour without monitoring.
- Shallow skills and rubber-stamp reviews.
- Custom AI extensions being replaced later by SAP standard.

## 8. What skills should SAP consultants build next?
- Prompting with constraints (sources, confidence, facts vs. assumptions), and then actually checking the sources.
- Fast verification on help.sap.com, the SAP Business Accelerator Hub and SAP for Me.
- BTP basics: CAP, destinations, OAuth, events, application logs.
- Clean core and released APIs.
- Observability for anything automated.
- Reviewing AI-generated code for SAP pitfalls: locks, LUW, authorization checks, performance.
