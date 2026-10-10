# Federal layer (applies across the UAE)

> **Library status:** researched from primary and secondary sources on 2026-10-10. Each item shows its source and confidence. Laws and standards change: check the "As of" note and the regulator's current text before relying on an item, and treat "Unconfirmed" items as questions, not requirements. This library is engineering guidance, not legal advice.



Research date: 2026-10-10. Scope: federal law and MOHAP (Northern Emirates: Sharjah, Ajman, Umm Al Quwain, Ras Al Khaimah, Fujairah). Emirate-level regimes (DoH Abu Dhabi, DHA Dubai, DHCC, DIFC, ADGM) are out of scope here. They add stricter rules on top of this layer.

## How to read this file

- **IDs** are librarys (FED-xx for federal, MOH-xx for MOHAP-specific). Renumber when the library is merged.
- **Confidence**
  - *Confirmed (primary)*: read in the official English text on uaelegislation.gov.ae, elaws.moj.gov.ae, ede.gov.ae, mohap.gov.ae, riayati.mohap.gov.ae, ehs.gov.ae or u.ae. On these portals the Arabic text prevails over the English translation.
  - *Confirmed (secondary)*: taken from a reputable law-firm or professional summary. The primary text was not read.
  - *Unconfirmed*: no reliable source found, or the sources disagree.
- Quotations from statutes are short extracts. Everything else is paraphrased.
- This is engineering guidance, not legal advice. Confirm with UAE counsel before relying on a point for a contract or a regulatory filing.

### Key instruments

| Short name | Instrument | Status (as of 2026-10) |
|---|---|---|
| ICT Health Law | Federal Law No. (2) of 2019 Concerning the Use of ICT in Health Fields. Issued 06 Feb 2019, Official Gazette No. 647, effective 14 May 2019 | Active, no amendments listed on uaelegislation.gov.ae |
| ICT Exec Reg | Cabinet Resolution No. (32) of 2020 (Executive Regulation of the ICT Health Law). Issued 22 Apr 2020, Gazette No. 677, effective 30 Oct 2020 | Active |
| MR 51/2021 | MOHAP Ministerial Resolution No. 51 of 2021 (exceptions to Art. 13 local storage). Issued 28 Apr 2021, Gazette No. 702, effective 16 May 2021 (dates per Al Tamimi) | In force (secondary sources) |
| PDPL | Federal Decree-Law No. (45) of 2021 on the Protection of Personal Data. Issued 20 Sep 2021, effective 02 Jan 2022 | Active. Executive Regulations **not issued** (see Watch items) |
| Medical Products Law | Federal Decree-Law No. (38) of 2024 Governing Medical Products, Pharmacists and Pharmaceutical Establishments. Issued 01 Oct 2024, Gazette No. 785, effective 02 Jan 2025 | Active |
| EDE Law | Federal Decree-Law No. 28 of 2023 establishing the Emirates Drug Establishment (EDE), in force 30 Sep 2023 (per Baker McKenzie and the uaelegislation related-legislation list) | Active |
| Telehealth rules | Cabinet Resolution No. (40) of 2019 (Executive Regulations of Federal Decree-Law No. 4 of 2016 on Medical Liability), Annex "Rules & Conditions for Providing Telehealth Services". Issued 02 Jul 2019, effective 16 Jul 2019 | Active per uaelegislation.gov.ae |
| Cybercrime Law | Federal Decree-Law No. 34 of 2021 on Countering Rumours and Cybercrimes. Amended by Federal Law No. 5 of 2024 | Active |

---

## A. ICT Health Law (Federal Law No. 2 of 2019): core obligations

### FED-01: The ICT Health Law applies to every health ICT system in the UAE, including free zones
- **Requirement:** The law applies to all ICT methods and uses in health fields in the State, including Free Zones. A "Competent Entity" is any entity in the State providing medical services, health insurance or national health insurance.
- **Engineering implication:** Assume the law applies to any system that creates, stores or moves health data about care delivered in the UAE, including SaaS run from a free-zone company and systems run by vendors for providers or insurers. Record "jurisdiction = UAE" as a tenant attribute so localisation, retention and consent rules switch on automatically.
- **Source:** ICT Health Law, Art. 1 (definitions) and Art. 2 (scope). https://elaws.moj.gov.ae/UAE-MOJ_LC-En/00_Health/UAE-LC-En_2019-02-06_00002_Kait.html?val=EL1 ; status at https://uaelegislation.gov.ae/en/legislations/1209
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Active, with no amendments listed on the federal legislation portal.

