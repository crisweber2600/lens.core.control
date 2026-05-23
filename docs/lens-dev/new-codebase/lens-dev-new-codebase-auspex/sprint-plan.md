---
feature: lens-dev-new-codebase-auspex
doc_type: sprint-plan
status: draft
track: express
updated_at: "2026-05-23T00:00:00Z"
depends_on: []
blocks: []
key_decisions:
  - "Sequence MVP1 as workflow contract first, snapshot data contract second, source integrations third, and read-only validation throughout."
  - "No FinalizePlan bundle artifacts are created during ExpressPlan."
  - "All implementation stories must include Given/When/Then validation and BMB channel usage where lens-work artifacts change."
open_questions: []
---

# Sprint Plan — Auspex Preferred Workflow and MVP1 Snapshot

## Sprint Overview

One focused implementation sprint should be enough for MVP1 if FinalizePlan keeps scope to a static, read-only reporting snapshot and preferred workflow contract. The work targets:

`TargetProjects\lens-dev\new-codebase\lens.core.src`

The sprint must not write implementation files in the control repo root and must not write governance feature docs directly.

## Story Map

| Story | Goal | Primary Files / Areas | Type |
|---|---|---|---|
| S1 | Define the Auspex preferred workflow contract and command boundary. | `_bmad\lens-work\skills\lens-reporting-snapshot\SKILL.md`, prompts/help docs as needed | Skill/workflow |
| S2 | Implement the MVP1 snapshot schema and static report generator. | Reporting snapshot scripts and tests | Code + tests |
| S3 | Integrate Derived Map, map-audit, ledger, and Salmon source adapters. | Projection/audit/ledger/Salmon readers | Code + tests |
| S4 | Enforce read-only boundaries and output routing. | Snapshot command, boundary tests | Code + tests |
| S5 | Add end-to-end workflow validation and documentation. | Tests, command docs, generated sample fixture | Test/docs |

## S1 — Preferred Workflow Contract

**Goal:** Make the Auspex workflow explicit and discoverable.

**Acceptance criteria:**

- The skill or command documentation defines the sequence: context verification → projection rebuild/read → map audit → ledger promotion context → Salmon status → reporting snapshot.
- The command states that Auspex is read-only and not source truth.
- Public wrapper paths use installed `lens.core/` boundary forms.
- BMad Builder documentation was consulted before editing any skill/workflow artifacts.
- BMB channel usage is recorded for any `lens-work` skill/workflow changes.

**Given/When/Then validation:**

- **Given** the updated skill contract,
- **When** a maintainer reads the command flow,
- **Then** the next operational step and write boundary are unambiguous.

## S2 — MVP1 Snapshot Schema and Generator

**Goal:** Produce static report artifacts from fixture inputs.

**Acceptance criteria:**

- Generator emits at least one machine-readable status file and one markdown summary.
- Machine-readable output includes `read_only: true`, generated timestamp, scope, source list, freshness, lifecycle, audit, Salmon, risks, and blockers fields.
- Output path is explicit and never defaults to governance feature docs.
- Report can be regenerated without changing source inputs.

**Given/When/Then validation:**

- **Given** fixture feature metadata and derived/audit/Salmon inputs,
- **When** the generator runs,
- **Then** report artifacts are created with deterministic required fields and source references.

## S3 — Source Adapters: Derived Map, Audit, Ledgers, Salmon

**Goal:** Connect the snapshot to current Lens evidence sources without mutating them.

**Acceptance criteria:**

- Derived Map reader reports `fresh`, `stale`, or `missing`.
- Map audit reader summarizes severity counts and unresolved findings.
- Ledger reader records which landscape ledgers were read.
- Salmon reader summarizes open, accepted, resolved, and blocked signals.
- Missing optional sources produce warnings and recommendations, not crashes, unless the requested scope cannot be resolved.

**Given/When/Then validation:**

- **Given** fixtures for present, stale, and missing evidence sources,
- **When** the adapters run,
- **Then** the report includes the expected source statuses and actionable next commands.

## S4 — Read-Only Boundary Enforcement

**Goal:** Prove Auspex cannot become an accidental writer.

**Acceptance criteria:**

- Attempts to write report output under governance feature docs fail before mutation.
- Snapshot generation does not modify `feature.yaml`, ledgers, derived-map inputs, or Salmon evidence.
- Tests compare source file hashes before and after generation.
- Any path traversal or outside-root output attempt exits non-zero with a structured error.

**Given/When/Then validation:**

- **Given** an output path inside the governance feature docs tree,
- **When** the generator is invoked,
- **Then** it exits with a boundary error and writes no files.

## S5 — End-to-End Workflow Validation

**Goal:** Validate the preferred workflow enough for lens-dev handoff.

**Acceptance criteria:**

- A test or scripted walkthrough exercises the preferred order using fixtures or existing command outputs.
- The final report includes lifecycle phase, next-command recommendation, map/audit health, and Salmon summary.
- Documentation states that richer dashboard/server behavior is deferred.
- FinalizePlan stories preserve express-track input contract: business-plan, tech-plan, sprint-plan, and review context.

**Given/When/Then validation:**

- **Given** a feature with planning docs, derived projection, audit warnings, and Salmon signals,
- **When** the Auspex workflow is run,
- **Then** the snapshot summarizes the system state and recommends the next Lens command without source mutations.

## Dependencies

- Existing `lens-projection-rebuild`, `lens-map-audit`, `lens-ledger-promotion`, `lens-salmon-impact`, and `lens-reporting-snapshot` surfaces in the target repo.
- BMad Builder module and documentation for any skill/workflow changes.
- Feature metadata and lifecycle validators for current phase/track interpretation.

## Risks Preserved for FinalizePlan

| Risk | Story Impact | Mitigation |
|---|---|---|
| Reporting snapshot output location is ambiguous | S2/S4 | Require explicit safe output dir and boundary tests. |
| Existing source commands expose inconsistent data shapes | S3 | Add tolerant adapters and fixture-driven tests around minimum fields. |
| Full dashboard expectations creep into MVP1 | S1/S5 | Keep serving/UI out of acceptance criteria. |
| BMB channel is skipped during implementation | S1/S5 | Make BMB and BMad Builder docs explicit story acceptance criteria. |

## Definition of Done for Dev Handoff

- FinalizePlan produces epics/stories that include BMB, Given/When/Then, and read-only boundary requirements.
- No story requires direct governance patching.
- No story requires FinalizePlan artifacts to have been created during ExpressPlan.
- MVP1 can ship as static report generation with traceability and safe regeneration.
