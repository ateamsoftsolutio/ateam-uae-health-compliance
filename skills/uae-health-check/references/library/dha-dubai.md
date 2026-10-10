# Dubai: Dubai Health Authority (DHA), plus e-claims for both emirates

> **Library status:** researched from primary and secondary sources on 2026-10-10. Each item shows its source and confidence. Laws and standards change: check the "As of" note and the regulator's current text before relying on an item, and treat "Unconfirmed" items as questions, not requirements. This library is engineering guidance, not legal advice.

Read the 'How to read this file' notes at the top of doh-abu-dhabi.md.

## B. Dubai Health Authority (DHA)

### B1. NABIDH (Dubai HIE): interoperability, onboarding and security

**DHA-01 — Standards for Interoperability and Data Exchange v2**
- Requirement: Applies to DHA-licensed facilities integrating with or accessing a Health Information System (HIS, defined to include EMR, NABIDH, claims, portals, practice management, RPM/telehealth and LIS). **HL7 v2 is the recommended standard** for accessing and submitting data. HIS **should also support FHIR and IHE**. Listed standards: HL7 v2.x, HL7 v3, HL7 CDA R2 and **HL7 FHIR R4** (sections 5.2–5.4).
- Engineering implication: Lead with HL7 v2 for NABIDH feeds, and expose a FHIR R4 API for portals, apps and future NABIDH use.
- Source: DHA "Standards for Interoperability and Data Exchange", code DHA/HISHD/ST-02, Version 2, issued 02/04/2025, effective 02/07/2025, revision 02/04/2030, sections 4–5. https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-%20Interoperability%20and%20Data%20Exchange%20Standards2022100429.pdf (the URL path says 2022 but the content is v2 2025)
- Confidence: Confirmed (primary)
- As of 2026-10: Current (effective since July 2025).

**DHA-02 — IHE profiles and FHIR APIs**
- Requirement: Facilities can use these IHE profiles: PIX (ITI-8/9/10) and PDQ (ITI-21/22) over HL7v2/MLLP; XDS.b (ITI-41/42/43/18/61) and XCA (ITI-38/39) over ebXML/SOAP; XCPD (ITI-55/56) over HL7v3/SOAP; PDQm (ITI-78), PIXm (ITI-83) and MHD (ITI-65) over FHIR/REST. XDS-SD is for scanned or PDF documents, including consent forms, and XDS-I.b registers imaging (sections 6–7). FHIR APIs cover Patient, Encounter, Allergies, Condition, Procedure, Medications/Prescriptions, Immunizations and Observations (section 8).
- Engineering implication: Implement an MLLP listener/sender with ACK handling, IHE document sharing (or a partner gateway) and FHIR resources for those functional areas.
- Source: DHA/HISHD/ST-02 v2 (2025), sections 6–8
- Confidence: Confirmed (primary)
- As of 2026-10: Current. These are written as "can use/should", so which are mandatory for a given facility is unconfirmed.

**DHA-03 — NABIDH message catalogue (Technical and Operational Standards)**
- Requirement: All facilities must comply with published NABIDH standards. Listed messages: ADT^A01–A13, A17, A20, A21, A23, A25, A27–A31, A39, A40, A45, A47; MDM^T02/T04/T11; ORM^O01; ORU^R01; RDE^O11; VXU^V04. IHE profiles listed: ATNA, BPPC, CT, PDQ, PIX, XCA, XCPD, XDS and XUA.
- Engineering implication: Use this as the conformance checklist for an HL7 interface: merges (A39/A40) and identifier changes (A47) must be handled properly. ATNA implies secure node authentication and audit trail messaging.
- Source: "NABIDH Policies and Standards – Technical and Operational Standards" https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-%20Technical%20and%20Operational%20Standards20221049100.pdf
- Confidence: Confirmed (primary). **Unconfirmed**: the HL7 v2 sub-version and the NABIDH implementation guide/Z-segments (not public). The document is undated in the text (2022 by URL).
- As of 2026-10: May be partly overtaken by Interoperability v2 (2025).

