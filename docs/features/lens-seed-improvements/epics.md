---
feature: lens-seed-improvements
doc_type: epics
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

# Epics: Lens Seed Improvements

## Implementation Boundary

These epics are bounded to implementation in `TargetProjects/lens-dev/new-codebase/lens.core.src`. They create command, metadata, validation, and report foundations for the Lens two-tree model. They do not build a dashboard, Salmon Inbox UI, broad historical migration, or direct governance write flow.

The first implementation sequence must start with shared metadata mechanics before any skill prose or command surface is changed. Doctor, projection, Salmon, ledger promotion, lifecycle compatibility, and module validation must consume the same parser, resolver, waiver, and diagnostic contracts.

## Carry-Forward Gate

`feature:lens-seed-improvements` currently has `belongs_to: unknown` with an accepted temporary topology waiver. That waiver exists only because this feature must first prove the pilot ledger model. Dev completion must not treat the waiver as permanent. The implementation bundle must create or verify the pilot `service:lens-workbench` ledger path, attach or validate `feature:lens-seed-improvements` against that pilot ledger, and either resolve the unknown parent state or leave a complete, machine-readable, reviewed exception that is visible to lifecycle validation.

## Epic 1: Shared Metadata And Relationship Foundation

Goal: establish the source metadata, fixtures, parser, resolver, waiver, and diagnostic primitives that every downstream Lens command uses.

Why this comes first: architecture and review both identify parser/resolver drift as the highest implementation risk. If doctor, projection, Salmon, promotion, and lifecycle checks parse metadata independently, they will disagree about source truth.

Stories:

| Story ID | Title | Outcome |
| --- | --- | --- |
| LSI-001 | Shared metadata schema and golden fixtures | Canonical two-tree metadata, waiver, Salmon, and fixture contracts exist with testable Feature A/B/C and pilot ledger examples. |
| LSI-002 | Shared inventory and frontmatter parser | A reusable parser scans feature archives and landscape ledgers and emits normalized source entities and diagnostics. |
| LSI-003 | Relationship resolver and topology waiver validation | Parent, child, membership, relationship, cycle, and waiver validation are shared by all command consumers. |

Definition of done:

- New two-tree records have stable ID, entity type, lifecycle, publication, parent, path, waiver, and Salmon schema coverage.
- Golden fixtures include `feature:lens-seed-improvements`, Feature A/B/C Salmon fixtures, `service:lens-workbench`, duplicate ID cases, broken parent cases, invalid prefix cases, and waiver completeness cases.
- Parser and resolver tests cover Markdown frontmatter, standalone `feature.yaml`, ledger metadata, path normalization, duplicate IDs, `belongs_to: unknown`, broken parents, wrong parent types, and parent cycles.
- The temporary `belongs_to: unknown` waiver is represented as machine-readable input and has explicit acceptance and expiry or review semantics.

## Epic 2: Topology Doctor And Projection Rebuild

Goal: provide read-only phase-aware diagnostics and deterministic projection rebuild/check/explain behavior from the shared engine.

Why this comes next: projection write ownership and phase-aware severity are the core product controls that replace legacy path and branch inference.

Stories:

| Story ID | Title | Outcome |
| --- | --- | --- |
| LSI-004 | Topology doctor phase-aware severity | Doctor emits stable Markdown and JSON findings with phase-aware info, warning, and blocker severity. |
| LSI-005 | Projection rebuild check, write, explain, and JSON modes | Projection rebuild owns generated `governance-map.json` and `governance-map.md` output with deterministic modes and source explainability. |

Definition of done:

- Doctor is read-only and never mutates feature archives, ledgers, projection caches, governance metadata, or config.
- Severity fixtures cover BusinessPlan, TechPlan, FinalizePlan, Dev, Complete, and published projection states.
- `belongs_to: unknown` is warning in early planning, warning with accepted plan in TechPlan, blocker in FinalizePlan without waiver, and blocker in Dev/Complete unless an explicit reviewed exception exists.
- Projection defaults are pinned to `governance-map.json` and `governance-map.md` under the provided `--reporting-output-path`; local development should use an explicit output path and must not hand-copy into governance.
- `--check`, `--write`, `--explain <stable_id>`, `--json`, and `--include-drafts` have stable behavior and tests.

## Epic 3: Salmon Impact And Ledger Promotion

Goal: prove advisory Salmon signal handling, deterministic cluster reporting, and audited ledger promotion against the pilot service ledger.

Why this comes after projection: Salmon rollup and promotion need resolved stable IDs, phase-aware findings, and explainable projection edges before they can be reviewed safely.

Stories:

| Story ID | Title | Outcome |
| --- | --- | --- |
| LSI-006 | Salmon signal schema, state transitions, and deterministic cluster report | Salmon signals are parsed, validated, state-checked, and reported with deterministic Feature A/B/C clustering. |
| LSI-007 | Ledger promotion provenance and pilot `service:lens-workbench` verification | Promotion requires provenance and verifies or creates the pilot service ledger needed to close the temporary topology waiver. |

