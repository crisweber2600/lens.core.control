---
feature: lens-seed-improvements
doc_type: stories
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

# Stories: Lens Seed Improvements

## Implementation Notes

All stories target `TargetProjects/lens-dev/new-codebase/lens.core.src`. Do not implement broad UI/dashboard work, do not migrate all historical features, do not hand-edit generated projection output as source truth, and do not write directly to governance feature folders.

Story IDs below are canonical for future story files and `sprint-status.yaml` entries. The intended story file slugs are the story IDs plus the lowercase title slug.

The accepted `belongs_to: unknown` waiver for `feature:lens-seed-improvements` is temporary. It permits planning to continue because the pilot ledger does not exist yet. Dev completion must verify that the pilot `service:lens-workbench` ledger exists and that the feature's parentage is resolved to that pilot ledger, or that a complete reviewed exception remains visible to lifecycle validation.

## LSI-001: Shared Metadata Schema And Golden Fixtures

Epic: Shared Metadata And Relationship Foundation

Goal: define the canonical two-tree metadata contract and golden fixtures used by all later parser, resolver, doctor, projection, Salmon, promotion, lifecycle, and validation stories.

Implementation scope:

- Update the Lens metadata schema asset in `lens.core.src` to cover source entities, waivers, Salmon signals, projection diagnostics, and candidate larger item reports.
- Add golden fixtures for one current feature, one pilot service ledger, Feature A/B/C Salmon rollup, duplicate IDs, invalid stable ID prefixes, broken parents, cycles, missing required fields, and waiver completeness.
- Keep fixture data deterministic and small enough for unit and CLI contract tests.

Dependencies: none.

Acceptance criteria:

1. Given a new two-tree feature fixture, when schema validation runs, then `stable_id`, `entity_type`, `feature_id`, `title`, `belongs_to`, `docs_path`, `status`, `phase`, `track`, `publication_state`, and `updated_at` are validated as source metadata.
2. Given a ledger fixture, when schema validation runs, then `stable_id`, `entity_type`, `title`, `belongs_to`, `ledger_path` or source path, linked `features`, `publication_state`, and optional owner or steward fields are validated.
3. Given `feature:lens-seed-improvements`, when fixture metadata is loaded, then it includes `belongs_to: unknown` and a complete temporary `topology_waiver` with owner, rationale, affected stable IDs, review point, impacted gate, status, and accepted timestamp.
4. Given Feature A, Feature B, and Feature C fixtures, when Salmon fixtures are loaded, then at least two signals share the same target stable ID and category so deterministic clustering can be tested without text similarity.
5. Given invalid fixture cases, when tests run, then duplicate stable IDs, invalid prefixes, missing required fields, broken parents, cycles, and incomplete waivers are all represented by named fixtures.
6. Given the fixture set is serialized twice, when tests compare normalized output, then record order and content are stable.

Validation: unit tests for schema fixtures and fixture serialization.

## LSI-002: Shared Inventory And Frontmatter Parser

Epic: Shared Metadata And Relationship Foundation

Goal: implement one reusable inventory and frontmatter parser for feature archives, standalone `feature.yaml`, and landscape ledger records.

Implementation scope:

- Create or extend a shared parser module under the Lens command script area in `lens.core.src` rather than duplicating parser logic in individual skills.
- Scan configured feature archive paths and landscape roots.
- Parse Markdown YAML frontmatter and standalone YAML metadata files.
- Normalize source paths relative to project root.
- Emit source entities, duplicate ID diagnostics, missing field diagnostics, and a source path index.

Dependencies: LSI-001.

Acceptance criteria:

1. Given a Markdown feature record with YAML frontmatter, when inventory scanning runs, then the parser emits one normalized entity keyed by `stable_id` with a project-relative source path.
2. Given a standalone `feature.yaml`, when inventory scanning runs, then the parser emits the same normalized entity shape as Markdown frontmatter.
3. Given a service ledger fixture, when the landscape root is scanned, then the parser emits a ledger entity without requiring the ledger to live under `docs/features/`.
4. Given two source records with the same `stable_id`, when parsing completes, then the inventory emits a duplicate stable ID diagnostic that includes both source paths.
5. Given a source record with missing required fields, when parsing completes, then diagnostics include finding code, severity candidate, source path, stable ID if available, and recommendation.
6. Given legacy topology files are present, when compatibility read mode is enabled, then they may be indexed for context without becoming the write pattern for new two-tree records.

