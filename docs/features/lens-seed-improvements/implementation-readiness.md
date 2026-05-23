---
feature: lens-seed-improvements
doc_type: implementation-readiness
status: draft
updated_at: "2026-05-23T00:00:00Z"
inputDocuments:
  - architecture.md
  - brainstorm.md
  - product-brief-distillate.md
  - product-brief.md
  - prd.md
  - research.md
  - ux-design.md
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
---

# Implementation Readiness: Lens Seed Improvements

## Readiness Verdict

Ready for Dev handoff with conditions.

The FinalizePlan package is coherent enough to hand to Dev. The epics, story definitions, sprint status, and per-story files define a viable implementation order for `TargetProjects/lens-dev/new-codebase/lens.core.src`, and the accepted temporary `belongs_to: unknown` topology waiver is visible in the planning packet. Dev must still treat topology waiver closure and Module Builder validation as release gates rather than optional cleanup.

This report does not claim source code has changed. It only assesses readiness of the planning bundle and required gates before Dev work begins.

## Required Pre-Dev Conditions

1. Story files exist for each `sprint-status.yaml` entry before Dev execution. `sprint-status.yaml` includes LSI-001 through LSI-010 and each entry points to an existing story file.
2. Each story file frontmatter includes `feature`, `story_id`, `doc_type: story`, `status`, `title`, `depends_on`, and `updated_at`.
3. The temporary topology waiver for `belongs_to: unknown` must remain visible. Verification of the pilot `service:lens-workbench` relationship, or a complete reviewed exception, must be a Dev story acceptance criterion and a closeout criterion.
4. Module Builder validation for `module.yaml`, `module-help.csv`, setup anti-zombie behavior, and progressive disclosure must be treated as a release gate, not documentation polish.
5. The implementation target repo is `TargetProjects/lens-dev/new-codebase/lens.core.src`. Dev edits must remain there unless a later approved boundary explicitly changes the target.

## Readiness Evidence

| Evidence | Status | Notes |
| --- | --- | --- |
| FinalizePlan review | Pass with warnings | Warnings are carried into the story bundle and this readiness gate. |
| Epics | Present | Five epics define metadata foundation, doctor/projection, Salmon/promotion, module/lifecycle compatibility, and release validation. |
| Stories | Present | LSI-001 through LSI-010 are defined with goals, scopes, dependencies, acceptance criteria, and validation notes. |
| Sprint status | Present | `sprint-status.yaml` includes LSI-001 through LSI-010 and tracks gate notes for topology waiver closure and Module Builder validation. |
| Per-story files | Present | Individual story files exist under `stories/` with required frontmatter and story-specific acceptance criteria. |
| Topology waiver | Present but temporary | `feature:lens-seed-improvements` still has `belongs_to: unknown`; Dev completion must verify `service:lens-workbench` or preserve a reviewed exception. |
| Target repo | Ready | Target repo is `TargetProjects/lens-dev/new-codebase/lens.core.src`. |

## Implementation Risk Matrix

| Risk | Story IDs | Severity | Required control |
| --- | --- | --- | --- |
| Shared metadata truth diverges across doctor, projection, Salmon, promotion, and lifecycle checks. | LSI-001, LSI-002, LSI-003 | High | Implement schema, fixtures, parser, resolver, waiver validation, and diagnostic contracts before command-surface changes. |
| Phase-aware severity or waiver behavior changes meaning between FinalizePlan, Dev, and Complete. | LSI-003, LSI-004, LSI-007, LSI-009, LSI-010 | High | Cover `belongs_to: unknown`, complete waiver, incomplete waiver, Dev completion, and reviewed exception states with fixtures. |
| Projection output becomes another hand-authored governance map. | LSI-005, LSI-010 | High | Require explicit output paths, deterministic check/write/explain modes, and no direct governance writes from implementation. |
| Salmon clustering promotes weak or heuristic matches as durable topology truth. | LSI-006, LSI-007 | Medium | Test Feature A/B/C deterministic clustering and keep weak similarity advisory only. |
| Ledger promotion bypasses provenance or hides material Salmon issues. | LSI-006, LSI-007 | High | Require source feature IDs, signal IDs, target ledger stable ID, doctor status, projection explain evidence, reviewer acceptance, and rationale. |
| Module registration drifts from implemented commands or leaves stale Lens rows after setup. | LSI-008, LSI-010 | High | Gate release on Module Builder validation of `module.yaml`, `module-help.csv`, setup anti-zombie idempotency, prompt references, and progressive disclosure. |
| Legacy topology compatibility reintroduces domain/service folder writes as authority. | LSI-009 | Medium | Preserve legacy reads as context only and validate new writes through stable metadata, `docs_path`, target repos, and waiver metadata. |
| Scope expands into full workbench UI, broad historical migration, or reporting authoring. | LSI-004, LSI-005, LSI-006, LSI-007, LSI-008, LSI-009 | Medium | Keep this release command/report/metadata-first; defer full UI and broad migration. |

