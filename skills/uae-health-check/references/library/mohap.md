# MOHAP and the Northern Emirates (Riayati, telehealth)

> **Library status:** researched from primary and secondary sources on 2026-10-10. Each item shows its source and confidence. Laws and standards change: check the "As of" note and the regulator's current text before relying on an item, and treat "Unconfirmed" items as questions, not requirements. This library is engineering guidance, not legal advice.

Sources for these items are listed at the end of federal.md.

## F. MOHAP: Riayati and telehealth

### MOH-01: MOHAP-licensed facilities must connect to Riayati, the national unified medical record
- **Requirement:** Riayati is the national unified medical record platform (MOHAP with EHS, DHA's NABIDH and DoH's Malaffi). A ministerial decision of 16 Nov 2021 requires health facilities licensed by MOHAP to join the unified record within one year of issuance.
- **Engineering implication:** Any EMR or practice-management system sold to MOHAP-licensed facilities (public or private, in the Northern Emirates) must integrate with the Riayati HIE. Facilities connect through a qualifying EMR, so vendor certification or listing is a sales prerequisite.
- **Source:** MOHAP news release (https://mohap.gov.ae/en/w/ministry-of-health-and-prevention-local-health-authorities-consolidate-their-medical-data-in-riayati-platform) and Gulf News, Dec 2021 (https://gulfnews.com/uae/health/uae-launches-unified-medical-records-platform-for-all-patients-1.84167227).
- **Confidence:** Confirmed (primary) for the obligation and date. **Unconfirmed:** the resolution number (a vendor blog, Pabau, cites "Ministerial Resolution No. 156 of 2021"), the claim that connection is checked at licensing and renewal (Pabau only), and MOHAP's "Connected EMRs" list (https://mohap.gov.ae/en/riayati/connected-emrs, not reviewed).
- **As of 2026-10:** Riayati remains MOHAP's HIE.

### MOH-02: Integrate with Riayati using HL7 v2 (FHIR, IHE and CDA also supported) and the mandated code sets
- **Requirement:** HL7 v2 is the recommended standard for submitting and accessing data. FHIR (gateway and IHE mobile profiles), IHE profiles and HL7 CDA (batch) are also supported. Message types include ADT (A01 to A60 range), ORU^R01, OMP^O09, RAS, RDS^O13, PPR, VXU^V04, MDM and MFN^M13. Code sets: ICD-10-CM 2018, SNOMED CT, CPT-4 2018, LOINC, UAE Drug List, CDT, IR-DRG, CVX. Patient matching uses IHE PIX/PIXm against the HIE master patient index.
- **Engineering implication:** Build HL7 v2 feeds for the listed events, a terminology service with ICD-10-CM, SNOMED CT, CPT, LOINC and UAE Drug List mappings, and PIX-based identity cross-referencing. Plan FHIR for app and query use cases.
- **Source:** Riayati Integration, Terminology and Coding Standards v2.1 (RYT-PGM-TECH001), 29 Jun 2022. https://riayati.mohap.gov.ae/resources/Riayati_Integration_Terminology_and_Coding_Standards_v2.1.pdf
- **Confidence:** Confirmed (primary). The document uses "recommended" and "supported" rather than mandatory language. The HL7 v2.x and FHIR release versions are not stated (unconfirmed). "Riayati HL7 & CCDA Specifications" is listed on https://mohap.gov.ae/en/riayati/resources but was not reviewed.
- **As of 2026-10:** v2.1 is the latest version located.

### MOH-03: Meet the Riayati Information and Cyber Security Standard before connecting
- **Requirement:** Applies to MOHAP-regulated healthcare entities that use Riayati HIE services, and to staff with access to PHI. Controls are tiered Basic, Foundational and Organisational. Large hospitals (21+ beds), payers and **cloud EMR vendors** must meet all three tiers. Small hospitals (up to 20 beds) must meet Basic and Foundational. Annual audits and technical assessments are expected ("ideally annually"). MOHAP may run a security assessment before authorising connectivity. Basic compliance is targeted within eight weeks of expressing interest, with an "ideal timeline" of six months.
- **Engineering implication:** A cloud EMR vendor should map its control set to this standard's ten domains (HR, asset, physical, access control, operations, communications and application security, healthcare information security, third parties, incident management, continuity). Controls map to UAE IA, ISO 27001:2013, NIST 800-53 and CIS, so an ISO 27001 ISMS gives a head start. Keep an evidence pack ready for the pre-connection assessment.
- **Source:** Riayati Information and Cybersecurity Standard v1.0 (RYT-PGM-POL-002), 25 Oct 2020, sections 4.1, 4.8.3, 4.8.5 and 4.8.6. https://riayati.mohap.gov.ae/ar/resources/Riayati_PGM_POL_Information-and-Cyber-Security-Standard_v1.0.pdf
- **Confidence:** Confirmed (primary). Only sections up to about 5.4 could be read. Domain 5.5 to 5.10 detail (encryption specifics, log retention, incident-reporting timelines, hosting location) is **unconfirmed**.
- **As of 2026-10:** v1.0 is the latest version located.

### MOH-04: Riayati access-control controls: unique IDs, privileged-account separation, protected passwords, encrypted remote access
- **Requirement:** Required controls include an access control policy (ACM 4.1.1), unique user IDs and timely de-registration (ACM 4.2.1), restricted privileged access with privileged IDs separate from regular IDs (ACM 4.2.2), formal credential management (ACM 4.2.3), storing and transmitting passwords in protected form (ACM 4.2.5), logging changes to privileged accounts (ACM 4.4.1), remote access over secure channels such as VPN or encrypted media (ACM 4.5.2), virtual desktops that prevent storage on privately owned equipment for teleworking (ACM 4.3.1), and secure disposal so that data is not recoverable (ASM 2.5.1, PHE 3.3.7).
- **Engineering implication:** Separate admin accounts with MFA and just-in-time elevation, salted password hashing (for example Argon2 or bcrypt, an engineering choice), privileged-change logs, VPN or zero-trust remote access, controls on bring-your-own-device data (VDI or no local caching), and crypto-erase or certified media disposal.
- **Source:** As MOH-03.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### MOH-05: Telehealth platforms must be licensed and run on UAE-based servers with backup, backup power and fallback communications
- **Requirement:** A facility providing telehealth must obtain a Health Authority licence and provide: (b) a well-rounded technical network per Health Authority standards; (c) adequate bandwidth; (d) alternative communication channels between provider and recipient; (e) backup power; (f) "Servers within the State to store and record the information and a backup"; (g) up-to-date internet technologies; (h) high-quality telephone and call-recording systems.
- **Engineering implication:** Host the telehealth platform, including recordings and backups, in the UAE. Provide a fallback channel (for example phone or audio-only) when video fails, quality-of-service monitoring, and recording infrastructure. Video/WebRTC media relays (TURN servers) and recording storage should be UAE-resident.
- **Source:** Cabinet Resolution No. 40 of 2019, Annex (Telehealth), General Controls 1(a) to (h). https://uaelegislation.gov.ae/en/legislations/1191/regulations/753/download ; also https://www.ehs.gov.ae/app_content/legislations/php-law-en-79/files/basic-html/page8.html
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Status Active on uaelegislation.gov.ae. MOHAP-specific telehealth standards beyond this annex were **not located** (unconfirmed).

### MOH-06: Identify telehealth patients and record their consent to the service and to recording
- **Requirement:** Facilities must develop a system for identifying the person receiving telehealth. Recipients must agree to receive the service, "give consent to audio, video and other forms of recording", and give correct and complete information about themselves.
- **Engineering implication:** Identity verification at session start (for example Emirates ID or UAE PASS, an engineering choice), plus explicit consent capture for the service and for recording, stored with the encounter. Block recording if consent is absent.
- **Source:** Cabinet Resolution No. 40 of 2019, Annex. URL as MOH-05.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### MOH-07: Telehealth records must be complete, access-controlled and time-stamped, and data shared with patient consent
- **Requirement:** Facilities must maintain and document full records and data of recipients, allowing only authorised persons access. Remote-monitoring data must be "stamped with the date and time to protect them from alteration" and documented in the electronic medical record. Before sending a patient's data to another facility for remote diagnosis, obtain consent, limit it to what the case requires and apply precise procedures for transfer, storage, protection and confidentiality.
- **Engineering implication:** Write telehealth encounters, recordings and device data into the EMR with trusted timestamps and integrity protection. Use a consent-gated referral or data-share workflow with field minimisation. The 25-year retention under FED-07 applies.
- **Source:** Cabinet Resolution No. 40 of 2019, Annex, items 1 (records), 1(c) (data exchange) and 4(d) to (e) (monitoring). URLs as MOH-05; page 10 at https://www.ehs.gov.ae/app_content/legislations/php-law-en-79/files/basic-html/page10.html
- **Confidence:** Confirmed (primary). The annex sets no retention period of its own.
- **As of 2026-10:** Current.

### MOH-08: Remote prescribing must follow diagnosis, electronic examination and documentation controls
- **Requirement:** Telehealth treatment may be prescribed only by a doctor licensed in the State working in a licensed telehealth facility, after an electronic examination. Files and documents must be certified under the applicable rules. Prescriptions must be based on a diagnosis with no contraindications, with the patient understanding the risks or else being referred. The treatment plan should be discussed with the patient. Medicines are prescribed according to legislation in force. Limited exceptions apply (consultation with a physician who has a direct relationship with the patient, or on-call coverage with record review).
- **Engineering implication:** The e-prescribing workflow must check the prescriber's licence and facility licence, require documented diagnosis and an examination note before signing, run contraindication checks, and capture patient-understanding attestation or a referral. Controlled-medicine rules come from other legislation (unconfirmed here).
- **Source:** Cabinet Resolution No. 40 of 2019, Annex, item 3(a) to (g). URLs as MOH-07.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

### MOH-09: Remote monitoring needs a signed physician order, reliable data collection and clinical review
- **Requirement:** Telehealth monitoring requires a full application to install and activate the service, a signed and dated order from the treating physician, accurate and reliable data collection, and review of monitoring data by a nurse or assistant doctor who reports to the physician. During remote medical intervention, a health professional must be with the patient to intervene if equipment malfunctions.
- **Engineering implication:** An RPM (remote patient monitoring) module needs order capture with e-signature, device data validation (calibration and range checks), review queues with escalation, and audit of who reviewed what and when.
- **Source:** Cabinet Resolution No. 40 of 2019, Annex, items 4 and 5(b). URLs as MOH-07.
- **Confidence:** Confirmed (primary)
- **As of 2026-10:** Current.