Validation: parser unit tests using the golden fixtures from LSI-001.

## LSI-003: Relationship Resolver And Topology Waiver Validation

Epic: Shared Metadata And Relationship Foundation

Goal: resolve two-tree relationships and validate `belongs_to: unknown` waiver metadata through a shared resolver used by doctor, projection, Salmon, promotion, and lifecycle checks.

Implementation scope:

- Resolve `belongs_to` parent edges, child edges, feature-to-ledger membership, `related_to`, `depends_on`, and Salmon reference edges.
- Validate allowed parent types for feature, service, domain, and program entities.
- Detect broken parents, wrong parent types, cycles, unresolved references, and waiver completeness.
- Treat `belongs_to: unknown` as a known debt state, not a parser failure.

Dependencies: LSI-001, LSI-002.

Acceptance criteria:

1. Given a feature whose `belongs_to` references `service:lens-workbench`, when resolver runs, then parent, child, and membership edges are emitted with source evidence.
2. Given `belongs_to: unknown`, when resolver runs, then it emits an unknown-parent finding and preserves waiver metadata for phase classification by doctor and lifecycle checks.
3. Given a complete `topology_waiver`, when waiver validation runs, then the waiver is accepted and includes code, owner, rationale, affected stable IDs, review point, impacted gate, and status.
4. Given an incomplete waiver, when validation runs, then an `invalid_waiver` diagnostic identifies missing fields and the affected stable IDs.
5. Given a parent cycle across ledger records, when resolver runs, then a `parent_cycle` diagnostic identifies the cycle path without infinite recursion.
6. Given `related_to`, `depends_on`, or Salmon target references, when resolver runs, then relationship edges are emitted separately from parent and membership edges.

Validation: resolver unit tests for parent resolution, unknown parent handling, waiver validation, cycle detection, and relationship edge output.

## LSI-004: Topology Doctor Phase-Aware Severity

Epic: Topology Doctor And Projection Rebuild

Goal: make `lens-doctor` consume the shared inventory and resolver and emit phase-aware findings without mutating source files.

Implementation scope:

- Wire doctor checks to shared parser/resolver output.
- Emit stable finding codes and severity values: `info`, `warning`, and `blocker`.
- Classify severity by lifecycle phase, publication state, and waiver completeness.
- Support machine-readable JSON and compact Markdown summaries for local runs, CI, and lifecycle gate reports.

Dependencies: LSI-003.

Acceptance criteria:

1. Given `belongs_to: unknown` during BusinessPlan, when doctor runs, then the finding is a warning and does not report blocked status.
2. Given `belongs_to: unknown` during TechPlan with an accepted resolution plan or waiver, when doctor runs, then the finding is a warning with recommendation to resolve or verify before Dev completion.
3. Given `belongs_to: unknown` during FinalizePlan without a complete waiver, when doctor runs, then the finding is a blocker.
4. Given Dev or Complete phase with unresolved `belongs_to: unknown`, when no verified pilot ledger relationship or reviewed exception exists, then doctor reports a blocker.
5. Given duplicate stable IDs, invalid stable ID prefixes, broken resolved parents, parent type mismatches, moved feature paths, projection drift, or material unchecked Salmon, when doctor runs, then finding codes and severities match the architecture severity policy.
6. Given doctor runs in any mode, then it does not write feature archives, ledgers, projection caches, governance metadata, or config files.

Validation: phase fixture tests for BusinessPlan, TechPlan, FinalizePlan, Dev, Complete, and published projection states; CLI contract tests for Markdown and JSON output.

## LSI-005: Projection Rebuild Check, Write, Explain, And JSON Modes