### FED-02: Keep health data confidential and share it only in authorised cases
- **Requirement:** Entities must "keep all health data and information confidential and … allow the circulation thereof only in authorised cases". Users of the Central System must share only the information that is necessary, limit access to authorised persons and not change or disseminate data without approval.
- **Engineering implication:** Default-deny access control. Every disclosure path (API, export, print, report, integration feed) needs an authorisation check and an audit event. Minimise the fields in outbound payloads. No unaudited bulk export.
- **Source:** ICT Health Law, Art. 4(1). Central System user duties in Arts. 8 and 15. URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-03: Protect the integrity of health data and keep it available to authorised parties
- **Requirement:** Entities must ensure "the validity and credibility of the health data and information, by protecting the integrity thereof" and ensure its availability to authorised parties.
- **Engineering implication:** Tamper-evident audit logs, versioned clinical records (no destructive updates), integrity checks on stored data and on transfers, and high availability with disaster recovery sized to clinical need. Define RPO/RTO per module.
- **Source:** ICT Health Law, Art. 4(2) and 4(3). URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-04: Store and process health data inside the UAE (data localisation)
- **Requirement:** Health data and information related to health services provided in the State "may not be stored, processed, generated or transferred outside the State, unless in the cases defined" by a decision of the Health Authority in coordination with the Ministry. Breaching Art. 13 carries a fine of AED 500,000 to AED 700,000.
- **Engineering implication:** Host primary databases, replicas, backups, logs and search indexes holding PHI, analytics stores, file and object storage, and **AI/LLM inference** in UAE regions. Inventory every sub-processor (email, SMS, monitoring/APM, error tracking, support desks, LLM APIs, transcription). Any that receives PHI outside the UAE is a transfer that must fit an MR 51/2021 exception (FED-21 to FED-24). Enforce region pinning in infrastructure-as-code and add an egress allow-list. Note that "processed" and "generated" are covered, so offshore processing with no storage is still caught.
- **Source:** ICT Health Law, Art. 13. Penalty in Art. 24. URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current. Exceptions in MR 51/2021. Emirate regulators can impose stricter localisation (for example Abu Dhabi ADHICS), which is out of scope here.

### FED-05: Storage conditions inside the UAE are set by Ministerial decision
- **Requirement:** Conditions for storing health data inside the State are to be set by a decision of the Minister.
- **Engineering implication:** Track this as a pending configuration input. Until it is identified, apply the ICT Exec Reg storage controls (FED-18, FED-19) and the Riayati security standard (MOH-03).
- **Source:** ICT Health Law, Art. 12. URL as FED-01.
- **Confidence:** Article confirmed (primary). **Unconfirmed:** the number and content of any Ministerial decision issued under Art. 12.
- **As of 2026-10:** No implementing decision located.

### FED-06: Do not use patient information for non-health purposes without written consent, except in listed cases
- **Requirement:** Patient information must not be used for non-health purposes without the patient's written approval. Exceptions: (1) information required by health insurers or other health funding entities; (2) scientific and clinical research, provided patients' identity is not disclosed; (3) preventive and curative public-health measures; (4) requests from competent judicial entities; (5) Health Authority requests for control, inspection and public-health protection.
- **Engineering implication:** Tag each data use with a purpose. Block secondary uses (marketing, product analytics on identifiable data, model training on identifiable data) unless a recorded written consent or a listed exception applies. Store consent artefacts (versioned text, timestamp, signer, channel). Provide a de-identification pipeline for research datasets and a workflow for logging judicial and regulator requests.
- **Source:** ICT Health Law, Art. 16. URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-07: Keep health data for at least 25 years from the last health procedure
- **Requirement:** Health information and data must be kept for "no less than (25) twenty-five years as of the date of the last health procedures", while maintaining confidentiality, validity and credibility.
- **Engineering implication:** Compute the retention clock per patient from the **latest** encounter or procedure date, and recompute it on every new encounter. Do not auto-purge clinical data before the date. Plan for 25+ years of format and media migration (open formats, checksums, periodic restore tests), and make vendor exit and data return clauses deliver the full record. Deletion requests must be checked against this retention floor.
- **Source:** ICT Health Law, Art. 20. URL as FED-01. The ICT Exec Reg, Art. 7, allows archiving for research and public health after the preservation period, with privacy preserved.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current. 25 years is the federal minimum. Telehealth rules (MOH-07) state no separate period.

### FED-08: Record the Emirates ID (Identity Number) in all health transactions and files
- **Requirement:** The Health Authority and Competent Entities must include the Identity Number in all health transactions, registers and files. Emergencies and cases set by Ministry decision are exempt.
- **Engineering implication:** Make Emirates ID a mandatory, validated patient-identity field. Provide an "emergency/unidentified" override with a reason code and a later reconciliation workflow. Include the ID in claims, referrals and exchange messages as the receiving system requires.
- **Source:** ICT Health Law, Art. 21. URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current. The Ministry decision listing other exempt cases was not located (unconfirmed).

