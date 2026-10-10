# Abu Dhabi: Department of Health (DoH)

> **Library status:** researched from primary and secondary sources on 2026-10-10. Each item shows its source and confidence. Laws and standards change: check the "As of" note and the regulator's current text before relying on an item, and treat "Unconfirmed" items as questions, not requirements. This library is engineering guidance, not legal advice.



Research date: 2026-10-10. Scope: emirate-level regulators only (Department of Health – Abu Dhabi, Dubai Health Authority). Federal law (Federal Law No. 2 of 2019 on ICT in Health, PDPL Federal Decree-Law No. 45 of 2021, Ministerial/Cabinet Decision No. 51 of 2021) is mentioned only where emirate documents depend on it.

How to read this file

- IDs are stable library IDs (DOH-xx, DHA-xx, CLM-xx) for the requirements library.
- Confidence:
  - **Confirmed (primary)**: read directly in the regulator's own published document (PDF text extracted and checked).
  - **Confirmed (secondary)**: from a law firm, vendor or news summary; not verified against the regulator text.
  - **Unconfirmed**: could not be verified; do not build to it without checking.
- Control numbers are given only where they appear in the source text. ADHICS "criteria" (Basic / Transitional / Advanced / Service Provider) are as printed beside each control in the v2 PDF.
- "As of 2026-10" notes say whether the item is current, superseded or has a deadline.

---

## A. Department of Health – Abu Dhabi (DoH)

### A1. ADHICS v2 (Abu Dhabi Healthcare Information and Cyber Security Standard)

Key facts (Confirmed, primary, ADHICS v2 cover page and section 1.1): ref. DOH/SD/ICSO/ADHICS/V2/2024; published May 2024; effective August 2024; revision due May 2027. It applies to any entity that generates, accesses, stores, uses, processes or transmits health information in Abu Dhabi, including healthcare facilities, payers and healthcare technology and service providers. It **supersedes** ADHICS v0.9 (2019), the IoMT Security Standard v0.9 (2020) and the Standard on Patient Healthcare Data Privacy v0.9 (2020). Owner: DoH Information & Cyber Security Office.

Structure (Confirmed, primary): Section A covers governance (ISGC, CISO, risk management, Statement of Applicability, asset classification, compliance and audit). Section B has 11 control domains: HR (Human Resources Security), AM (Asset Management), PE (Physical and Environmental Security), AC (Access Control), CO (Communications and Operations Management), DP (Data Privacy and Protection), CS (Cloud Security), TP (Third Party Security), SA (Information Systems Acquisition, Development and Maintenance), IM (Information Security Incident Management), SC (Information Systems Continuity Management). Each control cross-references the UAE IAR.

Source URL for all A1 items unless stated otherwise: https://www.doh.gov.ae/-/media/Feature/Resources/Standards/ADHICS-v2-standard.ashx

**DOH-01 — Entity tiering and compliance deadline**
- Requirement: Every in-scope entity must meet Basic controls. Hospitals with 1–20 beds and all centres (diagnostic, dialysis, IVF, rehabilitation) must also meet Transitional. Hospitals with 21+ beds, Malaffi, insurers and TPAs must meet Basic + Transitional + Advanced. External technology providers (medical device, EMR, web and mobile app providers) fall under a separate "Service Provider" criterion. Compliance is due within 6 months of programme induction/onboarding or release of the standard, whichever comes first. A control may be marked not applicable only with a business justification and evidence reported to DoH.
- Engineering implication: Decide early which tier the customer is in. Products sold into Abu Dhabi are themselves in scope as "Service Provider", so map every SP-tagged control to a product feature or a documented justification. Build a Statement of Applicability matrix into the delivery pack.
- Source: ADHICS v2, Section A "Control Adoption" table (Basic/Transitional/Advanced/Service Provider definitions). ADHICS FAQ v1.3 (relates to v1) gives the same facility-to-tier mapping: https://www.doh.gov.ae/-/media/Feature/Aamen/ADHICS-FAQ.ashx
- Confidence: Confirmed (primary)
- As of 2026-10: The 6-month window from the August 2024 effective date has passed. New entities and vendors get 6 months from onboarding.

**DOH-02 — Compliance reporting, audit and the AAMEN programme**
- Requirement: The entity's Information Security Governance Committee sends quarterly progress and compliance reports to DoH (Governance 2.1.1(c)). The entity runs an annual audit programme, with independent audits at least annually or after significant change (section 6.2). It keeps a register of applicable laws and circulars with compliance checklists and reporting obligations (6.1). DoH runs enforcement through the AAMEN programme, which has a portal at aamen.doh.gov.ae. According to the FAQ, ADHICS compliance is added to the existing audit and licence registration/renewal process.
- Engineering implication: Produce audit evidence by default: exportable access reviews, log samples, VAPT reports and policy mappings. Expect customer audits and DoH questionnaires during licence renewal.
- Source: ADHICS v2 sections 2.1.1, 6.1, 6.2. AAMEN page: https://www.doh.gov.ae/en/programs-initiatives/Aamen. ADHICS FAQ v1.3 (licence linkage)
- Confidence: Confirmed (primary) for the reporting and audit duties. The licence-renewal linkage is stated in the v1 FAQ; whether the same wording applies under v2 is unconfirmed. How the AAMEN portal works (self-assessment workflow, evidence upload) is unconfirmed.
- As of 2026-10: Current.