Epic: Topology Doctor And Projection Rebuild

Goal: implement projection rebuild as the only owner of generated `governance-map.json` and `governance-map.md` files, with deterministic check, write, explain, draft-inclusive, and JSON behavior.

Implementation scope:

- Implement or complete the projection engine in `lens.core.src` using the shared inventory, resolver, and doctor components.
- Pin default generated filenames to `governance-map.json` and `governance-map.md` under the provided `--reporting-output-path`.
- Require explicit output path arguments for local development and never write directly to governance as a fallback.
- Provide `--check`, `--write`, `--explain <stable_id>`, `--json`, and `--include-drafts` modes.

Dependencies: LSI-003, LSI-004.

Acceptance criteria:

1. Given stable source metadata and an explicit reporting output path, when projection rebuild runs twice, then generated JSON and Markdown content are deterministic except for an intentionally controlled generation timestamp field.
2. Given `--check`, when generated output differs from committed cache files, then the command reports projection drift and does not write files.
3. Given `--write` and no doctor blockers, when the command runs, then it writes only `governance-map.json` and `governance-map.md` under the configured reporting output path.
4. Given `--write` and doctor blockers, when no explicit force option is accepted, then the command refuses to write and reports blockers.
5. Given `--explain feature:lens-seed-improvements`, when the command runs, then output shows source path, source fields, parent edge or unknown parent state, child edges, ledger membership, related edges, Salmon edges, and diagnostics.
6. Given `--json`, when any projection mode runs, then machine-readable output includes module, report type, derived marker, source paths, entities, edges, diagnostics, and include-drafts state.
7. Given `--include-drafts`, when projection output is generated, then draft entities are included and the output clearly marks draft-inclusive status.

Validation: CLI contract tests for all projection modes and drift cases.

## LSI-006: Salmon Signal Schema, State Transitions, And Deterministic Cluster Report

Epic: Salmon Impact And Ledger Promotion

Goal: implement Salmon signal parsing, validation, materiality checks, legal state transitions, and deterministic candidate report generation using the Feature A/B/C fixture.

Implementation scope:

- Parse `salmon_upstream` entries from source metadata.
- Validate required signal fields, materiality, owner, rationale, affected IDs, review point, timestamps, and target stable IDs.
- Enforce legal state transitions: advisory to material, material to blocked, material to resolved, blocked to waived, blocked to resolved, and waived to resolved.
- Generate deterministic cluster and candidate larger item report data without building workbench UI.

Dependencies: LSI-001, LSI-002, LSI-003, LSI-004.

Acceptance criteria:

1. Given advisory Salmon signals, when parsed, then they do not block early planning by default.
2. Given a signal meets materiality criteria by contradicting published ledger truth, breaking identity or parent resolution, invalidating lifecycle assumptions, or exposing implementation-critical dependency, security, or API inconsistency, when classified, then it becomes material or blocked according to configured gate impact.
3. Given an illegal state transition, when validation runs, then the transition is rejected with a diagnostic that includes source path, signal ID, current status, requested status, and recommendation.
4. Given Feature A, Feature B, and Feature C fixtures with the same target stable ID and category across at least two features, when clustering runs, then one strong candidate report is produced deterministically.
5. Given signals share only weak text similarity or adjacency, when clustering runs, then the report may include an advisory suggestion but must not auto-promote or mark the candidate ready.
6. Given Salmon output is requested as JSON, when the report runs, then candidate records include candidate ID, title, source signals, source features, target ledger stable ID, cluster confidence, highest status, and promotion readiness.

Validation: unit tests for state transitions and materiality, plus CLI or report tests for Feature A/B/C deterministic clustering.

## LSI-007: Ledger Promotion Provenance And Pilot `service:lens-workbench` Verification

Epic: Salmon Impact And Ledger Promotion

Goal: require explicit provenance for ledger promotion and verify or create the pilot `service:lens-workbench` ledger relationship needed to close this feature's temporary topology waiver before Dev completion.

Implementation scope:

