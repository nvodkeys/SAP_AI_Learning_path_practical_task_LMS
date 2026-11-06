# 01 – My Personal AI Usage Guidelines

| | |
|---|---|
| **Role** | SAP Consultant |
| **Owner** | Konstantin Rapanov |
| **Exercise** | AI Adoption |
| **Status** | Draft – pending mentor review |
| **Date** | 2026-11-06 |

> **Legend:** ⚠️ = must verify with my mentor

---

## 1. EPAM-Approved Tools I Will Use

- **EPAM AI/Run CodeMie** – drafting, summaries, ABAP/configuration help
  - ⚠️ Approved for use on my project?
- **EPAM AI DIAL** – EPAM's internal AI platform
  - ⚠️ Do I have access, and which models am I allowed to use?
- **GitHub Copilot / Microsoft 365 Copilot** – only if licensed for me
  - ⚠️ Is a license assigned, and does the client allow it?
- EPAM accounts only. No personal accounts or public free AI tools.
- ⚠️ Does the client contract restrict or ban AI use?

---

## 2. Data I Will NEVER Paste into AI

- Client production or master data (customers, vendors, materials, finance, HR)
- Personal data: names, emails, employee IDs, bank or payroll details, addresses
- Credentials: passwords, S-user IDs, RFC/system connection details, API keys
- Contracts, pricing, NDA-covered or unpublished financial information
- Authorization roles, security settings, audit or vulnerability findings
- Client-specific custom code
  - ⚠️ Allowed if anonymized?
- ⚠️ What counts as properly anonymized data, and is it allowed at all?

---

## 3. How I Validate SAP-Related Outputs

- Check every t-code, table, field, BAPI, and function module in the system (SE11 / SE37 / SE93) or on the SAP Help Portal
- Confirm the answer fits the release: ECC vs. S/4HANA, version/feature pack
- Look up every SAP Note or KBA on SAP for Me before citing it
- Test code and configuration in a sandbox or DEV first, never directly in production
- Compare against project design documents and solution architecture
- Record which sources I checked

---

## 4. Example AI Hallucination to Watch For

- **Scenario:** For an S/4HANA system, the AI tells me to read goods movements only from tables **MKPF/MSEG**.
- **Reality:** In S/4HANA the main table is **MATDOC**. MKPF/MSEG remain only as compatibility views.
- **Why it's dangerous:** The answer sounds confident and is correct for ECC, but outdated for my release.
- **Also watch for:**
  - Invented SAP Note numbers
  - Non-existent SPRO/IMG paths
  - Made-up BAPI or function module parameters

---

## 5. Always Requires My Human Sign-Off

- Transports, configuration, or code moving to QA or production
- Authorization and segregation-of-duties (SoD) design
- Data migration mappings and cutover steps
- Financial posting logic, tax, and compliance-related settings
- Anything sent to the client (deliverables, estimates, recommendations)
- Any AI output I have not personally verified
- ⚠️ Is a peer or lead review also required (e.g., for code or security changes)?

---

## Open Questions for My Mentor

1. Which AI tools am I approved to use, and do I have access?
2. Does the client allow AI use on this project?
3. What are the anonymization rules?
4. Can client custom code be shared with AI tools?
5. Are there additional review steps?

---

## Mentor Review

| Item | Outcome / Notes |
|---|---|
| Approved tools confirmed | |
| Client AI policy confirmed | |
| Anonymization rules confirmed | |
| Custom code sharing rule | |
| Extra review steps | |
| **Reviewed by / Date** | |