**DOH-03 — Unique identities, account lifecycle and session controls**
- Requirement: Each user has a unique account. Shared accounts need explicit owner approval. Accounts are deactivated after a defined period of inactivity, revoked on exit, and revalidated on role change (AC 2.1, Basic/SP). Access is revoked within 24 hours of exit, and the entity informs DoH and the served entity to revoke relevant access (HR 4.3, Basic/SP). Workstations and systems lock automatically after inactivity, idle sessions terminate automatically, and a logon banner must be acknowledged (AC 6.1, Basic/SP). All user activity is logged against the unique ID (AC 6.2, Basic/SP).
- Engineering implication: Provide SSO or IdP integration, SCIM or equivalent deprovisioning, configurable idle timeouts, a banner acknowledgement screen and per-user audit attribution. Avoid generic clinic logins.
- Source: ADHICS v2 AC 2.1, AC 6.1, AC 6.2, HR 4.3
- Confidence: Confirmed (primary)
- As of 2026-10: Current. The inactivity durations are entity-defined; ADHICS sets no number.

**DOH-04 — Privileged access and MFA**
- Requirement: Normal accounts must not be used as service accounts or for privileged work. Privileged IDs are not shared. Service accounts are not hardcoded in code or scripts. MFA is enforced for all privileged, administrative and remote access, and remote access is monitored (AC 2.2, Transitional/SP).
- Engineering implication: Move secrets to a vault, separate admin roles, put MFA on admin consoles and support/VPN paths, and log remote vendor sessions.
- Source: ADHICS v2 AC 2.2
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-05 — Password and credential rules**
- Requirement: Default credentials are changed before deployment. Passwords are masked on entry and stored hashed and encrypted. Credentials are sent over two different channels. Passwords have at least 12 characters with a number, upper case, lower case and a special character. Passwords (including service and privileged accounts) are changed periodically. Accounts lock "after at least 5 failed attempts". History prevents reuse of at least the last 3 passwords. Passwords are changed after any remote maintenance that required sharing them (AC 2.3, Basic/SP).
- Engineering implication: Enforce a 12-character complexity policy, lockout, history and salted hashing (e.g. bcrypt/Argon2) in any local auth. Deliver initial credentials out of band, for example a link by email and a code by SMS.
- Source: ADHICS v2 AC 2.3
- Confidence: Confirmed (primary). The lockout wording "after at least 5 failed attempts" is ambiguous; a threshold of 5 or fewer is the safe reading.
- As of 2026-10: Current.

**DOH-06 — Logging and monitoring**
- Requirement: All critical technology (servers, databases, network devices, applications, medical devices) must be able to generate logs. Logs cover privileged activity, logon attempts, changes, information exchange, integration and backup events. The entity sets review frequencies and alerting/escalation criteria, and protects logs from tampering, modification or destruction (CO 6.1, Advanced/SP). Advanced entities keep logs in a centralised log management system run by people with no operational role, correlate events, define dashboards, and retain logs "for a period commensurate with legal, regulatory and entity demands" (CO 6.2, Advanced). All clocks, including medical devices, are synchronised to an agreed time source (CO 6.3, Basic). Data Leakage Prevention is required (CO 6.4, Transitional/SP).
- Engineering implication: Emit structured, tamper-evident audit logs (append-only or WORM storage, hash chaining), support SIEM forwarding (syslog/CEF/JSON), use NTP and UTC timestamps, and make retention configurable. ADHICS sets no fixed log retention period, so the customer's policy decides.
- Source: ADHICS v2 CO 6.1–6.4
- Confidence: Confirmed (primary)
- As of 2026-10: Current. The fixed retention period is **not specified** in ADHICS v2.

**DOH-07 — Health data localisation (no storage, processing or access outside the UAE)**
- Requirement: Health information (PII/PHI) or any copy of it, "whether encrypted, anonymized, deidentified, pseudonymized", must not be stored, processed or transferred outside the UAE without a DoH exemption (DP 1.4(4); CO 9.2(6), Basic/SP). Only people physically present in the UAE, or holding a DoH licence, may access systems containing PII/PHI; exemptions need entity management approval and then DoH approval (DP 1.4(3)). Employees and third parties must deliver services from within the UAE unless DoH grants an exemption (CO 9.2(7)). Identified or de-identified data must not be shared with third parties or processors without DoH authorisation (CO 9.2(8)).
- Engineering implication: Host in UAE regions only, with no global replication. Block offshore support access with geo-fencing and conditional access. Do not send telemetry or crash dumps containing PHI to offshore SaaS. This rules out offshore LLM APIs for PHI unless hosted in the UAE or exempted. Keep DevOps and support staff in the UAE, or obtain an exemption.
- Source: ADHICS v2 DP 1.4, CO 9.2
- Confidence: Confirmed (primary)
- As of 2026-10: Current. It is stricter than the federal Ministerial Decision 51/2021 exceptions because it expressly covers anonymised data.