### FED-09: Systems must be compatible and interoperable, and follow health-authority controls
- **Requirement:** Electronic systems must be compatible and interoperable so health data can be exchanged. Health Authorities set the principles and controls for electronic systems (operation, exchange, protection, access, copying, changes, risk management). The Ministry sets technical standards and device-connection controls.
- **Engineering implication:** Build on standards-based interfaces (HL7 v2, FHIR, CDA, standard code sets; see MOH-02) and avoid proprietary-only exchange. Keep a mapping layer so each regulator's specification can be met.
- **Source:** ICT Health Law, Arts. 6, 10 and 11. URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

## B. ICT Executive Regulation (Cabinet Resolution No. 32 of 2020): Central System controls

### FED-10: Connect to the national Central System and meet national digital health standards
- **Requirement:** Health authorities must follow the Central System's work rules, meet the Ministry's joining deadline, bear connection costs and meet national digital health standards, including mechanisms that protect confidentiality. The Ministry may audit personal health data for authenticity, quality and compliance.
- **Engineering implication:** Budget for and build the HIE connector (Riayati for MOHAP-licensed facilities, see MOH-01). Validate data quality before submission (mandatory fields, code-set validity, identity matching) and keep submission logs so audits can be answered.
- **Source:** ICT Exec Reg, Art. 2 (and Art. 3, joint committee). https://uaelegislation.gov.ae/en/legislations/1444
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Active. The Exec Reg itself sets no specific deadline. Deadlines come from Ministry decisions.

### FED-11: Grant access on a need-to-know basis tied to professional role, and review it periodically
- **Requirement:** Access is granted "on an as-needed basis", with access levels tied to each person's professional role and patient-care duties. Access must be audited periodically and privileges removed or changed as work requires.
- **Engineering implication:** RBAC (or attribute-based access control, ABAC) with clinical-relationship scoping (for example a care team or encounter link). Run scheduled access-recertification reports and alert on access outside the care relationship.
- **Source:** ICT Exec Reg, Art. 4. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-12: Authorise every user individually, including outsourced staff, and end access when service ends
- **Requirement:** No person may use the system unless authorised. Authorisation can extend to employees, outsourced staff, contracted experts, consultants and agency staff when their work requires it, limited to actual need. Authorities decide who may access remotely and must ensure access ends when service ends.
- **Engineering implication:** Named accounts only (no shared or generic accounts). Expiry dates for contractor and vendor accounts. Automate joiner, mover and leaver changes from HR or contract systems. Control remote access separately (for example a VPN or zero-trust policy with MFA). Vendor support access needs approval and time limits.
- **Source:** ICT Exec Reg, Art. 5. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-13: Let patients delegate access and restrict or prohibit access to their record
- **Requirement:** Patients may give others access to their health information, and those people must be registered users. Patients may ask to prohibit or restrict access to their information.
- **Engineering implication:** Build a consent-directive model: proxy or delegate access for registered users, and patient-level restrictions (for example blocking named clinicians or facilities, or sensitive sections). Enforce restrictions in every read path and in HIE queries, with a defined emergency override (FED-14).
- **Source:** ICT Exec Reg, Art. 5. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-14: Allow emergency ("break-glass") access without consent, and record the reason
- **Requirement:** Disclosure without patient consent is prohibited unless the law permits it. In an emergency where consent cannot be obtained, providers may examine the patient file for care purposes and must record the reasons.
- **Engineering implication:** A break-glass function that requires a reason, time-limits access, raises a high-priority audit event and triggers retrospective review.
- **Source:** ICT Exec Reg, Art. 6. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-15: Corrections must keep the original entry and record who changed what, when and why
- **Requirement:** Corrections retain the original entry. Each modification stores the reason, the changed information, the date and the modifier's electronic signature, and changes must be trackable.
- **Engineering implication:** Append-only amendment model for clinical documentation: no overwrite or hard delete. Capture user identity and e-signature, timestamp, reason code and before/after values, and show full history in the UI and exports.
- **Source:** ICT Exec Reg, Art. 6. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-16: Secure endpoints, encrypt email containing patient data, and do not share credentials
- **Requirement:** Files must not be left open unattended and devices should be switched off when not in use. Suspicious activity must be reported. Emails containing patient data must be encrypted. Each authorised user has individual credentials that must not be shared. All necessary steps must be taken against loss, misuse, unauthorised access, disclosure, modification or destruction.
- **Engineering implication:** Idle-session timeout and screen lock, encrypted email or secure-message delivery for any outbound PHI (avoid plain SMTP attachments), a user-facing "report suspicious activity" path feeding the incident process, unique logins with MFA, and detection of concurrent-session anomalies.
- **Source:** ICT Exec Reg, Art. 6. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current. No specific algorithms or key lengths are prescribed.

### FED-17: Do not publish national health statistics without Ministry approval, or identity data without patient consent
- **Requirement:** Health data and statistics cannot be published nationwide without Ministry approval. Patient consent is required before publishing identity data. The Minister sets the list of identity data.
- **Engineering implication:** Gate public dashboards and open-data exports behind an approval workflow. Apply aggregation and small-cell suppression. Never include identity data in publications without recorded consent.
- **Source:** ICT Exec Reg, Art. 6. URL as FED-10.
- **Confidence:** Confirmed (primary). **Unconfirmed:** the Ministerial list of "identity data".
- **As of 2026-10:** Current.

