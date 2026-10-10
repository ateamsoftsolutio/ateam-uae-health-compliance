---
name: uae-health-explain
description: Explains UAE healthcare compliance requirements to engineers in plain language, with sources and design guidance. Covers ADHICS v2, Malaffi, Sahatna, NABIDH, Riayati, the ICT Health Law and data localisation, consent rules, AI standards, telehealth standards and EDE medical device rules. Use when someone asks "what does ADHICS require for X", "can we use OpenAI with UAE patient data", "how should consent work for NABIDH", or "explain DOH-07".
---

# UAE Health Explain

This skill answers engineers' questions about UAE healthcare requirements, using the library in `../uae-health-check/references/library/`, and helps them design a compliant way to build things.

## How to answer

1. **Find the requirement(s)** in the library files. Search by topic, regulator or ID.
2. **Answer in plain language:**
   - what the rule says
   - who it applies to: which regulator, which ADHICS tier, which product types
   - what it means for the system
3. **Cite the library ID and source**, for example "DOH-07 (ADHICS v2 DP 1.4, CO 9.2)", and the item's confidence and "as of" note.
4. **Give a design suggestion:** one or two practical ways to meet it, for example a UAE-hosted model, customer-managed keys, geo-fenced access, or an amendment table for corrections.
5. **Flag uncertainty honestly:**
   - If the library marks the item Unconfirmed, say so and suggest how to confirm it: the regulator's document, onboarding contact or circular.
   - If the question isn't covered by the library, say so plainly. Don't answer from memory as if it were a rule. You may share general knowledge, clearly labelled as unverified.
6. **Note conflicts.** Where regulators differ, say so and recommend the stricter rule for systems used in several emirates.
7. **Close** with: "Engineering guidance from a library dated <date>, not legal advice."

## Common questions to be ready for

- **Offshore services and AI APIs:** the federal exceptions (FED-21 to FED-24), plus the stricter Abu Dhabi rule covering anonymised data (DOH-07) and UAE hosting for AI (DOH-32).
- **Offshore teams and support access:** DOH-07, DOH-11, DHA-11. Explain the exemption route, and that this is a reading of the text.
- **Retention and deletion:** FED-07 (25 years), FED-19, DHA-23.
- **Consent models:** DOH-16, DOH-27, DHA-07, DHA-08, MOH-06.
- **HIE integration:** DOH-23 to DOH-29, DHA-01 to DHA-04, MOH-01, MOH-02.
- **Whether a feature is a medical device:** FED-28, FED-29. Explain that classification is decided by the Emirates Drug Establishment.

If the engineer wants the answer applied to their project, suggest running `uae-health-check`.