**DOH-08 — Cloud security and customer-held keys**
- Requirement: The cloud environment, including backup and DR, must be physically hosted in the UAE. Cloud data must not be accessible to other parties in a multi-tenant environment, to analytics providers that move data offshore, or to remote support from outside the UAE. Data must be encrypted at rest and in transit. Encryption keys must not be provided, stored or controlled by the CSP hosting the service. The entity must be able to generate or configure its own keys, commission independent external testing, and plan for portability (CS 1.2, Transitional/SP). A cloud security policy covers SLAs/NDAs and data retention, portability and destruction (CS 1.1, Basic).
- Engineering implication: Design for customer-managed keys from an external KMS/HSM (HYOK/BYOK where the CSP cannot access keys), single-tenant data isolation (or strong logical isolation, documented), a UAE-only DR region and data export tooling. Note that "key not provided by the CSP" rules out default CSP-managed keys.
- Source: ADHICS v2 CS 1.1, CS 1.2
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-09 — Cryptography and key management**
- Requirement: Encrypt information stored and transmitted within and outside the entity. Operate a key lifecycle: generation, distribution, protection, activation/deactivation dates, revocation, backup, recovery, replacement and monitoring. Define key strength and storage standards (SA 3.1, Transitional/SP). Encrypt removable media (AC 3.1, CO 9.4) and data on medical devices (AM 4.4, Transitional).
- Engineering implication: Document crypto standards (e.g. TLS 1.2+ and AES-256; ADHICS names no algorithms), implement key rotation and expiry, and log key operations.
- Source: ADHICS v2 SA 3.1, AM 4.4, AC 3.1, CO 9.4
- Confidence: Confirmed (primary). No algorithms or key lengths are specified in ADHICS v2.
- As of 2026-10: Current.

**DOH-10 — Secure information exchange**
- Requirement: Do not send health information by mail/email unless it goes to the data subject. Keep chain of custody in transit. Connect to AD Healthcare Net and integrate the EMR securely with Malaffi. Encrypt critical information before transfer and share the key over a different channel. Use appropriate interoperability standards. Set security requirements for exchange with third parties (CO 9.2, Basic/SP). Exchange agreements must cover classification, incident notification and right to audit (CO 9.3). Official email must use UAE email domains, not public mailboxes (CO 9.5, Basic/SP).
- Engineering implication: Replace PHI email/export flows with authenticated portals or encrypted packages, deliver passwords separately, and host notification email on a UAE domain. Integrations must use standard formats (HL7/FHIR).
- Source: ADHICS v2 CO 9.2, CO 9.3, CO 9.5
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-11 — Secure development lifecycle**
- Requirement: Apply security engineering principles to new systems and enhancements. Security requirements must be approved, risk-based, compliant with ADHICS and secure coding practices, and include validation and acceptance criteria. Keep HLD/LLD architecture documentation. Ensure "no activity in development lifecycle is carried out outside the boundaries of UAE" (SA 2.1, Transitional/SP). Validate input, internal processing and output, and ensure message authenticity and integrity between systems (SA 2.3, Transitional/SP). Distributed and mobile apps must handle offline operation and duplicate or out-of-sequence messages (SA 2.4, Transitional/SP). Advanced entities train developers before they start (SA 2.2).
- Engineering implication: Run threat modelling, SAST/DAST and secure code review, and keep current architecture diagrams. Make interfaces idempotent with message IDs and sequence handling, and give mobile apps offline queues. **Offshore development teams working on Abu Dhabi health systems conflict with SA 2.1(8)** unless DoH grants an exemption.
- Source: ADHICS v2 SA 2.1–2.4
- Confidence: Confirmed (primary)
- As of 2026-10: Current. High impact for offshore delivery models.