**DHA-04 — NABIDH onboarding and mandatory EMR integration**
- Requirement: Organisations must reach at least the "Essential" level of every section of the NABIDH Health Information Security Standard before onboarding. DHA Circular CIR-2025-00000101 (26 May 2025, External Circular #0954) requires implementation of an EMR system compliant with NABIDH integration requirements **and Single Sign-On (SSO) authentication**, with an attached list of facilities compelled to integrate. Process steps reported by a vendor: online readiness assessment, data sharing agreement with DHA, connectivity testing via the NABIDH developer portal, and wave-based go-live.
- Engineering implication: EMRs for Dubai need NABIDH conformance and an SSO capability. The identity provider is not confirmed; UAE PASS is plausible but unverified. Plan a security self-assessment against the NABIDH standard.
- Source: NABIDH Information Security Standards, Table 2 https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-Information%20Security%20Standards20221021940.pdf; DHA circular page https://dha.gov.ae/en/circulars/details/CIR-2025-00000101; Clinicea onboarding guide https://help.clinicea.com/en/article/steps-to-nabidh-network-analysis-backbone-for-integrated-dubai-health-certification-1l7lwgx/
- Confidence: Confirmed (primary) for the Essential-level gate and the circular's existence and subject. Confirmed (secondary) for the onboarding steps. **Unconfirmed**: the circular's detailed requirements, its deadline and which SSO is required (attachment not retrievable).
- As of 2026-10: Current.

**DHA-05 — NABIDH Health Information Security Standard (HISS)**
- Requirement: A 17-domain framework: organisation of security, policies, assets, HR, physical, communications, operations, access control, acquisition/development, incident management, continuity, audit and compliance, cryptography, supplier relationships, mobile devices, electronic bio-medical devices and cloud computing. There are three compliance levels (Essential, Intermediary, Enhanced), with Essential the minimum for all. The framework lists **DESC ISR** as an "Applicable Legislative requirement". The cloud domain says facilities **shall not use cloud services to store, process or share health information outside the UAE, including CSP backup/DR**, and that Dubai government and semi-government entities must use DESC-compliant CSPs. The CSP must have no ownership rights over data, and portability must be ensured.
- Engineering implication: For Dubai, the equivalent of ADHICS is the NABIDH HISS plus DESC alignment. Host in UAE regions with UAE DR. For government or semi-government customers (e.g. DHA facilities), use DESC-certified CSPs.
- Source: NABIDH Information Security Standards, sections 5–6 and Domain 17 (6.1.17)
- Confidence: Confirmed (primary)
- As of 2026-10: The document dates from about 2022 by URL. A newer standalone DHA health information security standard was not found (unconfirmed).

**DHA-06 — DESC ISR applicability**
- Requirement: The Dubai Electronic Security Center's Information Security Regulation (ISR) is aimed at Dubai Government entities. For private health facilities, DHA brings ISR in by reference: the NABIDH HISS lists it as applicable legislation, the consent and sharing documents cite it, and the DHA AI Policy requires compliance with DESC requirements. The Telehealth Standards require DESC-certified CSP hosting.
- Engineering implication: Treat ISR controls as expected good practice for all Dubai customers and as mandatory for government or semi-government ones. Choose DESC-certified UAE cloud regions by default.
- Source: NABIDH HISS Table 4; DHA AI Policy 4.6.7; DHA Telehealth Standards v4 7.6.7(b); International Security Journal on ISR v3.0 (secondary) https://internationalsecurityjournal.com/dubai-electronic-security-centre/
- Confidence: Confirmed (primary) for the DHA references to DESC. Confirmed (secondary) that ISR targets government entities. **Unconfirmed**: whether ISR v3.0 has been formally released and its effective date.
- As of 2026-10: Watch for ISR v3.

### B2. Consent and access control (DHA Standards for Health Information Consent and Access Control v1, 2025)

Source for DHA-07 to DHA-12: DHA "Standards for Health Information Consent and Access Control", code DHA/HISHD/ST-09, Version 1, issued 02/01/2025, effective 02/04/2025, revision 02/01/2030. https://dha.gov.ae/uploads/012025/Standards%20for%20Consnet%20and%20Access%20Control2025129762.pdf. Confidence for all six: Confirmed (primary). As of 2026-10: current.

**DHA-07 — When consent is and is not required**
- Requirement: No consent is needed to record and access PHI within the EMR for direct care (6.1) or to upload to NABIDH (8.17). Consent is required for accessing PHI through other HIS (8.2) and normally for secondary use when anonymous data is impractical (7.2). Facilities must include a NABIDH HIE Access Consent clause in their general consent (8.18) and record consent, refusal, withdrawal and re-opt-in with dates (8.3). Consent may be paper or electronic; e-signatures must follow the Dubai e-signature standard and Federal Decree-Law 46/2021 (8.4–8.5). Consent is in Arabic or English (8.7). The age of consent is 18, with guardian or next-of-kin rules (8.13–8.15). Consent is **valid for one year after the last encounter** unless revoked (8.16). Mandatory demographics are captured, including Emirates ID (8.12).
- Engineering implication: Build a consent ledger with states and dates, expiry logic (last encounter + 12 months), guardian linkage, bilingual consent templates and e-signature compliance.

**DHA-08 — Opt-out handling**
- Requirement: Patients may object to sharing through HIS, except EMR, public health portals and insurance systems (9.1–9.2). After opt-out, data continues to flow to the HIS but **patient identifiers must be anonymised**, and the data must not be retrievable via break-the-glass (9.6–9.7). Opt-out consents are archived (9.8). NABIDH opt-out goes through DHA: the patient signs the form and sends it to Nabidh.optout@dha.gov.ae (9.9). Prior shared data may be retained by recipients (9.4).
- Engineering implication: The integration layer needs an opt-out flag that switches outbound messages to anonymised identifiers and suppresses BTG retrieval.

**DHA-09 — Access provisioning and least privilege**
- Requirement: Use zero-trust and least-privilege access (5.5, 10.5) with granular, function-level access (10.2). A signed User and Confidentiality Access Agreement is required before credentials are issued (10.3–10.4), and no access before training (5.11). Every user has a standard healthcare role, and clinician roles derive from the **Sheryan** licence code (10.7, 10.11). Consent status is verified at the time of use or disclosure (11.5). Privileged accounts are managed by a common access management system (11.1). External party access is in contracts, monitored, and revoked at contract end (12).
- Engineering implication: Build a role model mapped to Sheryan licence categories, pre-access attestation, consent checks in the access decision point and a PAM integration.

**DHA-10 — Account lifecycle, reviews and log retention**
- Requirement: Logs of access approvals and account creation, modification and deletion are kept for **at least six years** (13.3). Access is cancelled on the employee's last day (13.8). Temporary access has an end date and is revoked automatically (13.11). User lists are reviewed at least every 6 months (16.4). Accounts **inactive for 40 days are disabled** (16.5). Privileged accounts are reviewed **quarterly** (16.6). Authorisation is revalidated at least yearly (10.17). All HIS access is logged and monitored, with session time-outs (16.1, 16.9). Unauthorised connection attempts are alerted (11.4).
- Engineering implication: Provide a configurable dormancy lock (default 40 days for Dubai, versus "defined duration" in ADHICS), access review reports, 6-year retention of IAM audit logs and time-boxed access grants.

**DHA-11 — Remote access**
- Requirement: Remote access for entity employees is restricted to the **UAE's geographical boundaries** (14.1). External remote access is granted from an approved list of users, locations/countries and time windows (14.3). 2FA is required for all remote access via VPN (14.5). Remote activity is logged, with **remote access logs kept for one year** (14.7–14.8). Remote access is reviewed at least every 6 months (14.12) and endpoints are secured (14.9).
- Engineering implication: Apply geo-IP conditional access, MFA, session logging and expiring vendor support accounts. Offshore vendor support needs explicit approval with country and time scoping.

**DHA-12 — Break the glass, sensitive and VIP records**
- Requirement: Only physicians in direct care may use BTG, for emergencies or serious public health threats (17.1–17.2). BTG shows a warning pop-up, requires a case-by-case justification, triggers notification, is logged with a detailed audit trail and reviewed, and must stop after the emergency (17.3–17.8). Sensitive information is flagged with icons and role-restricted (18). VIP records are flagged, treated as "SECRET", limited to care providers and **not shared with HIS** (19.3–19.9).
- Engineering implication: Build a BTG workflow with reason codes, real-time alerts to a privacy officer and post-hoc review queues; add sensitivity and VIP flags that suppress HIE feeds.

### B3. Data sharing, protection and breach (Dubai)

**DHA-13 — Data Sharing Agreements, minimum necessary and sharing logs**
- Requirement: All PHI sharing with external entities needs a Data Sharing Agreement registered by the entity's information governance team, with data flows recorded in the Information Asset Register (4.5.1–4.5.2). Only the minimum necessary PHI is shared, accuracy is checked, access is role-based, and access is monitored and audited (4.5.3–4.5.6). Audit logs of all PHI shared must be **kept for at least 6 years after the DSA ends** (4.23.7). Policies are reviewed at least every two years (4.23.2).
- Engineering implication: Build a disclosure/sharing log per recipient and DSA ID with long retention, and use field-level minimisation in exports and APIs.
- Source: DHA "Policy for Health Data and Information Sharing", Ref DHA/HISHD/PP-13, v1, issued 10 Aug 2024, effective 10 Nov 2024 https://dha.gov.ae/uploads/082024/Policy%20for%20Health%20Information%20Sharing_EN202480306.pdf
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DHA-14 — Secure channels, test data and third-party processors**
- Requirement: PHI is emailed only if encrypted or as links to an authenticated portal, from official accounts only. Email to secure-list domains is encrypted automatically; other email is encrypted manually with "(Encrypt)" in the subject line (4.10). Removable media must be encrypted and approved (4.11). Avoid PHI in system testing; if live data is unavoidable, anonymise it and **complete a DPIA before testing** (4.18). Processors within the UAE need a DSA or contract with confidentiality clauses and "sufficient guarantees", PHI in transit is encrypted, and data is not kept longer than necessary (4.19).
- Engineering implication: Use automated test-data generation, encrypted exports, portal-based sharing and processor contracts that carry DHA clauses.
- Source: DHA Policy for Health Data and Information Sharing (2024), sections 4.10, 4.11, 4.18, 4.19
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DHA-15 — Cross-border transfer prohibition (Dubai)**
- Requirement: PHI must not be transferred outside the UAE except within the categories of (Ministerial/Cabinet) Decision No. 51 of 2021 **and** with DHA approval (request to HISH@dha.gov.ae). Transfers must be encrypted in transit (4.20). The NABIDH HISS forbids cloud storage or processing outside the UAE, including backup/DR (DHA-05).
- Engineering implication: Use UAE-only hosting and processing. Any offshore analytics, AI inference or support tooling needs a legal basis under Decision 51 plus DHA approval. A federal secondary source notes that a copy must always remain in the UAE.
- Source: DHA Policy for Health Data and Information Sharing (2024) 4.20; DHA Policy for Health Data Protection and Confidentiality (HISHD/PP-11, 2022) 4.15 https://www.dha.gov.ae/uploads/082022/Health%20Data%20Protection%20and%20Confidentiality%20Policy_EN2022810559.pdf; Taylor Wessing summary of the federal rule https://www.taylorwessing.com/en/insights-and-events/insights/2021/08/me-health-data-in-the-uae-how-the-health-data-law-works-what-it-protects-and-what-exemptions-exist
- Confidence: Confirmed (primary) for the DHA rule. Confirmed (secondary) for "copy must remain in UAE". Note that DHA documents name it both "Cabinet Decision" and "Ministerial Decision" No. 51 of 2021.
- As of 2026-10: Current.

**DHA-16 — Breach notification timelines (Dubai)**
- Requirement: Report breach notifications to both the UAE Data Office ("UAE Information Office" in the text) and DHA (datacompliance@dha.gov.ae) **within 24–48 hours** (Sharing Policy 4.23.4(d)). The DHA Data Protection Policy also names HISH@dha.gov.ae, ties timing to the PDPL Executive Regulations and lists required report contents: nature, date, discovery, cause, data subject categories and numbers, severity, measures and DPO details (4.18.4–4.18.6). For NABIDH-related incidents, the facility ISO reports to NABIDH **within 48 hours** of discovery and NABIDH notifies DHA within 48 hours. Investigation completes **within 30 days** (extendable by 30), with mitigation within 180 days. Investigation reports are kept **25 years**. Public or patient notification is made within 30 days of discovery (NABIDH Incident Management and Breach Notification policy 3.2.19–3.2.29, 3.3.7).
- Engineering implication: Design for detect-to-notify within 24 hours. Provide breach scoping queries and evidence export, and retain incident records for 25 years.
- Source: DHA Sharing Policy (2024); DHA Data Protection and Confidentiality Policy (2022); NABIDH Incident Management and Breach Notification policy https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-%20Incident%20Management%20and%20Breach%20Notification%20policy20221034427.pdf
- Confidence: Confirmed (primary). The email addresses differ between documents (datacompliance@ vs HISH@); confirm with DHA.
- As of 2026-10: Current. Note the federal PDPL Executive Regulations status separately.

### B4. AI (Dubai)

**DHA-17 — DHA Policy for Use of AI in Healthcare: design obligations**
- Requirement: AI must be fair and free of bias (4.1). It must have built-in appeals procedures so users can challenge significant decisions (4.2.3), and failures must be traceable, diagnosable and controllable (4.3.1). Mandatory disclosure covers: AI-enabled features, validation method, datasets, expected outcomes, the clinician's role in final decisions, algorithm type, ethical data use, performance validation, integration and limitations (4.3.2). **Independent validation by a competent third party** is required (4.3.3). High-impact AI must allow designated users to override or reverse decisions (4.4.2). It needs graceful degradation (4.6.13), auditable validation statements (4.6.12), monitoring of real-world performance against training and validation results (4.6.11), and use of current data and interoperability standards (4.6.15). It must comply with DESC requirements and Federal Law 2/2019 (4.6.2, 4.6.7).
- Engineering implication: Ship a model fact sheet covering the 4.3.2 items, an independent validation report, override and appeal UI, and performance monitoring.
- Source: DHA "Policy for Use of Artificial Intelligence in the Healthcare in the Emirate of Dubai", Ref DHA/HISHD/PP-09, v1, issued 01/07/2021 (effective date shown as 1/7/2021 on the cover and 01/10/2021 in the page table), revision 01/07/2026 https://www.dha.gov.ae/uploads/082022/Artificial%20Intelligence%20Policy20228457.pdf
- Confidence: Confirmed (primary)
- As of 2026-10: **The revision date (July 2026) has passed.** No successor DHA AI policy or guideline was found (unconfirmed). Build to the 2021 policy plus the telehealth ambient-AI rules (DHA-21).

**DHA-18 — DHA AI Policy: operational duties for end users**
- Requirement: End users report known or suspected AI incidents and deficiencies, and implementation issues affecting safety, to DHA-HRS (4.8.1–4.8.2). They set boundaries on patient data access and sharing (4.8.3), conduct **regular audits of AI functionality by an independent third party** with reporting (4.8.5), and submit user feedback to DHA-HRS and the manufacturer (4.8.6). Developers document risk–benefit analysis, technical specifications, hazards and KPIs (4.7).
- Engineering implication: Include in-product incident and feedback reporting, export of audit evidence for third-party auditors, and risk–benefit documentation.
- Source: DHA AI Policy (2021), sections 4.7–4.8
- Confidence: Confirmed (primary)
- As of 2026-10: As DHA-17.

### B5. Telehealth (Dubai)

Source for DHA-19 to DHA-22: DHA "Standards for Telehealth Services", code DHA/HRS/HPSD/ST-14, **Version 4**, issued 26/09/2025, effective 26/11/2025, revision 26/09/2030. https://dha.gov.ae/uploads/012023/Standards%20for%20Telehealth%20Services2023158613.pdf (the URL path says 2023 but the content is v4 2025). Related: DHA Telehealth Policy DHA/HRS/HPSD/HP-20, effective 22/08/2021 (not reviewed in detail). Confidence for all four: Confirmed (primary). As of 2026-10: current, effective since Nov 2025.

**DHA-19 — Telehealth platform approval and hosting**
- Requirement: Telehealth platforms, whether internal or commercial, must be assessed and approved by DHA before implementation (7.6.2). Requirements include legal representation in Dubai and an assigned Business Technical Director (7.6.3–7.6.4), TDRA-approved communication channels (7.6.6), data centres at least **Tier 3 certified**, all data stored on servers at a **DESC-certified CSP in the UAE**, and **HIPAA compliance certification and ISO 27001** certification. Some applicants may need HITRUST or SOC 2 (7.6.7). Changes to core functions, data practices or ownership need DHA approval (7.7). Platforms may not deliver clinical services themselves (7.8).
- Engineering implication: Choose a DESC-certified UAE region with Tier III facilities, budget for ISO 27001 and third-party HIPAA attestation, and run a change-control process that notifies DHA.

**DHA-20 — Telehealth infrastructure and NABIDH integration**
- Requirement: IT infrastructure must integrate with the provider's EHR, and the health records system **shall integrate with NABIDH** (7.9.1(e)). Servers must be secure and in the UAE with backup (7.9.1(h)). Systems must record and document patient information and protect stored EHR data (7.9.1(i)–(j)). The EHR must guarantee patient identification and authentication, including voice recording for quality purposes (8.2.6). Data must be available to DHA in the required format (8.2.7). Storage, development or transfer outside the UAE is prohibited unless DHA approves (8.2.5(d)).
- Engineering implication: Telehealth must write back to the EHR, which then feeds NABIDH. Run UAE-hosted media servers and recordings, and provide regulator export.

**DHA-21 — Telehealth consent, identification and ambient AI**
- Requirement: Embed consent in the workflow; e-consent is valid if it meets manual consent and identification requirements and is auditable (10.1). Consent identifies the patient with **at least three identifiers** (full name, date of birth, Emirates ID, health record number or contact number) and covers security measures, recording, data storage and transfer, and tracking mechanisms (10.2, 10.4.3). Consent for treatment is documented at each encounter (10.4.2). **Ambient AI documentation needs patient consent.** The physician must review and e-sign AI-generated content before it enters the permanent record, AI tools need documented clinical validation and disclosure, and data must stay on UAE-based infrastructure unless law permits otherwise (10.6).
- Engineering implication: Use a three-identifier match at session start and per-encounter consent capture. AI scribe drafts must stay in a draft state until clinician sign-off, with provenance flags.

**DHA-22 — Recording and documentation**
- Requirement: Ad hoc video recording needs documented purpose, a responsible person, capture/anonymisation/storage protocols, encryption and backup, written consent, a **defined storage duration and deletion**, and a breach risk plan (9.7). Every encounter record includes service type and platform, clinician names, licence numbers and roles, date, time and locations, consent confirmation, and the clinical content (11.6). Telephone consultations are written into the EHR (11.5).
- Engineering implication: Build recording retention policies with automatic deletion, encryption and structured telehealth encounter fields.

### B6. Retention and localisation (Dubai)

**DHA-23 — Health record retention**
- Requirement: DHA policy defers to the UAE ICT Health Law and DHA rules for retention periods, and PHI must not be kept longer than necessary, then securely disposed of (DHA Data Protection Policy 4.3.6). The federal ICT Health Law sets a minimum retention of 25 years from the last procedure (secondary source). DHA-specific log retentions: IAM provisioning logs at least 6 years (DHA-10), remote access logs 1 year (DHA-11), sharing logs 6 years after DSA end (DHA-13), NABIDH investigation reports 25 years (DHA-16).
- Engineering implication: Set the default clinical record retention to at least 25 years after the last encounter, with legal-hold support. Set the log retentions per the listed DHA values.
- Source: DHA Data Protection and Confidentiality Policy (2022) 4.3.6; Taylor Wessing summary (federal 25 years); DHA documents cited above
- Confidence: Confirmed (primary) for the DHA log periods. Confirmed (secondary) for the 25-year federal minimum.
- As of 2026-10: The DHA Telehealth Standards v4 refer to a "Dubai Health Data Law" (11.1.1). Its existence, number and content are **unconfirmed**.

---

## C. Claims and e-claims integration (brief)

**CLM-01 — Abu Dhabi: DoH Shafafiya e-claims data dictionary**
- Requirement: DoH publishes e-claims technical standards under "Shafafiya". These include XSD schemas for Claim Submission, Remittance Advice, Person Register, Prior Request/Authorization, Electronic Prescription and Cost Submission; web services (UploadTransaction, GetNewTransactions, SearchTransactions, GetPrescriptions and others); code sets (ICD-10 2021, CPT/HCPCS, drugs, LOINC, SNOMED CT); and a public test environment. The DoH Tele-Medicine Standard points to this dictionary for telehealth billing.
- Engineering implication: RCM modules for Abu Dhabi integrate with the DoH transaction web services using the published XSDs, tested in the public test environment.
- Source: https://www.doh.gov.ae/en/Shafafiya/dictionary; DoH Tele-Medicine Standard 11.10
- Confidence: Confirmed (primary) for the Shafafiya name and contents. **Unconfirmed**: the name of the Abu Dhabi claims exchange endpoint ("post office") and its current version.
- As of 2026-10: Current.

**CLM-02 — Dubai: eClaimLink mandatory electronic claims**
- Requirement: eClaimLink is DHA's claims portal (with Dimensions Healthcare) and publishes mandated coding standards and data sets. A Health Insurance Claims Management Policy Directive from the Dubai Health Insurance Corporation (effective 16 Nov 2025) requires all claims to be submitted electronically via eClaimLink, prohibits manual submission, and requires pre-authorisation requests within 1 hour of the physician's order. Payer response times are 6h (elective outpatient), 24h (elective inpatient) and immediate (emergency).
- Engineering implication: RCM for Dubai needs eClaimLink integration with near-real-time prior authorisation submission.
- Source: https://eclaimlink.ae/about.aspx; Al Tamimi summary https://www.tamimi.com/news/dubai-issues-comprehensive-overhaul-of-health-insurance-claims-management-framework/
- Confidence: Confirmed (primary) that eClaimLink exists and is DHA-operated. Confirmed (secondary) for the 2025 directive's terms. The directive number and the "DHPO" technical specification are **unconfirmed**.
- As of 2026-10: Current.

---

## Watch items (as of 2026-10-10)

1. **Sahatna integration deadline: 30 Nov 2026** (DoH Circular 2026/170). This is the only open hard deadline found.
2. **DoH Responsible AI Standard revision due Oct 2026.** The DoH AI policy (June 2025) and AI Risk Management protocol (July 2025) referenced by the RAI Standard were not retrieved; obtain them.
3. **DHA AI Policy revision date of 1 Jul 2026 has passed** with no successor found. Watch DHA circulars for a replacement or AI guidelines.
4. **ADHICS implementation guideline (v2):** obtain the IM 2 Incident Reporting Matrix (SOC notification timelines) and the Data Breach Form. ADHICS v2 itself is due for revision in May 2027.
5. **Malaffi technical specifications** (HL7 v2 sub-version, REF trigger events, Sahatna API) are not public; get them through onboarding@malaffi.ae.
6. **DHA Circular CIR-2025-00000101** (NABIDH-compliant EMR plus SSO): obtain the attachment for the facility list, the SSO method and any deadline.
7. **DESC ISR v3.0:** release and effective date unconfirmed. Re-check DHA HISS alignment when published.
8. **"Dubai Health Data Law"** referenced in the DHA Telehealth Standards v4: confirm whether a Dubai-level health data law has been issued.
9. **Conflicting older rules:** the HIE Policy 2020 (8-character passwords, 5-business-day breach notice) and the 2020 Data Privacy Standard (24-hour breach) are superseded or overtaken by ADHICS v2. Apply the stricter or newer rule.
10. **Offshore delivery risk:** ADHICS SA 2.1(8) (no SDLC activity outside the UAE), DP 1.4(3) (access only from within the UAE), RAI 3.1.4.5.1 and DHA remote access 14.1 all restrict offshore engineering and support. An ATeam delivery model with India-based engineers touching Abu Dhabi or Dubai PHI environments needs DoH/DHA exemptions or UAE-resident operations. This conclusion is drawn from the cited text and is not a regulator statement.

## What could not be confirmed

- ADHICS severity-based incident reporting timelines to the Abu Dhabi Health SOC (in the separate guideline).
- How the AAMEN portal works (self-assessment and certification process) under v2.
- Whether patients can opt out of Malaffi (no provision found).
- Malaffi HL7 v2 sub-version and any FHIR route. NABIDH HL7 v2 sub-version and implementation guide.
- The content of DoH's 2025 AI policy and AI Risk Management protocol.
- Any newer DoH telemedicine or remote-care standard after 2020.
- The detailed requirements of DHA Circular CIR-2025-00000101.
- Whether DESC ISR v3.0 has been released, and whether a "Dubai Health Data Law" exists.
- The Abu Dhabi e-claims post office name, the DHPO specification, and the Dubai claims directive number.

## Primary sources consulted

- ADHICS v2 (2024): https://www.doh.gov.ae/-/media/Feature/Resources/Standards/ADHICS-v2-standard.ashx
- ADHICS FAQ v1.3: https://www.doh.gov.ae/-/media/Feature/Aamen/ADHICS-FAQ.ashx
- AAMEN programme page: https://www.doh.gov.ae/en/programs-initiatives/Aamen
- DoH Circular 116/2020 (Patient Healthcare Data Privacy): https://www.doh.gov.ae/-/media/Feature/Resources/Circulars/116--20201.ashx
- DoH Policy on the Abu Dhabi HIE (2020): https://doh.gov.ae/-/media/A78104416DF2440E89AC90FB75F11055.ashx
- DoH Circular 2026/10 (Malaffi minimum dataset): https://www.doh.gov.ae/-/media/84A213F71AA545D69E01A23A0A99C598.ashx
- DoH Circular 2026/144 (referrals): https://www.doh.gov.ae/-/media/C01533BB56EE4A7D9B75107C89656941.ashx
- DoH Circular 2026/170 (Sahatna): https://www.doh.gov.ae/-/media/73037D74C0A441AA8C69CF8E233FFE3E.ashx
- Malaffi facilities required to connect: https://www.malaffi.ae/providers/facilities-required-to-connect/
- DoH Responsible AI Standard (2025): https://www.doh.gov.ae/-/media/B84FF5DD027846699C51F36C439A580F.ashx
- DoH AI Policy (2018): https://www.doh.gov.ae/-/media/E9C1470A575146B18015DEBE57E47F8D.ashx
- DoH Standard on Tele-Medicine (2020): https://www.doh.gov.ae/-/media/Feature/Resources/Standards/Telemedicine-Standard-Final-with-Appendices.ashx
- DoH Shafafiya dictionary: https://www.doh.gov.ae/en/Shafafiya/dictionary
- DHA Interoperability and Data Exchange v2 (2025): https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-%20Interoperability%20and%20Data%20Exchange%20Standards2022100429.pdf
- DHA Consent and Access Control v1 (2025): https://dha.gov.ae/uploads/012025/Standards%20for%20Consnet%20and%20Access%20Control2025129762.pdf
- NABIDH Technical and Operational Standards: https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-%20Technical%20and%20Operational%20Standards20221049100.pdf
- NABIDH Information Security Standards: https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-Information%20Security%20Standards20221021940.pdf
- NABIDH Incident Management and Breach Notification: https://www.dha.gov.ae/uploads/102022/NABIDH%20Policies%20%26%20Standards%20-%20Incident%20Management%20and%20Breach%20Notification%20policy20221034427.pdf
- DHA Policy for Health Data and Information Sharing (2024): https://dha.gov.ae/uploads/082024/Policy%20for%20Health%20Information%20Sharing_EN202480306.pdf
- DHA Health Data Protection and Confidentiality Policy (2022): https://www.dha.gov.ae/uploads/082022/Health%20Data%20Protection%20and%20Confidentiality%20Policy_EN2022810559.pdf
- DHA AI Policy (2021): https://www.dha.gov.ae/uploads/082022/Artificial%20Intelligence%20Policy20228457.pdf
- DHA Standards for Telehealth Services v4 (2025): https://dha.gov.ae/uploads/012023/Standards%20for%20Telehealth%20Services2023158613.pdf
- DHA Circular CIR-2025-00000101: https://dha.gov.ae/en/circulars/details/CIR-2025-00000101
- eClaimLink: https://eclaimlink.ae/about.aspx