- Update ledger promotion behavior to require source feature stable IDs, source paths, signal IDs, target ledger stable ID and path, material Salmon review status, doctor status, projection preview or explain output, owner or steward acceptance, and promotion rationale.
- Add a pilot verification path for `service:lens-workbench` as the service-level ledger for this feature.
- Ensure promotion remains explicit and audited, never an automatic side effect of projection rebuild.

Dependencies: LSI-003, LSI-005, LSI-006.

Acceptance criteria:

1. Given a candidate promotion, when required provenance is missing, then promotion is blocked with a finding that names the missing source feature, signal, ledger, doctor, projection, owner, or rationale evidence.
2. Given material or blocked Salmon signals exist for a candidate, when promotion is reviewed, then promotion is disabled until each material issue is resolved or has a complete waiver.
3. Given promotion is accepted, when the ledger entry is created or updated through the approved boundary, then provenance records include `source_feature`, `source_signals`, `promotion_rationale`, reviewer or steward acceptance, and source paths.
4. Given Feature A/B/C fixtures, when promotion preview runs, then it shows the target ledger stable ID, linked features, signal IDs, and projection explain evidence without moving feature archive paths.
5. Given `feature:lens-seed-improvements` still has `belongs_to: unknown`, when Dev completion validation runs, then it fails unless the pilot `service:lens-workbench` ledger exists and is linked to the feature or a complete reviewed exception remains visible.
6. Given the pilot ledger exists, when resolver and doctor run, then `feature:lens-seed-improvements` can be attached or verified against `service:lens-workbench` without inventing a fake legacy domain or service placeholder.

Validation: promotion provenance tests, pilot ledger fixture tests, and Dev-completion waiver closure tests.

## LSI-008: `module.yaml`, `module-help.csv`, Prompt Registration, And Anti-Zombie Setup Validation

Epic: Module Registration And Two-Tree Lifecycle Compatibility

Goal: keep the Lens module discoverable and installable by updating module registration assets and anti-zombie setup behavior alongside changed command surfaces.

Implementation scope:

- Update `module.yaml`, `module-help.csv`, prompt stubs, setup assets, and relevant SKILL references for changed Lens commands.
- Ensure setup registration for the multi-skill Lens module removes stale Lens rows before inserting current rows.
- Keep SKILL frontmatter triggerable and concise; move detailed behavior into scripts, assets, references, or prompt stages.
- Add tests or validation checks for orphan help entries, duplicate menu codes, missing files, broken references, inaccurate descriptions, missing capabilities, and oversized SKILL bodies.

Dependencies: LSI-005, LSI-006, LSI-007.

Acceptance criteria:

1. Given changed Lens commands for doctor, projection, Salmon, promotion, topology, map audit, or reporting consumers, when module assets are inspected, then `module.yaml` and `module-help.csv` list current capabilities, args, outputs, and dependency order.
2. Given setup registration runs twice, when module help or prompt entries are merged, then old Lens rows are removed before current rows are inserted and no duplicate Lens entries remain.
3. Given a help entry references a skill or prompt that does not exist, when module validation runs, then it fails with an orphan reference diagnostic.
4. Given two help rows share a duplicate menu code, when validation runs, then it fails with both row identifiers.
5. Given SKILL files grow procedural details that belong in scripts, assets, references, or prompts, when validation runs, then it warns or fails according to the module validation policy.
6. Given command output contracts changed, when help is rendered, then it describes check, write, explain, JSON, Salmon report, promotion review, and validation outputs accurately.

Validation: module asset validation tests and setup anti-zombie idempotency tests.

## LSI-009: Compatibility And Lifecycle Validation For Two-Tree Features

Epic: Module Registration And Two-Tree Lifecycle Compatibility

Goal: update lifecycle validation so new two-tree features use stable metadata and waivers, while legacy topology remains readable for context but does not govern new writes.

Implementation scope:

- Validate `stable_id`, `entity_type`, `docs_path`, `target_repos`, `phase`, `track`, `publication_state`, `belongs_to`, and waiver metadata for two-tree features.
- Supersede legacy domain/service write requirements for new two-tree work without removing compatibility reads.
- Ensure branch names and folder locations are not treated as authoritative lifecycle status.
- Make waiver visibility available to lifecycle gates and Dev completion checks.