**DOH-12 — Test data and source code protection**
- Requirement: Test with sample data sets and restrict real production data. Anonymise health information before using it for testing or training. Do not use PII as test data. Erase test data after testing and keep records of copying and erasure. Restrict source code access to need (SA 4.2, Transitional/SP). Control software installation (SA 4.1).
- Engineering implication: Use synthetic data generators and masked staging environments, record dataset provenance, and enforce repository RBAC.
- Source: ADHICS v2 SA 4.1, SA 4.2
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-13 — Outsourced development and supply chain**
- Requirement: The healthcare entity must hold sole custody of source code and source code backups for outsourced development. Contracts require compliance with entity policy, ADHICS, law and secure coding practices, plus right to audit. The entity conducts source code reviews for vulnerabilities, back doors and malicious code (SA 5.1, Transitional/SP). Highly critical third-party products must conform before procurement (SA 6.1, Basic/SP). Advanced entities evaluate suppliers, verify that delivered software is unaltered and limit architecture sharing (SA 6.2–6.4).
- Engineering implication: Contracts with Abu Dhabi customers may require source code escrow or delivery, signed builds/SBOMs and the client's right to review code. Price and plan for this.
- Source: ADHICS v2 SA 5.1, SA 6.1–6.4
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-14 — Vulnerability assessment and penetration testing**
- Requirement: Run yearly VA/PT of systems, network and infrastructure, internet-facing web and mobile apps, and connected medical devices. Security-test and authorise every new deployment or change before production. Cooperate with DoH vulnerability assessments. Track remediation timelines and revalidate (CO 7.1, Advanced/SP). Assessment data must be erased from the assessor's environment afterwards (CO 7.2, Advanced/SP). Patch promptly and restrict obsolete technology (CO 8.1).
- Engineering implication: Schedule an annual third-party pentest for each release line, add security gates to CI/CD and keep remediation SLAs.
- Source: ADHICS v2 CO 7.1, CO 7.2, CO 8.1
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-15 — Third-party contracts and exit**
- Requirement: Third-party agreements must define the data accessed and the security requirements. The third party must not use entity data for its own purposes. Agreements include data centre security, right to audit, prompt notification to DoH of incidents involving the third party, and incident support. Exit clauses must cover: conversion of data to a standard format, removal of data from the third party's environment, migration, handover of all backups, disconnection of integrations, cooperation with the new provider and knowledge handover (TP 2.1, Basic/SP). Processors must notify the entity of sub-contractors, incidents and processing beyond the agreed period (DP 1.6, Basic/SP).
- Engineering implication: Build bulk export in standard formats (FHIR/HL7/CSV), verifiable deletion with certificates, a sub-processor register and incident notification hooks to customers.
- Source: ADHICS v2 TP 2.1, DP 1.6
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-16 — Consent capture and withdrawal**
- Requirement: Do not process PII/PHI without consent except in listed cases, including medical diagnosis and treatment, insurance, public health, regulator requests, information exchange with Malaffi, and legal obligations. Collect and store informed consent (written or electronic) that is clear and accessible, and support withdrawal or modification (DP 1.2, Basic/SP).
- Engineering implication: Build a consent record model (purpose, lawful-basis exception, timestamp, representative, withdrawal) and enforce it before non-care processing such as marketing, analytics or research.
- Source: ADHICS v2 DP 1.2
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-17 — Lawful processing, minimisation and data subject rights**
- Requirement: Process data on a lawful basis, collect the minimum necessary, keep it accurate and delete it when the purpose ends or under the retention policy (DP 1.3, Transitional/SP). Fulfil data subject requests for information, transfer (including machine-readable export), correction, deletion, restriction and objection to automated processing and profiling. Keep records of disclosures. Rejection grounds are listed (DP 3.1, Transitional/SP). Limit printing to controlled printers (DP 1.4(2)). Limit access to need, for example an encounter (DP 1.4(5)–(6)).
- Engineering implication: Provide a DSAR workflow, machine-readable patient export (FHIR bundle), a disclosure log, retention and deletion jobs, and encounter-based access checks.
- Source: ADHICS v2 DP 1.3, DP 1.4, DP 3.1
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-18 — DPIA, processing inventory and DPO**
- Requirement: Advanced entities keep a Data Processing Inventory (data categories, subjects, access, purpose, transfers, cross-border movement, security measures) and run a DPIA before acquiring or implementing technology that processes PII/PHI, or before high-risk processing (DP 1.5, Advanced). They appoint a DPO where processing is large-scale, automated/high-risk or involves profiling (DP 2.1, Advanced).
- Engineering implication: Vendors should supply DPIA input packs (data flow diagrams, data inventory, hosting locations, sub-processors) with each product.
- Source: ADHICS v2 DP 1.5, DP 2.1
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-19 — Breach and incident notification to DoH**
- Requirement: Report information security and privacy incidents to the Abu Dhabi Health SOC within "predetermined timeframes" set by the ADHICS implementation guideline's IM 2 Incident Reporting Matrix (IM 2.1, Basic/SP). Inform DoH of breaches at the entity or its processor within the guideline timelines. Submit the DoH "Data Breach Form" **within 72 hours of acknowledging the incident**. Give DoH full evidence **within 30 working days** of the initial report. Notify affected data subjects where a breach is likely to result in high risk (DP 1.7, Basic/SP). Advanced entities run a CSIRT (IM 2.2); Transitional entities classify incidents using the DoH matrix and test response capability (IM 2.3, 2.4).
- Engineering implication: Provide incident detection and alerting, forensic-ready logs and a breach-impact query (which patients and records were affected) so customers can meet the 72-hour form. Vendor contracts should commit to notification well inside 72 hours.
- Source: ADHICS v2 IM 2.1–2.5, DP 1.7. The incident reporting matrix sits in the separate "Guidelines for the Implementation of ADHICS" (not retrieved).
- Confidence: Confirmed (primary) for the 72-hour Data Breach Form and the 30 working days. **Unconfirmed**: the severity-based SOC reporting timelines in the IM 2 matrix.
- As of 2026-10: Current. The legacy 2020 Data Privacy Standard's "24 hours" breach rule (secondary source) is superseded.

