# Checklist register and Excel export

The register in the project is the source of truth. The Excel file is generated from it after every check or recheck.

## Folder layout

```
.uae-health-compliance/
├── scope.md            Scope answers, regulators, ADHICS tier, library version used
├── checklist.json      The register (source of truth)
├── CHECKLIST.md        Readable view, regenerated after every change
├── history.jsonl       Append-only event log
├── runs/C1.md          One summary per check run
└── export/             Excel checklists, one per run
```

**Git rules:** commit the folder in private repositories so the team shares one checklist. Don't commit it in public repositories: it describes the system's compliance gaps.

## checklist.json

```json
{
  "schema": "uae-health-checklist/1",
  "project": "Clinic booking portal",
  "libraryAsOf": "2026-10-10",
  "regulators": ["DoH", "DHA"],
  "adhicsTier": ["Basic", "Service Provider"],
  "runs": [{ "id": "C1", "date": "2026-10-12", "commit": "a1b2c3d", "readiness": { "met": 18, "partly": 9, "missing": 14, "confirm": 11, "na": 22, "info": 4, "percent": 35 } }],
  "items": [
    {
      "ref": "DOH-07",
      "regulator": "DoH",
      "source": "ADHICS v2 DP 1.4, CO 9.2",
      "requirement": "Health data, including anonymised copies, must not be stored, processed or accessed outside the UAE without a DoH exemption.",
      "engineeringTask": "Move error tracking off the US-hosted Sentry project or strip all patient data before sending; geo-fence production access to the UAE.",
      "taskId": "T-03",
      "satisfies": ["DOH-07", "FED-04", "DHA-15"],
      "theme": "Hosting and residency",
      "priority": "Blocker",
      "status": "Missing",
      "evidence": "infra/main.tf:12 region me-central-1 (UAE) ✔; src/app.ts:8 Sentry DSN to a US ingest endpoint ✘",
      "statedBy": null,
      "owner": null,
      "due": null,
      "libraryConfidence": "Confirmed (primary)",
      "regressed": false,
      "notes": ""
    }
  ]
}
```

## Status values

| Status | Meaning |
|---|---|
| **Met** | Evidence found that satisfies the requirement. Cite it. |
| **Partly met** | Some evidence, with a gap. Say what's missing. |
| **Missing** | No evidence, or evidence that contradicts the requirement. |
| **Not applicable** | Doesn't apply to this scope. A reason is required. |
| **Needs confirmation** | Can't be decided from the system: waiting for an answer, a document, or confirmation of an unconfirmed library item. |
| **Informational** | A scope, penalty or status provision with nothing to build or test (for example FED-01, FED-20, FED-35, DOH-22). Listed for awareness. |

**Readiness** = Met ÷ (all items − Not applicable − Informational), as a whole-number percentage. Always label it "indicator only, not certification".

**Never mark an item Met on someone's word alone** when the evidence should be visible in the system. A stated answer is enough only for things outside the system: contracts, approvals, policies, team location. Record those as "Met (stated by <name>)".

`regressed` is set to true by `uae-health-recheck` when an item that was Met loses its evidence.

## Priority

| Priority | When to use it |
|---|---|
| **Blocker** | A legal prohibition, or a gate on licensing or HIE connection: data localisation and offshore access, a required HIE connection, Emirates Drug Establishment approval for medical device software, a regulator deadline |
| **High** | Core security and privacy controls: access control, audit logging, encryption, consent, breach readiness, retention |
| **Medium** | Process and documentation controls with a clear engineering element |
| **Low** | Good practice and future-proofing |

## Shared tasks

Where one piece of work satisfies several items, give it one task ID (`T-01`, `T-02`, …). Put the same `taskId` on each of those items and list them all in `satisfies`. The recheck then closes them together, but only on evidence for each item.

## Blocker grouping

In summaries, group Blockers by root cause, for example:

- offshore data flows
- offshore access and development
- HIE connection
- regulator deadlines
- medical device approval
- telehealth approval

Show the item references under each group, so the list stays scannable.

## Ordering the "next tasks"

List High-priority tasks in this order:

1. tasks that close the most items (`satisfies` count)
2. then by theme: access control, audit logging, data model and retention, encryption, consent, incident readiness, development lifecycle, other
3. then by reference

## Engineering task wording

Each item needs one concrete, testable task. Examples:

- Good: "Add a 12-character minimum with complexity, a 5-attempt lockout and 3-password history to the local login (auth/password.ts)."
- Bad: "Improve password security."

## history.jsonl

One JSON object per line, append only. Fields: `ts` (ISO 8601, UTC), `event`, `ref` (or `-` for run events), `run`, `commit`, and as relevant `from`, `to`, `by` (as stated, with "(stated)"), `evidence` and `note`.

Events: `run-start`, `created`, `status`, `owner`, `priority`, `note`, `run-end`.

```json
{"ts":"2026-10-12T09:00:00Z","event":"run-start","ref":"-","run":"C1","commit":"a1b2c3d"}
{"ts":"2026-10-12T09:40:00Z","event":"created","ref":"DOH-07","run":"C1","commit":"a1b2c3d","to":"Missing","evidence":"src/app.ts:8 Sentry US ingest"}
```

## Excel export

File name: `export/UAE-Health-Compliance-<project>-C<n>-<date>.xlsx`.

Build it with Python and openpyxl if available, with the xlsx skill if the session has one, or as CSV files if neither is available, and say so. Use the font from the house rules: Arial by default, 10 pt. No headers or footers.

**Sheets:**

1. **Summary:**
   - project, date, commit, regulators, ADHICS tier, library date
   - counts by status and by priority
   - open Blockers listed by name
   - a "Readiness" line (formula above), labelled as an engineering readiness indicator, not a compliance certification
   - Blockers grouped by root cause
2. **Checklist:** one row per item. Columns: Ref · Regulator · Theme · Requirement · Source · Task ID · Engineering task · Also satisfies · Priority · Status · Evidence · Stated by · Owner · Due · Library confidence · Notes.
   - Colour Informational light grey.
   - Freeze the header row and turn on filters.
   - Add a drop-down for Status and Priority.
   - Colour Status: Met green, Partly met amber, Missing red, Needs confirmation blue, Not applicable grey.
   - Wrap text and set sensible column widths.
3. **Questions:** open questions, who should answer, and the item each one unblocks.
4. **Watch items:** pending rules and deadlines from the library that touch this scope.
5. **Sources:** each regulator document used, with its link and the library's "as of" date.

Put this note at the bottom of the Summary sheet:

> "Engineering readiness checklist generated from a requirements library dated <date>. Not legal advice or certification; confirm requirements with the regulator or a qualified adviser."