Dependencies: LSI-003, LSI-004, LSI-005, LSI-007.

Acceptance criteria:

1. Given a new two-tree feature under `docs/features/<feature_id>` with stable metadata, when lifecycle validation runs, then it does not require domain/service folder placement.
2. Given legacy topology artifacts exist, when compatibility discovery runs, then they can be read for context but are labeled as compatibility inputs rather than required write targets.
3. Given branch location differs from recorded phase metadata, when lifecycle validation runs, then recorded lifecycle metadata is used as authority and the branch mismatch is reported separately if needed.
4. Given `target_repos` is missing before Dev handoff, when lifecycle validation runs, then it reports a blocker for implementation readiness.
5. Given `belongs_to: unknown` has a complete temporary waiver, when FinalizePlan validation runs, then the waiver is visible and accepted only through its configured review point.
6. Given Dev completion is requested, when `belongs_to: unknown` remains unresolved and the pilot ledger relationship is not verified, then lifecycle validation reports a blocker.

Validation: lifecycle fixture tests for new two-tree records, legacy compatibility reads, branch/status separation, target repo readiness, and waiver expiry or review point enforcement.

## LSI-010: Release Validation Or VM-Style Module Validation For `lens.core.src`

Epic: Release Validation For `lens.core.src`

Goal: provide a local release validation flow for the Lens module source tree that verifies command contracts, module structure, tests, and temporary waiver closure before Dev completion.

Implementation scope:

- Add or document a local validation command or VM-style validation procedure for `lens.core.src`.
- Run parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, and module asset validations together.
- Validate that release artifacts stay inside `lens.core.src` and that generated projection output is produced only by approved commands.
- Fail validation when the pilot `service:lens-workbench` relationship or reviewed waiver exception is not present.

Dependencies: LSI-001, LSI-002, LSI-003, LSI-004, LSI-005, LSI-006, LSI-007, LSI-008, LSI-009.

Acceptance criteria:

1. Given a clean `lens.core.src` checkout, when release validation runs, then parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, and module validation checks are all invoked or explicitly reported as unavailable.
2. Given module assets are incomplete, when validation runs, then missing module files, orphan help entries, duplicate menu codes, broken references, inaccurate descriptions, missing capabilities, and weak entries fail or warn according to policy.
3. Given projection output is stale, when validation runs in check mode, then it reports drift and does not write generated cache files.
4. Given a SKILL body contains implementation detail that should live in scripts, assets, references, or prompt stages, when module validation runs, then the result identifies progressive disclosure risk.
5. Given `feature:lens-seed-improvements` still has `belongs_to: unknown`, when release validation runs, then validation fails unless `service:lens-workbench` is verified as the pilot ledger relationship or a complete reviewed exception remains active.
6. Given validation completes successfully, when Dev handoff is prepared, then the report names the commands run, fixtures covered, output paths checked, module assets checked, and any intentional deferrals such as full UI, broad historical migration, or always-blocking Salmon policy.

Validation: local release validation run or documented VM-style validation transcript for `lens.core.src`.

## Sprint Status Seed

Use this story order when generating `sprint-status.yaml` unless implementation planning discovers a lower-risk split:

1. LSI-001: ready for implementation.
2. LSI-002: blocked by LSI-001.
3. LSI-003: blocked by LSI-001 and LSI-002.
4. LSI-004: blocked by LSI-003.
5. LSI-005: blocked by LSI-003 and LSI-004.
6. LSI-006: blocked by LSI-001, LSI-002, LSI-003, and LSI-004.
7. LSI-007: blocked by LSI-003, LSI-005, and LSI-006.
8. LSI-008: blocked by LSI-005, LSI-006, and LSI-007.
9. LSI-009: blocked by LSI-003, LSI-004, LSI-005, and LSI-007.
10. LSI-010: blocked by all previous stories.