### FED-18: Keep backups secure and recoverable, test recovery regularly, and maintain continuity plans
- **Requirement:** Backups must be secure and recoverable and regularly reviewed and updated. Recovery tests must be run periodically. Risk-management and business-continuity plans are required. Storage must follow facility medical-records and archiving rules, at minimum meeting Ministry controls.
- **Engineering implication:** Encrypted, UAE-resident backups (FED-04), immutable or offline copies, scheduled restore drills with evidence kept, and a documented BCP/DR plan with tested RTO/RPO.
- **Source:** ICT Exec Reg, Art. 7. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current. No test frequency is specified.

### FED-19: Patient withdrawal from the Central System, and archiving after the preservation period
- **Requirement:** The Central System should hold all patient files in the State. A patient may withdraw, in which case their data may be kept in unidentified form. After the preservation period, data may be archived for research and public health while protecting privacy.
- **Engineering implication:** Support a "withdrawn" status that de-identifies a patient's data in shared repositories rather than deleting it. Build an archive tier with de-identification for records past retention.
- **Source:** ICT Exec Reg, Art. 7. URL as FED-10.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-20: Sanctions for breaching the ICT Health Law
- **Requirement:** Disciplinary sanctions by the Health Authority: oral or written warning, a fine of AED 1,000 to AED 1,000,000, suspension of Central System access for up to 6 months, or cancellation of authorisation. Art. 13 (localisation) breach: AED 500,000 to 700,000. Unlicensed health advertising on the Central System: AED 100,000 to 200,000. Grievances: file within 15 days. The committee decides within 30 days (silence means rejection), and a court appeal is possible within 30 days.
- **Engineering implication:** Suspension of Central System access can stop clinical operations that depend on the HIE, so treat compliance evidence (audit logs, access reviews, transfer register, consent records) as a deliverable and keep it exportable on demand.
- **Source:** ICT Health Law, Arts. 23 to 26. URL as FED-01.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

## C. Ministerial Resolution No. 51 of 2021: exceptions to local storage

Note: the gazette text was not read directly. Al Tamimi lists 8 exception categories. Hogan Lovells and Baker McKenzie describe 10, adding telemedicine and patient request. The conditions below follow Hogan Lovells, cross-checked against CMS and Al Tamimi.