## Dependencies

| Story ID | Dependency state |
| --- | --- |
| LSI-001 | No story dependency; must start implementation. |
| LSI-002 | Depends on LSI-001. |
| LSI-003 | Depends on LSI-001 and LSI-002. |
| LSI-004 | Depends on LSI-003. |
| LSI-005 | Depends on LSI-003 and LSI-004. |
| LSI-006 | Depends on LSI-001, LSI-002, LSI-003, and LSI-004. |
| LSI-007 | Depends on LSI-003, LSI-005, and LSI-006. |
| LSI-008 | Depends on LSI-005, LSI-006, and LSI-007. |
| LSI-009 | Depends on LSI-003, LSI-004, LSI-005, and LSI-007. |
| LSI-010 | Depends on LSI-001 through LSI-009. |

## Test Strategy

| Story ID | Required validation |
| --- | --- |
| LSI-001 | Unit tests for metadata schema fixtures, Feature A/B/C fixtures, invalid fixture cases, waiver completeness, and deterministic serialization. |
| LSI-002 | Parser unit tests for Markdown frontmatter, standalone `feature.yaml`, ledger records, normalized paths, duplicate stable IDs, missing fields, and compatibility read mode. |
| LSI-003 | Resolver unit tests for parent resolution, unknown parent handling, waiver validation, incomplete waivers, parent cycles, and relationship edges. |
| LSI-004 | Phase fixture tests and CLI contract tests for Markdown and JSON doctor output; assert doctor does not mutate archives, ledgers, projection caches, governance metadata, or config. |
| LSI-005 | CLI contract tests for `--check`, `--write`, `--explain`, `--json`, `--include-drafts`, deterministic output, drift detection, blocker refusal, and explicit output paths. |
| LSI-006 | Unit and report tests for Salmon materiality, legal state transitions, illegal transitions, Feature A/B/C deterministic clustering, advisory weak similarity, and JSON output. |
| LSI-007 | Promotion provenance tests, pilot ledger fixture tests, material Salmon blocking tests, and Dev-completion waiver closure tests for `service:lens-workbench`. |
| LSI-008 | Module asset validation tests for `module.yaml`, `module-help.csv`, orphan references, duplicate menu codes, broken references, weak descriptions, and setup anti-zombie idempotency. |
| LSI-009 | Lifecycle fixture tests for new two-tree records, legacy compatibility reads, branch/status separation, target repo readiness, waiver visibility, and Dev-completion blockers. |
| LSI-010 | Local release validation command or VM-style transcript covering parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, module validation, projection drift, and waiver closure. |

## Handoff Checklist

| Story ID | Dev handoff item | Gate |
| --- | --- | --- |
| LSI-001 | Create canonical metadata schema and golden fixtures before downstream command work. | Required first story. |
| LSI-002 | Implement reusable inventory/frontmatter parser in the target repo. | Requires LSI-001 fixtures. |
| LSI-003 | Implement relationship resolver and waiver validation as shared infrastructure. | Blocks doctor, projection, lifecycle, and promotion work. |
| LSI-004 | Wire `lens-doctor` to shared parser/resolver output with phase-aware findings. | Must remain read-only. |
| LSI-005 | Implement projection rebuild check, write, explain, JSON, and draft-inclusive modes. | Must own generated projection files only through explicit output paths. |
| LSI-006 | Implement Salmon schema, materiality, legal transitions, and deterministic cluster report. | Weak similarity remains advisory. |
| LSI-007 | Implement promotion provenance and verify or create the pilot `service:lens-workbench` relationship. | Required for waiver closure before Dev completion. |
| LSI-008 | Update module registration assets and setup anti-zombie behavior. | Module Builder validation is a release gate. |
| LSI-009 | Update lifecycle validation for two-tree features and compatibility reads. | Must enforce target repo readiness and waiver visibility. |
| LSI-010 | Run or document release validation for `lens.core.src`. | Blocks release/Dev closeout until module, command, test, projection, and topology-waiver checks pass. |

## Dev Entry Decision

Dev may proceed with the condition that topology waiver closure and Module Builder validation remain release gates. The bundle now includes `sprint-status.yaml` and per-story files with required metadata, so the remaining readiness risk belongs to implementation execution and closeout validation rather than missing FinalizePlan artifacts.