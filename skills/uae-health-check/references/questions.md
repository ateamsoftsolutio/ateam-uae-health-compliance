# Questions for the engineering team

Ask only what the code and documents couldn't answer. Group the questions into one round, and offer your best guess where you have one so engineers can confirm quickly. Record each answer with the person's name, marked "(stated)".

## Scope (asked first, in the check skill's Step 1)

1. What does the system do, and who uses it (clinicians, patients, admin staff, insurers)?
2. Which emirates will it be used in, and who licenses the customer: DoH, DHA, MOHAP, or Dubai Healthcare City?
3. Is it built for one facility, or sold as a product to many?
4. In Abu Dhabi, what kind of facility is the customer (hospital and bed count, centre, clinic, insurer), so the ADHICS tier can be set?
5. Does it have AI features? Does any of them diagnose, treat, monitor or recommend clinical action?
6. Does it offer telehealth or remote monitoring?
7. Must it connect to Malaffi, NABIDH or Riayati, or handle insurance claims?
8. Where is it hosted (provider and region), including backups and disaster recovery?

## Follow-up questions (only where evidence is missing)

**Residency and access:**

- Do any engineers, support staff or vendors access production systems or health data from outside the UAE? If yes, from where, and is there a DoH or DHA exemption? *(Blocker-priority whenever DoH or DHA applies.)*
- Do development or testing ever happen outside the UAE on systems that hold health data? (ADHICS SA 2.1(8))
- Which third-party services receive any patient data, and where do they process it?

**Keys and hosting:**

- Who controls the encryption keys: the cloud provider or the customer?
- Is the tenancy dedicated or shared? Is there a portability and exit plan?

**Contracts and governance:**

- Are there data-sharing agreements with every party that receives health data (DHA-13)?
- Does the contract give the facility source code custody and audit rights (DOH-13)?
- Is there a named data protection officer, an incident response plan and an AI use policy?
- When was the last penetration test, and who did it?

**HIE and claims:**

- Has onboarding started with Malaffi, NABIDH or Riayati? Have conformance tests been passed?
- Are the Abu Dhabi circular deadlines met: minimum dataset, referrals, Sahatna by 30 Nov 2026?

**AI:**

- Where are the models hosted? Is production data used for training?
- Has the AI been validated? By whom?
- Has an Emirates Drug Establishment classification been sought for clinical AI?

**Telehealth:**

- Is the platform approved or licensed by the regulator?
- Where are recordings stored?