**DOH-20 — Backup, archival and continuity**
- Requirement: The backup process must identify critical systems, data owners and RPO/RTO. Back up data and configuration. Test restoration. Keep backup instances separate and not on the live environment (CO 5.1, Basic/SP). Archive and destroy data according to retention requirements (CO 5.2). Advanced entities and SPs run a BIA with RTO/RPO, keep continuity and recovery plans with redundancy, and test failover (SC 2.1–2.3).
- Engineering implication: Publish RPO/RTO per tier, use immutable and offline backups in a separate UAE location (see DOH-08), run restore drills with evidence, and design high availability.
- Source: ADHICS v2 CO 5.1, CO 5.2, SC 1.1, SC 2.1–2.3
- Confidence: Confirmed (primary)
- As of 2026-10: Current. No fixed RTO/RPO values are set by DoH.

**DOH-21 — Medical devices and IoMT**
- Requirement: Keep a medical device inventory linked to patients (with sensitive data redacted), with defined roles, authentication where possible, a risk log and secure decommissioning (AM 1.2, Basic). Encrypt data on devices and secure device communications (AM 4.4, Transitional). Prefer wired connections (AM 4.5, Transitional). Synchronise device clocks (CO 6.3). Include connected devices in annual VA/PT (CO 7.1). Procure devices with built-in security features (SA 6.2).
- Engineering implication: Device-integration middleware should authenticate devices, encrypt traffic, record device IDs against observations and support time sync.
- Source: ADHICS v2 AM 1.2, AM 4.3–4.5, CO 6.3, CO 7.1, SA 6.2
- Confidence: Confirmed (primary)
- As of 2026-10: The standalone DoH IoMT Security Standard (2020) is retired and folded into ADHICS v2 (AAMEN page; ADHICS 1.1).

**DOH-22 — Status of the 2020 Patient Healthcare Data Privacy Standard**
- Requirement: DoH Circular 116 (15 Oct 2020) introduced the "DoH Standard on Patient Healthcare Data Privacy", under Federal Law No. 2 of 2019, with quarterly compliance reporting and reporting of breaches to DoH. ADHICS v2 now supersedes it (v0.9 of 2020), and the AAMEN page marks it "retired" and "Addressed in ADHICS V2".
- Engineering implication: Build to the ADHICS v2 DP domain (DOH-07, DOH-16 to DOH-19), not to the 2020 standard. Older tenders may still cite the 2020 name.
- Source: Circular 116/2020 https://www.doh.gov.ae/-/media/Feature/Resources/Circulars/116--20201.ashx; ADHICS v2 section 1.1; AAMEN page; Al Tamimi summary https://www.tamimi.com/news/abu-dhabi-uae-issues-standard-on-healthcare-data-privacy/
- Confidence: Confirmed (primary)
- As of 2026-10: Superseded. The text of the 2020 standard itself was not retrieved.

### A2. Malaffi (Abu Dhabi Health Information Exchange) and Sahatna

**DOH-23 — Mandatory Malaffi connection and onboarding gate**
- Requirement: All facilities with an active DoH licence in the listed categories must connect to Malaffi. Listed categories include hospitals, centres, clinics, diagnostic centres, dialysis, IVF, all tele-medicine providers, pharmacies, mobile units and home care; military, police and similar facilities are exempt. Facilities that are not connected cannot renew their DoH licence. Onboarding requires an Accession Agreement and Facility Declaration, the Malaffi Security Assessment (an excerpt of the ADHICS survey), an "HL7 capable EMR" and an Etisalat SDWAN connection. The HIE Policy makes patient data sharing via the ADHIE mandatory and requires participants to build their systems to the ADHIE Platform Interface Standards.
- Engineering implication: An EMR sold in Abu Dhabi must have certified Malaffi HL7 interfaces and pass the ADHICS-derived security assessment. Telehealth platforms are in scope.
- Source: Malaffi "Facilities Required to Connect" https://www.malaffi.ae/providers/facilities-required-to-connect/; DoH Policy on the Abu Dhabi HIE, DOH/POL/STR/HIE/1.1/2020 (effective 16 Apr 2020), sections 4.1–4.2 and Appendix 6.1.2 https://doh.gov.ae/-/media/A78104416DF2440E89AC90FB75F11055.ashx; ADHICS FAQ v1.3 (minimum ADHICS controls are a prerequisite for onboarding)
- Confidence: Confirmed (primary)
- As of 2026-10: Current. Malaffi states the general connection deadline "has now closed".

