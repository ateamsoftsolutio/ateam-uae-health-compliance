# Finding evidence in the system

For each applicable requirement, look for evidence in the code, configuration, infrastructure files and documents. Record what you found and where (file and line, or document and section).

Some things can't be seen in code: contracts, policies, team location, regulator approvals, the hosting provider's actual setup. For those, ask the engineer (see `questions.md`) and record their answer as **stated by** that person.

Never print secrets you come across. Mask them (first 4 and last 4 characters) and suggest a security audit if you find any.

**If you find real patient data in the repository** (seed files, fixtures, logs, dumps):

1. Don't reproduce it anywhere, including the register, Excel file or chat.
2. Record only the file and the type of data.
3. Raise it as a High item: it may be a reportable incident (DOH-19, DHA-16), and it breaks the test-data rules (DOH-12, DHA-14).
4. Recommend removing it from the repository and its history.

**Application-security flaws** (for example IDOR, self-assigned roles, injection) are outside this library's scope. Note any you see under the closest access-control item, and recommend running a security audit (for example the Security Audit Loop plugin).

## 1. Hosting and data residency

FED-04, DOH-07, DOH-08, DHA-15, MOH-05, DHA-19

- **Where to look:** infrastructure-as-code (Terraform, CloudFormation, Bicep, Pulumi), Kubernetes and Helm values, docker-compose files, CI/CD deploy steps, `.env.example`, cloud SDK client configuration, database connection strings (host names only; never print credentials).
- **UAE regions** (confirm against the provider's current list):
  - AWS `me-central-1`
  - Azure `uaenorth` and `uaecentral`
  - Oracle Cloud Abu Dhabi and Dubai regions
  - UAE local providers
- **Not UAE:** AWS `me-south-1` is Bahrain. Any other region, or one you can't identify, is "Needs confirmation" until the engineer confirms it.
- **Also check:**
  - backup, DR and replica regions: cross-region replication, geo-redundant storage, and where snapshots are copied
  - CDN or edge caching of pages or files containing health data
  - where logs and backups land
- **Key ownership (DOH-08):** cloud-provider default keys don't meet ADHICS. Look for customer-managed or externally held keys.

## 2. Offshore data flows through third-party services

FED-21, DOH-07, DOH-32, DHA-14

Search dependencies (`package.json`, `requirements.txt`, `pom.xml`, `*.csproj`, `go.mod`, `Gemfile`) and code for SDKs and endpoints that could receive health data:

| Category | Examples |
|---|---|
| Analytics | Google Analytics, Mixpanel, Segment, Amplitude, Hotjar |
| Error tracking and logging/APM | Sentry, Bugsnag, Datadog, New Relic, Logtail |
| Email, SMS, push and WhatsApp | SendGrid, Mailgun, Twilio, Firebase messaging |
| Video for telehealth | Zoom, Agora, Twilio Video, Vonage |
| AI and LLM APIs | OpenAI, Anthropic, Google Gemini, Azure OpenAI (check the endpoint region), Hugging Face inference |
| Storage and backend-as-a-service | Firebase, Supabase, offshore S3 buckets |

For each one, decide whether health data or identifiers can reach it, for example in prompts, error payloads, email bodies or event properties. Then check whether it processes data in the UAE, or whether a listed exception applies. The DoH rule covers anonymised and de-identified data too (DOH-07).

## 3. Identity and access

FED-11 to FED-14, FED-16, DOH-03 to DOH-05, DHA-09 to DHA-12, MOH-04

- **Authentication:** auth library or identity provider, SSO, MFA for privileged and remote access.
- **Password rules:** ADHICS needs at least 12 characters with complexity, lockout and history.
- **Sessions:** idle timeout and logon banner.
- **Accounts:** unique accounts, no shared logins, and deactivation of inactive accounts. DHA sets 40 days; check DHA-10.
- **Roles:** role-based access tied to professional role, plus periodic access review support.
- **Break-glass:** emergency access with a recorded reason and alerting (FED-14, DHA-12).
- **Patient control:** patients can delegate access, or restrict access to their record (FED-13).
- **Sensitive records:** VIP and sensitive-record flags (DHA-12).
- **No hard-coded service credentials** (DOH-04).

## 4. Audit logging

DOH-06, DHA-10, DHA-13, FED-15

- Who viewed, created, changed or exported which patient record, and when, against a unique user.
- Logs are tamper-resistant (append-only or a write-once store) and time-synchronised.
- **Retention:** DHA access logs 6 years, remote-access logs 1 year, data-sharing logs 6 years. Check the library items for the current numbers.
- A query that answers "which patients were affected?" for breach reporting (DOH-19, DHA-16).

## 5. Data model and records

FED-07, FED-08, FED-15, FED-19, DOH-16, DHA-07, DHA-08, DHA-23, MOH-06

- **Emirates ID** is captured and validated in patient records and transactions (FED-08). The usual format is 15 digits starting with 784.
- **Retention:** clinical records kept at least 25 years from the last health procedure (FED-07). Look for hard deletes of clinical data, or retention jobs that purge too early.
- **Corrections** keep the original entry, plus who changed it, when and why (FED-15). Look for versioned records or an amendment history, not in-place updates.
- **Consent records:** capture, withdrawal and scope, with timestamps (DOH-16, DHA-07, DHA-08, MOH-06). Check the DHA opt-out behaviour, which anonymises identifiers rather than stopping the data flow (DHA-08).

## 6. Encryption and keys

DOH-08, DOH-09, FED-16

- TLS on all endpoints and internal service calls.
- Encryption at rest for databases, files and backups.
- Key management through a KMS or HSM, with keys not held by the hosting provider in Abu Dhabi (DOH-08).
- Encrypted email for patient data (FED-16).

## 7. Interoperability

DOH-23 to DOH-29, DHA-01 to DHA-04, MOH-01, MOH-02

- **Libraries:** HL7 v2 (for example HAPI, NHapi, hl7apy) and FHIR R4.
- **Message types:** compare against the catalogues in DHA-03 and DOH-29.
- **Configuration:** endpoint settings for Malaffi, NABIDH or Riayati, and the code sets in use.
- **Abu Dhabi deadlines:** DOH-24 to DOH-26.

## 8. Development lifecycle and supply chain

DOH-11 to DOH-14, DHA-14

- **CI pipeline:** static analysis, dependency scanning, signed builds or an SBOM.
- **Test data:** no production health data in test or development (DOH-12, DHA-14). Look for production database dumps, seed files with real names or Emirates IDs, and test configuration pointing at production.
- **Source code custody and escrow** for Abu Dhabi customers (DOH-13). This is a contract question.
- **Penetration test reports** and their dates (DOH-14).

## 9. Backup and continuity

FED-18, DOH-20

Backup schedules, restore tests, recovery objectives, and backups kept in the UAE.

## 10. AI features

FED-28 to FED-32, FED-36, DOH-30 to DOH-33, DHA-17, DHA-18

- Where models are hosted and where inference runs: UAE hosting is required by the DoH Responsible AI Standard (DOH-32).
- Whether production data is used for training (prohibited by DOH-32).
- Logs of inputs, outputs and model versions; human oversight and override; a patient opt-out from AI.
- Validation evidence (DHA-17).
- Whether the feature drives diagnosis or treatment. If it does, raise the Emirates Drug Establishment medical device question (FED-28, FED-29).

## 11. Telehealth

MOH-05 to MOH-09, DOH-34, DOH-35, DHA-19 to DHA-22

- Patient identity verification.
- Consent to the service and to recording.
- Where recordings are stored, including interim storage at the video provider before they reach your own storage, and where the video provider hosts its media.
- Consent for ambient AI note-taking (DHA-21).
- Remote prescribing controls.
- Two points to raise as Needs confirmation questions:
  - whether recordings form part of the health record; if so, short deletion periods may conflict with the 25-year retention rule (FED-07)
  - whether the facility holds the telemedicine licence or approval itself, as well as the platform's approval

## 12. Source code and CI hosting

Where is the git repository hosted, and where do CI runners and build artefacts live? This matters if any of them can contain health data, such as test fixtures, logs or dumps.

## 13. Things only people can answer

Team location and remote access, contracts and data-sharing agreements, regulator approvals and exemptions, policies (incident response, AI use, data protection officer), pen-test schedule, actual cloud account settings. Use `questions.md`.
