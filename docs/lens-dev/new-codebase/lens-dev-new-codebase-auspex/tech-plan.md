---
feature: lens-dev-new-codebase-auspex
doc_type: tech-plan
status: draft
track: express
updated_at: "2026-05-23T00:00:00Z"
depends_on: []
blocks: []
key_decisions:
  - "Implement Auspex MVP1 as a read-only reporting snapshot over existing Lens artifacts and derived projections."
  - "Keep report output separate from governance, Feature Archive, and Landscape source truth."
  - "Use BMB and BMad Builder reference docs for any lens-work skill, workflow, prompt, or module artifact edits."
open_questions: []
---

# Tech Plan — Auspex Preferred Workflow and MVP1 Snapshot

## Architecture Intent

Auspex is the read-only visibility plane for Lens. It consumes the Feature Archive, Landscape ledgers, Derived Map, map-audit outputs, lifecycle metadata, and Salmon signals to produce a current snapshot. It does not promote ledgers, update feature.yaml, mutate governance mirrors, or replace lifecycle validators.

The implementation target is `TargetProjects\lens-dev\new-codebase\lens.core.src`, primarily under `_bmad\lens-work\`. Any skill, workflow, prompt, or module metadata changes must be made through the BMad Builder channel and must consult:

`TargetProjects\lens\lens-governance\externaldocs\bmad-builder-docs\llms-full\index.md`

## Proposed Component Boundaries

| Component | Responsibility | Expected Location |
|---|---|---|
| Preferred workflow conductor | Document/run the sequence and guardrails for projection, audit, ledger, Salmon, and reporting steps. | `_bmad\lens-work\skills\lens-reporting-snapshot\SKILL.md` or a dedicated Auspex skill if FinalizePlan chooses one. |
| Snapshot generator | Read derived/map/audit/Salmon inputs and emit static report artifacts. | `_bmad\lens-work\skills\lens-reporting-snapshot\scripts\...` or `_bmad\lens-work\scripts\...` |
| Output schema | Machine-readable report model with source references and freshness metadata. | Snapshot script and tests. |
| Prompt/public wrapper updates | Expose the command using installed `lens.core/` boundary paths. | `.github\prompts\...` and `_bmad\lens-work\prompts\...` as required. |
| Regression tests | Prove read-only behavior, source traceability, and snapshot content. | Existing `_bmad\lens-work\scripts\tests\` or skill-local tests. |

## Workflow Contract

The preferred Auspex workflow is:

1. **Context and setup verification**
   - Resolve active feature/domain/service from feature metadata or explicit args.
   - Run preflight/postflight boundaries where the command contract requires them.
   - Refuse to infer feature identity from branch or open files.
2. **Derived Map rebuild**
   - Invoke or consume `lens-projection-rebuild` output.
   - Record source roots, generated map path, freshness timestamp, and rebuild status.
3. **Map audit**
   - Invoke or consume `lens-map-audit`.
   - Include audit severity counts, unresolved findings, and source references.
4. **Ledger promotion context**
   - Read the state produced by `lens-ledger-promotion` when present.
   - Do not promote or rewrite ledger facts from reporting.
5. **Salmon impact context**
   - Read or invoke `lens-salmon-impact` status.
   - Summarize upstream-impact signals by status and affected entity IDs.
6. **MVP1 reporting snapshot**
   - Emit static JSON/YAML plus markdown summary.
   - Include next-command recommendations derived from lifecycle and audit state.

## Output Contract

The MVP1 report output should be static and safe to regenerate. Candidate files:

| File | Purpose |
|---|---|
| `status.json` | Canonical machine-readable snapshot for agents and future UI. |
| `stakeholder-summary.md` | Human-readable summary of active state, risks, blockers, and next actions. |
| `read-only-report.md` | Traceability-focused report with source file references. |
| `dashboard.html` | Optional generated static view; defer unless it can be produced without expanding scope. |

Minimum `status.json` shape:

```json
{
  "report_type": "auspex-status",
  "read_only": true,
  "generated_at": "ISO-8601",
  "scope": {"feature_id": "...", "domain": "...", "service": "..."},
  "sources": [{"path": "...", "kind": "feature-yaml|ledger|derived-map|audit|salmon"}],
  "freshness": {"derived_map": "fresh|stale|missing", "details": "..."},
  "lifecycle": {"phase": "...", "track": "...", "next_command": "..."},
  "audit": {"status": "pass|warn|fail", "findings": []},
  "salmon": {"open": 0, "accepted": 0, "resolved": 0, "blocked": 0},
  "risks": [],
  "blockers": []
}
```

## Source-of-Truth Rules

- `feature.yaml` remains the lifecycle authority.
- Landscape ledger files remain the human-authored service/domain/program truth.
- Derived Map remains generated projection/cache, not source truth.
- Salmon records remain upstream-impact signal evidence and do not auto-change ledgers.
- Auspex reports are regenerated views and must include source references.
- Governance repo writes remain restricted to approved publication or feature-yaml operations; reporting must not patch governance docs directly.

## Implementation Guardrails

- Use Windows-safe path handling with `pathlib.Path`; no string path concatenation for output routing.
- Default to explicit `--output-dir` or a configured report root under generated output; never write report artifacts into governance feature folders.
- Fail closed if a requested source path resolves outside allowed roots.
- Keep report generation deterministic enough for snapshot-style tests.
- If credentials or external service settings are introduced later, add credential/security documentation before implementation closure. MVP1 should not require credentials.
- If any `/v1/` route is introduced later, add OpenAPI documentation and tests; MVP1 should avoid API routes.

## Test Strategy

Every coding story must define a Given/When/Then scenario and execute it.

Recommended regression coverage:

| Scenario | Given | When | Then |
|---|---|---|---|
| Read-only snapshot | A fixture governance repo with feature.yaml, ledgers, derived map, audit output, and Salmon records | Snapshot generator runs | Report files are created and source files are byte-for-byte unchanged. |
| Missing derived map | Feature metadata exists but projection output is absent | Snapshot generator runs | Report marks derived map as `missing` and recommends projection rebuild. |
| Stale map | Projection timestamp/hash is older than source metadata | Snapshot generator runs | Report marks freshness as `stale` and includes affected source paths. |
| Audit warnings | Map audit fixture contains warnings | Snapshot generator runs | Summary includes severity counts and warning details. |
| Salmon signals | Salmon fixture contains open and resolved signals | Snapshot generator runs | `status.json.salmon` counts match fixtures and source references are preserved. |
| Boundary guard | Output path points into governance feature docs | Snapshot generator runs | Command exits non-zero and writes nothing. |

## Rollout Notes

- Keep MVP1 focused on static report generation and preferred workflow documentation.
- Defer dashboard serving, cross-workspace aggregation, and rich visual UX until after MVP1 proves the data contract.
- FinalizePlan should split workflow/conductor, generator/schema, integration, and tests into separate stories.