**DOH-24 — Malaffi minimum required dataset (Circular 2026/10)**
- Requirement: All facilities must transmit the minimum datasets to Malaffi. The circular is dated 19 Jan 2026; facilities had to submit an implementation plan within 1 month and be onboarded within 6 months. Datasets and HL7 message types listed: ADT (admit/discharge/transfer, ER arrival, birth notification, ED triage, inpatient admission notes, new visit identifiers), PPR (chronic problems), ORU (lab, radiology, microbiology, clinical documents, vitals, cancer staging), OMP (medication orders), RDS (dispense), RAS (administration), VXU (immunisations), SIU (future appointments, appointment mode), and API integration for Sahatna (in-person and tele-consultation) and e-prescription. Non-compliance may lead to denial or restriction of licence renewal.
- Engineering implication: An HL7 v2 interface engine must produce these message types, including the newer PHC Phase 1 fields (waist/hip circumference, social and family history) and appointment-mode flags.
- Source: DoH Circular No. 2026/10 "Mandatory On-boarding – Minimum Required Dataset with Malaffi" https://www.doh.gov.ae/-/media/84A213F71AA545D69E01A23A0A99C598.ashx
- Confidence: Confirmed (primary)
- As of 2026-10: The 6-month onboarding deadline (about 19 Jul 2026) has passed. DoH is monitoring compliance.

**DOH-25 — Referral data to Malaffi (Circular 2026/144)**
- Requirement: All facilities must submit all referral transactions to Malaffi using the HL7 **REF** message type, including referral priority and SNOMED-coded specialty, to support the Abu Dhabi Primary Care Model and Malaffi eReferral. Full compliance was due by **31 Aug 2026**.
- Engineering implication: Implement HL7 v2 REF referral messaging to the Malaffi specification (trigger events and segment profile to be obtained from Malaffi), with SNOMED CT specialty code sets and priority values from Malaffi's code sets.
- Source: DoH Circular No. 2026/144 (28 Jul 2026) https://www.doh.gov.ae/-/media/C01533BB56EE4A7D9B75107C89656941.ashx
- Confidence: Confirmed (primary). The trigger event codes are unconfirmed.
- As of 2026-10: The deadline has passed. DoH monitors through Malaffi utilisation reports.

**DOH-26 — Sahatna patient platform integration (Circular 2026/170)**
- Requirement: All facilities must integrate with "Sahatna", the official patient-facing platform, by **30 Nov 2026**. This covers technical integration, in-person appointment booking, teleconsultation booking and delivery (where offered), Primary Care Digital Registration (where applicable), a nominated technical and business point of contact, and continued availability after go-live. The circular builds on Circular 2023/153.
- Engineering implication: Scheduling and telehealth modules need Sahatna booking APIs (onboarding via sahatnaonboarding@malaffi.ae). Plan integration and testing before the deadline.
- Source: DoH Circular No. 2026/170 (1 Sep 2026) https://www.doh.gov.ae/-/media/73037D74C0A441AA8C69CF8E233FFE3E.ashx
- Confidence: Confirmed (primary). The API specification itself is unconfirmed (not public).
- As of 2026-10: **Open deadline 30 Nov 2026.**

**DOH-27 — HIE consent model**
- Requirement: Facilities must add Malaffi sharing consent to standard patient consent forms, covering both new and past data and all participating facilities. Where consent is unavailable (e.g. legacy data), DoH mandates and authorises sharing anyway. Patients must be told that sharing with the ADHIE is part of their treatment. VIP patient data must not be supplied to the ADHIE except as DoH directs. Emergency "break the seal" access is permitted under defined conditions.
- Engineering implication: Registration workflows must capture the HIE consent clause and support VIP suppression flags that block outbound HL7 for flagged patients.
- Source: DoH Policy on the Abu Dhabi HIE (2020), sections 4.2.1–4.2.2, 6.1.2.6, 6.1.4
- Confidence: Confirmed (primary). **Unconfirmed**: whether a patient-level opt-out from Malaffi exists. No opt-out provision was found in the 2020 policy or on the Malaffi patient pages reviewed.
- As of 2026-10: The policy is dated 2020. A newer HIE policy version was not found.