### FED-21: Offshore transfers must fit a listed exception and meet its conditions (consent, minimum data, encryption, UAE copy)
- **Requirement:** Health data may be stored or processed outside the UAE only under a listed exception: treatment abroad; samples sent to overseas laboratories; scientific research; insurance coverage and claims; cooperation with competent authorities; simple personal medical devices and wearables; pharmacovigilance; transfers approved by the health authority; telemedicine; a formal patient request. Most categories require **written patient consent**, disclosure only to notified recipients, **only the data necessary**, and **encryption to the highest standard**. Several (cooperation with authorities, pharmacovigilance, health-authority-approved transfers, patient request, and per CMS telemedicine) require **a copy, and the consents, to be kept in the UAE**.
- **Engineering implication:** Build a "cross-border transfer gateway": every offshore flow is registered with its exception category, legal basis, recipient, data fields and encryption method. The gateway enforces consent presence, field minimisation and encryption, keeps a UAE-resident copy where required, and writes an audit record. Default is block.
- **Source:** MR 51/2021, per Al Tamimi (https://www.tamimi.com/news/health-data-transfers-outside-the-uae/), Hogan Lovells (https://engage.hoganlovells.com/knowledgeservices/news/the-uaes-health-data-law-an-important-and-welcome-update-permitting-certain-transfers-of-health-data) and CMS (https://cms.law/en/are/legal-updates/uae-health-data-new-data-transfer-permissions).
- **Confidence:** Confirmed (secondary). The exact count (8 or 10) and per-category conditions are **unconfirmed against the gazette**.
- **As of 2026-10:** No amendment found. "Highest standard" encryption is not defined. Use current strong cryptography (for example TLS 1.2+ in transit and AES-256 at rest) as an engineering choice, not a legal citation.

### FED-22: Telemedicine exception: time-limited access for the remote doctor and direct delivery of reports
- **Requirement:** For telemedicine involving an offshore professional, the doctor's system access must be limited to a specific, short period and to the scope needed. Reports or images go only directly to the relevant professional. Written patient consent is required.
- **Engineering implication:** Time-boxed, case-scoped guest access for remote clinicians (auto-expiry), direct secure delivery (no forwarding or download where possible), and a consent capture step before the session.
- **Source:** MR 51/2021 per Hogan Lovells and CMS (URLs as FED-21).
- **Confidence:** Confirmed (secondary). Not listed by Al Tamimi, so **partially unconfirmed**.
- **As of 2026-10:** As above.

### FED-23: Research and insurance exceptions: anonymisation, approvals and minimum fields
- **Requirement:** Research transfers must be anonymised, go to an authorised entity, be encrypted, be used only for the research, comply with UAE research rules and be **approved by the relevant health authority**. Insurance transfers must involve UAE-licensed insurers or claims managers, keep all data stored in the UAE, transfer only the specific information required (not full records), and have written consent. The policy number may leave the UAE only where treatment or claims processing is overseas.
- **Engineering implication:** Research export tooling must produce de-identified datasets and require an approval reference before release. Claims integrations to offshore TPAs or group entities must send minimum fields only and keep the master record in the UAE.
- **Source:** MR 51/2021 per Hogan Lovells and CMS (URLs as FED-21).
- **Confidence:** Confirmed (secondary)
- **As of 2026-10:** As above.

### FED-24: Personal devices and wearables, and patient-requested transfers
- **Requirement:** Data from simple personal medical devices (for example blood-pressure, glucose or SpO2 monitors) is an exception with no specific conditions listed. A formal patient request requires written consent that details the information and recipient, minimum data, highest-standard encryption and a copy stored in the UAE.
- **Engineering implication:** Consumer-device data flowing to an offshore device cloud may be permissible, but once it enters a UAE provider's clinical record, FED-04 applies to that record. "Send my record abroad" features need a structured request form, recipient verification, encryption and a retained UAE copy.
- **Source:** MR 51/2021 per Al Tamimi and Hogan Lovells (URLs as FED-21).
- **Confidence:** Confirmed (secondary)
- **As of 2026-10:** As above.

## D. PDPL (Federal Decree-Law No. 45 of 2021)

### FED-25: Classify data by regime: health data under the ICT Health Law, other personal data under the PDPL
- **Requirement:** The PDPL does not apply to personal health data "that has legislation regulating its protection and processing", which is the ICT Health Law's territory. The PDPL defines Sensitive Personal Data to include health, genetic and biometric data. Where the PDPL applies, it allows processing without consent for medical purposes (preventive or occupational medicine, diagnosis, treatment, health insurance) and for public health.
- **Engineering implication:** Healthcare systems still hold PDPL-scope data: non-clinical personal data (website and app users, marketing leads, staff and HR, payment contacts) and possibly health-adjacent data not covered by the ICT Health Law (for example wellness apps outside regulated care, unconfirmed). Keep a data inventory that tags each field with its regime, and apply the stricter control where unsure.
- **Source:** PDPL, Arts. 1, 2(2) and 4. https://uaelegislation.gov.ae/en/legislations/1972 (English download read)
- **Confidence:** Confirmed (primary). How far the health exclusion reaches in practice is **unconfirmed** (case-specific).
- **As of 2026-10:** Active.

### FED-26: Where the PDPL applies: security measures, breach notification, DPIA and DPO
- **Requirement:** Controllers and processors must apply appropriate security measures (for example encryption, pseudonymisation, resilience, timely recovery and regular testing). Controllers must notify the Data Office of breaches that threaten privacy or security, and notify data subjects in certain cases. Processors must alert the controller. A DPIA is required before high-risk processing, including large-scale processing of sensitive data. A DPO is required for high-risk processing, systematic assessment of sensitive data (including profiling) or large-volume sensitive data processing.
- **Engineering implication:** Build an incident pipeline that can produce breach facts quickly (scope, records, data classes, timeline). Processor contracts need a "notify controller without delay" obligation backed by alerting. Run a DPIA template for AI and analytics features, and record the DPO contact in system documentation.
- **Source:** PDPL, Arts. 9, 10, 20 and 21. URL as FED-25.
- **Confidence:** Confirmed (primary). The breach-notification **deadline is unconfirmed** because it is left to the Executive Regulations.
- **As of 2026-10:** Executive Regulations not issued (ITSEC portal check, 23 Sep 2026: https://itsecnow.com/regulators/pdpl-executive-regulations-2026). Data Office not fully operational per Chambers 2026 and Kayrouz (Feb 2026).

### FED-27: Where the PDPL applies: data subject rights and objection to automated decisions
- **Requirement:** Rights to information (Art. 13), portability in machine-readable form (Art. 14), correction and erasure in listed cases (Art. 15), restriction (Art. 16) and objection (Art. 17). Individuals may object to automated decisions with legal or significant adverse effects, and controllers must allow human review on request (Art. 18). Cross-border transfers are allowed to adequate jurisdictions, or under contract, explicit consent and other listed grounds (Arts. 22 and 23).
- **Engineering implication:** Self-service or ticketed subject-request handling with export in a machine-readable format (JSON or FHIR). For AI triage, scoring or eligibility features, add a human-review path and keep decision explanations and inputs. Erasure must respect FED-07 for clinical data.
- **Source:** PDPL, Arts. 13 to 18, 22 and 23. URL as FED-25.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** No adequacy list or standard contractual clauses published (Kayrouz, Feb 2026). Penalties await a Cabinet decision under Art. 26 (not published).

## E. Medical Products Law (Federal Decree-Law No. 38 of 2024) and EDE

### FED-28: Software and AI that form part of medical equipment need EDE Marketing Approval before release
- **Requirement:** No Medical Product may be imported, sold, used or manufactured for circulation "except after obtaining Marketing Approval of any type from the EDE". "Medical Equipment" covers a product that contains a device, instrument or system, including "accessories, and operating software thereof", and expressly covers "wearable devices and products based on AI technology". Approvals are valid for five years. Conditional, emergency-use and fast-track routes exist (Arts. 8 to 14).
- **Engineering implication:** Assess intended use early. A diagnostic, triage, dosing or monitoring algorithm, or an AI feature in or driving a medical device, can make the product a medical product needing EDE approval before UAE deployment. Plan the regulatory path (classification, QMS evidence, clinical evaluation, labelling) into the release roadmap and lock the design baseline per approval.
- **Source:** Medical Products Law, Art. 1 (definitions), Arts. 2, 5(1) and 7. https://uaelegislation.gov.ae/en/legislations/2751 (English download read)
- **Confidence:** Confirmed (primary) for device software and AI-based products. Whether **standalone SaMD with no hardware** falls under "Medical Equipment" is **unconfirmed**: the text does not address it expressly.
- **As of 2026-10:** Active since 02 Jan 2025. Executive Regulations covering classification by product type (Art. 6(4)) were not located.

### FED-29: Get an EDE classification decision and meet class-based evidence requirements
- **Requirement:** EDE runs a product-classification service (AED 500, about 10 working days) whose letter states which UAE laws apply and whether marketing authorisation is needed. Medical-device marketing authorisation uses Classes I to IV. It requires reference approvals (EC, 510(k) or PMA by class), an EC Declaration of Conformity, post-marketing monitoring arrangements, and safety and efficacy data for Classes III and IV. Fees are AED 100 (application) and AED 5,000 (registration), with about 45 working days processing and 5-year validity.
- **Engineering implication:** Get a classification letter before go-live for any clinical-decision feature that could be a device. Keep the technical file and design history in a form that can support CE or FDA-referenced submissions. Expect a UAE marketing authorisation holder, through an EDE-licensed local entity, to be needed.
- **Source:** EDE service pages: https://www.ede.gov.ae/en/w/classification-of-a-product and https://www.ede.gov.ae/en/w/issuance-of-marketing-authorization-for-a-medical-device
- **Confidence:** Confirmed (primary) for the service pages. These pages do not mention software specifically, and EDE SaMD/AI-specific guidance is **unconfirmed (not located)**.
- **As of 2026-10:** RegDesk (Jan 2026) reports the transfer of device services from MOHAP to EDE is complete, with submissions through the EDE portal using UAE PASS (secondary).

### FED-30: Run post-market vigilance and performance monitoring for approved products
- **Requirement:** Applicants must implement a vigilance (pharmacovigilance) system and post-marketing follow-up. Product performance must be monitored and EDE vigilance guidance followed. Approvals can be cancelled for new safety concerns.
- **Engineering implication:** For regulated software: complaint and adverse-event intake in the product, field-performance and model-drift monitoring, versioned release notes, and recall or rollback capability. Retain records to support EDE vigilance reports.
- **Source:** Medical Products Law, Arts. 6(1)(b), 15(7), 17(1)(i), 21(5) and (6), and 68. URL as FED-28.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Reporting timelines for devices are **unconfirmed** (not in the text read).

### FED-31: Get EDE approval before advertising medical products, including online and on social media
- **Requirement:** Advertising Medical Products is prohibited unless approved by EDE. Approval is valid from 30 days to one year, and content must be truthful and disclose risks.
- **Engineering implication:** In-app promotional content, websites and app-store listings for a regulated medical software product need EDE advertising approval. Add a content-approval gate in the CMS.
- **Source:** Medical Products Law, Arts. 44 to 47. URL as FED-28. The Cybercrime Law, Art. 49, separately criminalises online promotion of unlicensed medical products (see FED-35).
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### FED-32: One-year transition period from 02 Jan 2025, and fines up to AED 1 million
- **Requirement:** Covered entities had one year from 02 Jan 2025 to comply, extendable by Cabinet decision. Fines reported up to AED 1,000,000 for establishments and AED 500,000 for practitioners, plus suspension, closure and licence cancellation.
- **Engineering implication:** Products already on the UAE market should have completed regularisation by early 2026 unless an extension applies. Check the product's EDE status before contract signature.
- **Source:** Baker McKenzie (https://healthcarelifesciences.bakermckenzie.com/2025/01/14/uae-enacts-new-pharma-law-to-boost-pharmaceutical-innovation/) and Gulf News (https://gulfnews.com/uae/new-law-to-regulate-medical-products-in-uae-up-to-dh1-million-fine-for-violators-1.500008369).
- **Confidence:** Confirmed (secondary). The transitional and penalty articles were beyond the portion of the official text that could be read. **Any extension is unconfirmed.**
- **As of 2026-10:** The transition period nominally ended 02 Jan 2026. No extension decision located.

## G. Cybersecurity, cybercrime and AI (federal)

### FED-33: UAE Information Assurance Regulation applies to designated critical entities, and maps onto the Riayati standard
- **Requirement:** TDRA's UAE IA Regulation applies to critical entities designated under the UAE CIIP Policy and to the information and systems they use, process, store or transmit. Secondary sources describe 188 controls across management and technical families, prioritised P1 (mandatory) to P4 (risk-based), also applying to government entities.
- **Engineering implication:** If the customer is a government health entity (for example EHS) or a designated critical entity, expect contractual flow-down of UAE IA controls to the vendor. Riayati controls are cross-referenced to UAE IA (MOH-03), so one control framework can serve both.
- **Source:** u.ae (https://u.ae/en/information-and-services/justice-safety-and-the-law/cyber-safety-and-digital-security/uae-information-assurance-regulation). Control counts and priorities per ImmuniWeb (https://www.immuniweb.com/compliance/uae-information-assurance-regulation-1-1-compliance/).
- **Confidence:** Applicability to designated critical entities confirmed (primary). Control counts and priorities confirmed (secondary). **Unconfirmed:** whether healthcare is formally designated a critical sector, and the federal incident-reporting timelines to TDRA/aeCERT.
- **As of 2026-10:** v1.1 is the version in circulation. No newer version located.

### FED-34: National Cloud Security Policy (Cyber Security Council, 2025)
- **Requirement:** Issued 17 Sep 2025 by the UAE Cyber Security Council. It applies to cloud consumers that are federal ministries and authorities, non-government Critical Information Infrastructure entities and (where no emirate CIIP programme exists) emirate government entities. Components include cloud governance, contractual agreements, data security and lifecycle, **data localisation and sovereignty**, interoperability and portability, IAM, incident management and forensics, and resilience.
- **Engineering implication:** For government or CII health customers, expect cloud contracts to require localisation, exit and portability, forensic cooperation and IAM controls aligned to this policy. Design for data portability (exportable formats) and forensic-ready logging.
- **Source:** https://uaelegislation.gov.ae/en/policy/details/national-cloud-security-policy
- **Confidence:** Confirmed (primary) for issuer, date, scope and components. Detailed obligations are **unconfirmed** (full text not read).
- **As of 2026-10:** Published policy.

### FED-35: Cybercrime Law: aggravated offences for medical data and for attacks on health entities
- **Requirement:** Unauthorised acquisition, possession, alteration, disclosure, copying or dissemination of personal electronic data: imprisonment of at least six months and/or a fine of AED 20,000 to 100,000. This is **aggravated where the data relates to examinations, diagnosis, medication, healthcare or medical records**. Harm to or disruption of a system of a health entity: temporary imprisonment and a fine of AED 500,000 to 3,000,000. Creating or managing a site to promote or sell unlicensed medical products is an offence (Art. 49).
- **Engineering implication:** The criminal exposure reinforces controls against insider snooping (access monitoring, FED-11), ransomware resilience (FED-18), and marketplace or e-pharmacy listing checks against EDE registration.
- **Source:** Federal Decree-Law No. 34 of 2021, Arts. 4(2), 6 and 49, per Legal Advice Middle East (https://legaladviceme.com/legislation/165/uae-federal-decree-law-34-2021-countering-rumors-cybercrimes) and Mondaq (https://webiis08.mondaq.com/privacy-protection/1618274/learn-more-about-privacy-violations-and-secret-disclosure-under-the-uae).
- **Confidence:** Confirmed (secondary). The exact aggravated penalty for medical data is **unconfirmed** (one source says penalties "may double", another states no separate amount).
- **As of 2026-10:** Amended by Federal Law No. 5 of 2024 (amendment content not reviewed).

### FED-36: UAE Charter for AI: non-statutory principles to reflect in AI features
- **Requirement:** The UAE Charter for the Development and Use of AI sets 12 principles, including safety, algorithmic bias, data privacy, transparency, human oversight, governance and accountability, and compliance with treaties and applicable laws. Its expected outcomes include enhanced healthcare.
- **Engineering implication:** For clinical AI, document intended use, bias testing across UAE population groups, human-in-the-loop design, transparency to clinicians and patients, and accountability (model cards, decision logs). The binding hooks are the PDPL Art. 18 human review (FED-27), EDE device rules (FED-28) and emirate AI policies (out of scope).
- **Source:** https://u.ae/en/about-the-uae/strategies-initiatives-and-awards/policies/Ai/The-UAE-Charter-for-the-Development-and-Use-of-Artificial-Intelligence
- **Confidence:** Principles confirmed (primary). **Unconfirmed:** legal status (the page does not say whether it is binding; treat it as policy guidance) and the launch date (secondary sources say June 2024).
- **As of 2026-10:** No general federal AI law. CMS (2025 review) considered one unlikely in 2026.

---

## Watch items (pending or changing, as of 2026-10)

1. **PDPL Executive Regulations:** still not issued (ITSEC check of the legislation portal, 23 Sep 2026). Once issued, there is a 6-month regularisation period (Art. 29), extendable. Breach-notification timelines, transfer controls and the Cabinet penalty decision (Art. 26) are all pending. One firm (Kayrouz) states "full compliance required by 1 January 2027". This is **unconfirmed**, and no Cabinet instrument setting it was found.
2. **UAE Data Office:** not fully operational. No adequacy list or standard clauses issued.
3. **Medical Products Law Executive Regulations:** product classification by type (Art. 6(4)), vigilance rules and violations/penalties regulations are pending or not located. The 1-year transition nominally ended 02 Jan 2026, and any Cabinet extension is unconfirmed. **EDE SaMD/AI-specific guidance not located.** Watch EDE for software and AI device guidance and for whether standalone software is treated as "Medical Equipment".
4. **ICT Health Law Art. 12 Ministerial decision** on storage conditions inside the UAE: not located.
5. **MR 51/2021 text:** sources disagree on the number of exceptions (8 or 10) and on conditions for telemedicine and health-authority-approved transfers. Obtain the gazette (Arabic) text before finalising the transfer gateway rules.
6. **Riayati:** confirm the ministerial resolution number (vendor blog says 156/2021), the Connected EMRs onboarding process, the "Riayati Confidentiality and Data Privacy Policy" and the "HL7 & CCDA Specifications" (listed on mohap.gov.ae but not reviewed), and Riayati security standard domains 5.5 to 5.10.
7. **Cyber:** whether healthcare is formally a designated critical sector under the UAE CIIP Policy; federal incident-reporting timelines; the full text of the National Cloud Security Policy (Sep 2025). Secondary commentary expects further federal cybersecurity rules through 2026.
8. **Cybercrime Law amendment (Federal Law No. 5 of 2024):** content not reviewed for health-data impact.
9. **Telehealth:** Cabinet Resolution No. 40 of 2019 remains Active. Check whether MOHAP has issued its own telehealth standard (DHA has ST-14 v4 effective 26 Nov 2025 per Kayrouz, but that is Dubai, out of scope).
10. **Emirate overlays:** DoH Abu Dhabi (ADHICS v2.0, Responsible AI Standard Oct 2025) and DHA impose stricter localisation and AI controls. Cross-check with the emirate-level research file.

## Primary sources used

- Federal Law No. 2 of 2019 (English): https://elaws.moj.gov.ae/UAE-MOJ_LC-En/00_Health/UAE-LC-En_2019-02-06_00002_Kait.html?val=EL1 ; metadata: https://uaelegislation.gov.ae/en/legislations/1209
- Cabinet Resolution No. 32 of 2020: https://uaelegislation.gov.ae/en/legislations/1444
- Federal Decree-Law No. 45 of 2021 (PDPL): https://uaelegislation.gov.ae/en/legislations/1972
- Federal Decree-Law No. 38 of 2024: https://uaelegislation.gov.ae/en/legislations/2751
- Cabinet Resolution No. 40 of 2019 (telehealth annex): https://uaelegislation.gov.ae/en/legislations/1191/regulations/753/download
- EDE services: https://www.ede.gov.ae/en/w/classification-of-a-product ; https://www.ede.gov.ae/en/w/issuance-of-marketing-authorization-for-a-medical-device
- Riayati: https://riayati.mohap.gov.ae/resources/Riayati_Integration_Terminology_and_Coding_Standards_v2.1.pdf ; https://riayati.mohap.gov.ae/ar/resources/Riayati_PGM_POL_Information-and-Cyber-Security-Standard_v1.0.pdf ; https://mohap.gov.ae/en/riayati/resources
- National Cloud Security Policy: https://uaelegislation.gov.ae/en/policy/details/national-cloud-security-policy
- UAE IA Regulation: https://u.ae/en/information-and-services/justice-safety-and-the-law/cyber-safety-and-digital-security/uae-information-assurance-regulation
- UAE AI Charter: https://u.ae/en/about-the-uae/strategies-initiatives-and-awards/policies/Ai/The-UAE-Charter-for-the-Development-and-Use-of-Artificial-Intelligence

Secondary: Al Tamimi, Hogan Lovells, CMS, Baker McKenzie, DLA Piper, Chambers 2026, Kayrouz & Associates, ITSEC, RegDesk, Legal Advice Middle East, Mondaq, Gulf News (URLs inline above).