Definition of done:

- Salmon supports advisory, material, blocked, waived, and resolved states with legal transitions and required audit fields.
- Materiality is evidence-backed and only blocks when it meets architecture-defined thresholds.
- Feature A/B/C fixtures produce one deterministic candidate report when target stable ID and category match, while weak text similarity remains advisory only.
- Ledger promotion records source feature IDs, source paths, signal IDs, target ledger stable ID, projection explain output, doctor status, owner or steward acceptance, and promotion rationale.
- The pilot `service:lens-workbench` ledger is verified as the service-level home for this feature or its absence is reported as a Dev-completion blocker.

## Epic 4: Module Registration And Two-Tree Lifecycle Compatibility

Goal: align Lens module registration and lifecycle validation with the two-tree model so commands are discoverable, setup is anti-zombie, and legacy topology is read for compatibility without governing new writes.

Why this is separate: command surfaces and lifecycle gates must not be treated as documentation polish. They are release-critical because they determine whether the new mechanics are actually invoked by Lens flows.

Stories:

| Story ID | Title | Outcome |
| --- | --- | --- |
| LSI-008 | `module.yaml`, `module-help.csv`, prompt registration, and anti-zombie setup validation | Changed Lens commands are registered through module assets and setup removes stale Lens rows before inserting current rows. |
| LSI-009 | Compatibility and lifecycle validation for two-tree features | New two-tree features validate through stable metadata, while legacy topology remains readable but not a write requirement. |

Definition of done:

- `module.yaml`, `module-help.csv`, setup assets, prompt stubs, and command documentation reflect the changed Lens capabilities together.
- Anti-zombie setup behavior removes previous Lens module entries before adding current registrations.
- Skill frontmatter stays concise and procedural detail remains in scripts, assets, references, or prompt stages.
- Lifecycle validation reads stable IDs, `docs_path`, `target_repos`, waivers, and publication state rather than inferring identity or phase from branch or path.
- Compatibility tests prove legacy topology can be read for context while new writes remain under `docs/features/<feature_id>` with stable metadata.

## Epic 5: Release Validation For `lens.core.src`

Goal: create a release-quality validation gate for the Lens module source tree before Dev completion.

Why this closes the bundle: the architecture requires Module Builder validation, command contract tests, and local validation before release or publication consumption.

Stories:

| Story ID | Title | Outcome |
| --- | --- | --- |
| LSI-010 | Release validation or VM-style module validation for `lens.core.src` | A local validation command or VM-style checklist verifies module structure, help quality, command contracts, tests, and waiver closure. |

Definition of done:

- Validation checks for missing module files, orphan help entries, duplicate menu codes, broken references, inaccurate descriptions, missing capabilities, oversized SKILL bodies, and weak help entries.
- Parser, resolver, doctor, projection, Salmon, promotion, lifecycle, and registration tests are run together through a documented local command or VM-style validation flow.
- Validation fails if `feature:lens-seed-improvements` still has unresolved `belongs_to: unknown` without a verified pilot `service:lens-workbench` ledger relationship or a complete reviewed exception.
- The final release package stays inside `lens.core.src`; no implementation edits are made in the control repo, governance repo, or release clone.

## Story Dependency Order

1. LSI-001 has no story dependency and creates the fixture and schema foundation.
2. LSI-002 depends on LSI-001.
3. LSI-003 depends on LSI-001 and LSI-002.
4. LSI-004 depends on LSI-003.
5. LSI-005 depends on LSI-003 and LSI-004.
6. LSI-006 depends on LSI-001, LSI-002, LSI-003, and LSI-004.
7. LSI-007 depends on LSI-003, LSI-005, and LSI-006.
8. LSI-008 depends on LSI-005, LSI-006, and LSI-007.
9. LSI-009 depends on LSI-003, LSI-004, LSI-005, and LSI-007.
10. LSI-010 depends on LSI-001 through LSI-009.

## Requirements Coverage Map

| Source requirement theme | Covered by |
| --- | --- |
| Stable IDs, entity types, source metadata, permanent feature paths | LSI-001, LSI-002, LSI-009 |
| Shared inventory and frontmatter parsing | LSI-001, LSI-002 |
| Parent resolution, relationship graphs, waiver validation | LSI-003 |
| Phase-aware topology doctor severity | LSI-004 |
| Projection rebuild check, write, explain, JSON, draft-inclusive modes | LSI-005 |
| Advisory and material Salmon, state transitions, deterministic clustering | LSI-006 |
| Ledger promotion provenance and pilot `service:lens-workbench` verification | LSI-007 |
| Module Builder registration, module help, prompts, setup anti-zombie behavior | LSI-008 |
| Two-tree lifecycle compatibility and legacy read behavior | LSI-009 |
| Release validation or VM-style module validation for `lens.core.src` | LSI-010 |