**DOH-28 — HIE user, logging and breach rules**
- Requirement: Each authorised user is uniquely identified by Emirates ID verification. User additions and removals are notified to the ADHIE within 48 hours. The ADHIE removes users within 24 hours of notice and disables accounts inactive for 60+ days. Facilities keep a Master Patient Index. Clinical transactions are logged in immutable logs that do not contain the full record. Facilities notify DoH of breaches within 5 business days and patients within 60 days of discovery. Data quality needs real-time validation and correction feedback.
- Engineering implication: Provide a user-sync feed to Malaffi, MPI/EMPI matching, and audit logs that store references rather than payloads.
- Source: DoH Policy on the Abu Dhabi HIE (2020), sections 4.3.17, 4.4.3, 6.1.2.9–6.1.2.17, 6.1.3.7–6.1.3.13
- Confidence: Confirmed (primary)
- As of 2026-10: The password rules in this policy (8 characters, 60-day expiry) are older and weaker than ADHICS v2 (12 characters); apply ADHICS. The breach timelines may be overtaken by the ADHICS DP 1.7 72-hour form (assumption: apply the stricter rule).

**DOH-29 — Interoperability standards for Malaffi**
- Requirement: The HIE Policy names HL7 (v2.x or 3.0), NCPDP SCRIPT, SNOMED and XML 1.0 as standards, with web services and private WAN or SSL transport. Malaffi requires an "HL7 capable EMR", and the 2026 circulars specify HL7 v2 message types.
- Engineering implication: Build HL7 v2 messaging as the primary integration. Do not assume a FHIR route into Malaffi.
- Source: DoH Policy on the Abu Dhabi HIE (2020), sections 4.4.2, 6.1.3.18; Malaffi facilities page; Circular 2026/10
- Confidence: Confirmed (primary) for HL7 v2 messaging. **Unconfirmed**: the exact HL7 v2 sub-version (e.g. 2.5.1) and whether Malaffi offers a FHIR API for providers. The Malaffi interface specification is not public.
- As of 2026-10: Current.

### A3. AI in Abu Dhabi healthcare

**DOH-30 — DoH Responsible AI Standard: scope and risk tiers**
- Requirement: The Responsible AI Standard (ref. DoH/ST/DDGO/RAI/V1/2025, published and effective Oct 2025, owner Data and Digital Governance Office) applies to DoH and all licensed entities developing, procuring or deploying AI. That includes clinical, financial and administrative AI tools, devices, datasets, agents and platforms, whether built in-house, bought from vendors or created in research, across the full lifecycle. Every AI system needs a risk assessment, a risk register and risk owners. After residual risk is assessed: **Very High** risk systems are prohibited; **High** risk systems need conformity assessments before and after deployment, coordinated with DoH DDGO; **Medium** risk systems follow governance and risk reporting; **Low/Very Low** risk systems are exempt from formal reporting but still monitored (3.3.1). Entities report compliance periodically, and DoH audits (section 5). Sanctions follow the Disciplinary Regulations (section 6). Details are in a separate "Responsible AI-Risk Management Protocol".
- Engineering implication: Every AI feature (including LLM agents and admin automation) needs a risk classification record. High-risk clinical AI needs a pre- and post-deployment conformity file for DoH.
- Source: DoH Responsible AI Standard V1 (2025), sections 1.2–1.4, 3.3, 5, 6 https://www.doh.gov.ae/-/media/B84FF5DD027846699C51F36C439A580F.ashx
- Confidence: Confirmed (primary). The Risk Management Protocol text was not retrieved (unconfirmed).
- As of 2026-10: Current. **Revision date is Oct 2026**, so a revised version may be imminent.

**DOH-31 — Responsible AI: human oversight, safety and monitoring**
- Requirement: AI must include human-in-the-loop or human-on-the-loop mechanisms with escalation protocols (3.1.1.2). It must have graceful degradation and fallbacks, pre-deployment safety testing and failure mode analysis, and continuous post-deployment monitoring for drift and degradation (3.1.2.1). Models are validated against benchmarks on diverse data, and significant performance declines trigger review (3.1.3.1). Override mechanisms and audit trails are embedded, and clinicians retain final decision authority (3.1.7.2). Datasets must be representative, with bias audits and mitigation (3.1.5.1).
- Engineering implication: Build override/accept/reject UI with logging, confidence displays, drift and performance monitoring dashboards, kill switches and fallback to manual workflows, and subgroup performance reporting.
- Source: DoH Responsible AI Standard V1, sections 3.1.1–3.1.3, 3.1.5, 3.1.7
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-32 — Responsible AI: security, privacy, sovereignty and traceability**
- Requirement: Enforce RBAC, MFA and least privilege across AI data, models, APIs and admin interfaces, with all activity logged (3.1.4.2). Run secure code reviews and automated vulnerability scanning to OWASP/NIST before deployment and at major updates (3.1.4.3). Use privacy-by-design and minimisation, with encryption, anonymisation/pseudonymisation and key management across the lifecycle. **"Production data shall not be used to train the model"** (3.1.4.4.2). Compute and storage for active and DR sites must be **in the UAE**, and remote access for development and support teams should be from within the UAE only (3.1.4.5.1). Use segmented networks, IDS and centralised logging (3.1.4.5). Keep end-to-end traceability logs of inputs, outputs and version history, and document data provenance and model architecture (3.1.6.2). Inform patients of their right to opt out of AI-driven services where applicable, with clear consent (3.1.6.1.3). Provide role-specific explanations, with plain language for patients (3.1.6.3). Datasets are uniquely versioned in immutable storage with datasheets and metadata (3.2.4–3.2.5). Patient-derived data may be used only as consent and DoH rules permit (3.2.1.7).
- Engineering implication: Use UAE-hosted inference (no offshore LLM endpoints for PHI), log prompts, outputs and model versions, keep model and data cards, and provide an AI opt-out flag per patient. Training pipelines must use governed, non-production datasets. Note how strict "production data shall not be used to train".
- Source: DoH Responsible AI Standard V1, sections 3.1.4, 3.1.6, 3.2
- Confidence: Confirmed (primary)
- As of 2026-10: Current.

