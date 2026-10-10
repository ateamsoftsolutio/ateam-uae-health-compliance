# UAE Healthcare Compliance by ATeam

**Know what the UAE regulators require, see what your system already does, and build the rest.**

This Claude plugin checks a healthcare software system against UAE healthcare regulations. It produces an engineering checklist: every applicable requirement, its source, the evidence found in your system, and the concrete task to close each gap. The checklist is kept as a register in your project and exported to Excel, and it is re-checked as engineers build.

It works in Claude Code and in Claude Cowork with a project folder. It was built by [ATeam Soft Solutions](https://www.ateamsoftsolutions.com), a software and AI engineering firm working with UAE healthcare providers, and is shared as a contribution to the community.

## Install

Search for "UAE Healthcare Compliance by ATeam" in Claude's plugin directory, or in Claude Code run:

```
/plugin install ateam-uae-health-compliance --marketplace ateamsoftsolutio/ateam-uae-health-compliance
```

## The three skills

| Skill | Say | What it does |
|---|---|---|
| `uae-health-check` | "Run UAE Health Check" | Scopes the system (emirates, regulator, ADHICS tier, AI, telehealth, HIE), checks code, configuration and infrastructure for evidence, asks about what code can't show, and writes the checklist and Excel file |
| `uae-health-recheck` | "Recheck UAE compliance" | Re-checks after fixes, updates statuses on evidence, flags anything that went backwards, and re-exports the Excel file |
| `uae-health-explain` | "What does ADHICS require for…?" | Explains any requirement in plain language, with its source and a design suggestion |

## What it covers (library dated 10 October 2026)

| Layer | Covers |
|---|---|
| Federal | ICT Health Law (Federal Law 2/2019) and its Executive Regulation (Cabinet Resolution 32/2020): data localisation, 25-year retention, Emirates ID, access, corrections, backups. Ministerial Resolution 51/2021 exceptions to local storage. The PDPL's scope for health data. The Medical Products Law (Decree-Law 38/2024) and Emirates Drug Establishment approval for software and AI medical devices. National cyber and cloud policies. |
| MOHAP and the Northern Emirates | Riayati integration and its security standard; telehealth rules (Cabinet Resolution 40/2019) |
| Abu Dhabi (DoH) | ADHICS v2 (2024) by tier, including Service Provider controls for vendors; Malaffi and Sahatna integration and circular deadlines; Responsible AI Standard (2025); telemedicine standard |
| Dubai (DHA) | NABIDH interoperability, onboarding and information security; Consent and Access Control standard (2025); data sharing policy (2024); AI policy (2021); Telehealth Standards v4 (2025) |
| Claims | DoH Shafafiya and DHA eClaimLink (brief) |

Every requirement in the library cites its source document. Each one is marked **Confirmed (primary)**, **Confirmed (secondary)** or **Unconfirmed**, with an "as of" note. Unconfirmed items become questions in your checklist, never hard requirements.

Facilities in Dubai Healthcare City are regulated by DHCA and are not covered by this version.

## How it works

1. Open your project folder and say **"Run UAE Health Check"**.
2. Confirm the scope Claude drafts from your project.
3. Claude checks the system theme by theme: hosting and residency, offshore data flows, identity and access, audit logging, data model, encryption and keys, interoperability, development lifecycle, backup, AI and telehealth.
4. Answer one round of questions about what code can't show, such as contracts, approvals and team location.
5. You get `.uae-health-compliance/CHECKLIST.md`, plus an Excel checklist with Summary, Checklist, Questions, Watch items and Sources sheets.
6. Build the tasks, then say **"Recheck UAE compliance"**.

Statuses are Met, Partly met, Missing, Not applicable and Needs confirmation. Priorities run from Blocker (a legal prohibition or licensing gate) through High, Medium and Low.

## Make it yours

Copy `compliance-house-rules.md` into your project and edit it to set:

- the approver
- the Excel font
- where the register is kept
- your own internal requirements

## Important limits

- **Engineering readiness, not certification.** The plugin shows which requirements have evidence in your system. It does not make a system compliant or replace a regulator's assessment, an audit or legal advice.
- **Rules change.** The library is dated, and Claude warns you when it's more than 6 months old or a deadline has passed. Check the regulator's current text before relying on a requirement.
- **Instructions only.** It contains instructions for Claude and a requirements library. It has no MCP servers, hooks or scripts, and it sends no data anywhere. See [PRIVACY.md](PRIVACY.md).

## Privacy

See the [Privacy policy](PRIVACY.md).

## Licence

MIT. See [LICENSE](LICENSE).