**DOH-33 — DoH Policy on Use of AI in Healthcare (2018)**
- Requirement: AI tools must be robust, shown by certification of the software and major versions from recognised international agencies, compliance with ADSSSA regulations and auditable validation statements. They must have graceful degradation (automatic alerts and gradual cessation on malfunction), improve continuously from user feedback (sent to DoH Drug & Medical Products Regulation and the manufacturer), be auditable, and comply with the HIE Policy. End-users need AI governance, a defined point of entry of AI and the clinician's role, regular audits reported to DoH, and reporting of AI incidents and patient-safety issues to DoH.
- Engineering implication: Keep a validation dossier and certifications (e.g. CE/FDA where the product is SaMD), build degradation alerts and capture user feedback.
- Source: DoH "Policy on Use of Artificial Intelligence (AI) in the Healthcare Sector of the Emirate of Abu Dhabi", Policy/AI/0.9, 30 Apr 2018, sections 7–9 https://www.doh.gov.ae/-/media/E9C1470A575146B18015DEBE57E47F8D.ashx
- Confidence: Confirmed (primary)
- As of 2026-10: Still published. The RAI Standard (2025) references a newer "AI policy standard" dated June 2025 and an "AI Risk Management V1.0" dated July 2025 on the DoH policies page. Their content was not retrieved (unconfirmed). Assume the 2025 instruments take precedence where they conflict.

### A4. Telemedicine (Abu Dhabi)

**DOH-34 — DoH Standard on Tele-Medicine: identity, consent and records**
- Requirement: Tele-consultation providers must securely identify, authenticate and authorise patients and guardians, and register and verify identity before service (11.5.3–11.5.4). Verbal consent must include re-confirmation of the patient's identity and be recorded in the EMR, and e-consent is permitted (6.4.1, 11.5.1.1). Recording of the whole video, web or phone consultation is at the physician's judgement (11.5.1.2). Encounters are maintained in the EMR (11.9.4).
- Engineering implication: Telehealth apps need identity verification at registration (Emirates ID or UAE PASS is a sensible choice; the standard names no method), consent capture tied to the encounter and optional session recording with secure storage.
- Source: DoH Standard on Tele-Medicine, DOH/SA/TLM/0.9/2020, effective 16 Sep 2020 https://www.doh.gov.ae/-/media/Feature/Resources/Standards/Telemedicine-Standard-Final-with-Appendices.ashx
- Confidence: Confirmed (primary). The specific identity method is unconfirmed (not specified).
- As of 2026-10: The 2020 standard is the latest found. Telehealth Jawda KPI guidance exists for 2025 and 2026 (not reviewed). Whether a newer remote-care standard replaces it is unconfirmed.

**DOH-35 — Tele-medicine ICT security, residency and tele-prescribing**
- Requirement: ICT must meet Abu Dhabi Digital Authority (ADDA) and DoH technical and security requirements, including ADHICS. It needs network security and protection of recorded images, sufficient availability, and procedures for technical failure (5.7). Sharing data outside the UAE needs DoH authorisation (9.2.3). Tele-prescribing requires live real-time audio/video before prescribing, using an ICT system located on the facility's premises. The prescription is captured in the EMR with an electronic signature and transmitted securely through the HIS to a DoH-licensed Abu Dhabi pharmacy (12.3–12.6). Tele-monitoring devices must meet procurement criteria including encryption in transit and storage, authentication and patchability (13.8.11).
- Engineering implication: Build e-prescribing with e-signature, tie it to an active AV session and integrate with pharmacy. Remote monitoring devices must support encryption and secure updates.
- Source: DoH Standard on Tele-Medicine (2020), sections 5.7, 9.2.3, 12.3–12.6, 13.8
- Confidence: Confirmed (primary)
- As of 2026-10: See DOH-26 (Sahatna teleconsultation integration by 30 Nov 2026) and DOH-23 (all tele-medicine providers must connect to Malaffi).

---


Watch items, unconfirmed points and primary sources for both emirates are listed at the end of dha-dubai.md.
