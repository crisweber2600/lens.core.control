---
feature: nextlens-src-bottomup
doc_type: stories
status: approved
phase: finalizeplan
track: full
updated_at: 2026-05-20T00:00:00Z
inputDocuments:
  - architecture.md
  - architecture.md
  - brainstorm.md
  - prd.md
  - product-brief.md
  - research.md
  - ux-design.md
supportingReviewContext:
  - preplan-adversarial-review.md
  - businessplan-adversarial-review.md
  - techplan-adversarial-review.md
  - finalizeplan-review.md
storyIdPattern: E{epicNumber}-S{storyNumber}
---

# Story Queue - NextLens Bottom-Up LENS

## Implementation Target and Write Boundary

Feature: `nextlens-src-bottomup`.

Dev write target: `NextLens` at `TargetProjects/nextlens/src/NextLens`.

Planning artifacts write scope: `docs/nextlens/src/nextlens-src-bottomup` only.

Story-level implementation boundary: every story below must be implemented only in the `TargetProjects/nextlens/src/NextLens` target repository for this feature. Runtime module writes are allowed only under configured Bottom-Up LENS `packet_output_path` and `reports_output_path` roots.

Forbidden implementation and runtime surfaces for every story: no `feature.yaml`, no governance publish, no direct governance repo or governance docs mirror writes, no Lens branch topology, no Lens constitution runtime, no release clone, no `.github`, no current NextLens top-down runtime dependency, no Lens control-repo implementation writes, no Landscape, no Derived Graph, no Salmon, no promotion, no adjacency, no pressure, no roadmap, no service/domain/program truth paths, and no writes outside configured output roots.

## Story Queue Summary

| Story ID | Title | Depends on | Primary sources | Carry-forward refs |
|---|---|---|---|---|
| E1-S1 | Scaffold Standalone Bottom-Up LENS Module Skeleton | none | architecture.md | finalize H1/M2, techplan M3 |
| E1-S2 | Register Setup Configuration and Help Discovery | E1-S1 | architecture.md, ux-design.md | business M2, finalize H1/M2 |
| E1-S3 | Add Module Validation and Trigger Eval Placeholders | E1-S1, E1-S2 | architecture.md | techplan M2, finalize H2/M2 |
| E1-S4 | Complete Marketplace and Package Metadata Decisions | E1-S1 | architecture.md, finalizeplan-review.md | techplan L2, finalize L1 |
| E2-S1 | Lock Handwritten Python Packet Validation Contract | E1-S1 | architecture.md, research.md | techplan M1, finalize M1 |
| E2-S2 | Implement Packet Schema Fixtures and Read-Only Validator | E2-S1 | prd.md, architecture.md, research.md | preplan M2, finalize M1/M2 |
| E2-S3 | Implement BMAD Readiness as a Separate Result | E2-S2 | prd.md, ux-design.md, research.md | preplan H3, business M3 |
| E2-S4 | Implement Path Guard and Forbidden-Write Fixtures | E1-S2, E2-S1 | architecture.md, prd.md | preplan H1/M4, business M1, finalize M2/M3 |
| E2-S5 | Implement Receipt and Run-Metadata Verification | E2-S4 | prd.md, architecture.md, research.md | business M1, techplan challenge, finalize M3 |
| E3-S1 | Implement Create Packet Skill Routing and Stage Prompts | E1-S1, E1-S2 | ux-design.md, architecture.md | finalize H1/M2 |
| E3-S2 | Compose Candidate, Sufficiency, and Scope Drafts | E3-S1, E2-S2, E2-S3 | brainstorm.md, prd.md, ux-design.md | preplan H1/H2, business M3 |
| E3-S3 | Render Preview, Dry Run, and Confirmation Token | E3-S2, E2-S4 | prd.md, ux-design.md, architecture.md | finalize L2/M2 |
| E3-S4 | Write Packet Atomically and Emit Verified Receipt | E3-S3, E2-S5 | prd.md, architecture.md | business M1, techplan QA, finalize M3 |
| E3-S5 | Handle Duplicate, Revise, Cancel, and Interrupted Runs Safely | E3-S4 | prd.md, ux-design.md, architecture.md | business M1/M2, finalize M2 |
| E4-S1 | Write Standalone Module README and Command Documentation | E1-S1, E3-S3, E2-S5 | product-brief.md, ux-design.md, architecture.md | techplan Paige, finalize party-mode |
| E4-S2 | Maintain Golden Valid and Invalid Examples | E2-S2, E2-S3, E2-S5, E3-S4 | prd.md, research.md, architecture.md | preplan M4, business M3, finalize M3 |
| E4-S3 | Add Artifact and Trigger Evals for Module Behavior | E1-S3, E4-S2 | architecture.md, finalizeplan-review.md | techplan gap 1, finalize gap 3 |
| E4-S4 | Finalize Read-Only State Contracts and Package Validation | E1-S4, E4-S1, E4-S2, E4-S3 | prd.md, architecture.md, finalizeplan-review.md | finalize M4/M5, techplan L2 |

## Global Validation Expectations

Every completed story must include or preserve these validation expectations:

- Unit tests for deterministic Python scripts changed by the story.
- Artifact or trigger eval updates when skill invocation or generated artifacts change.
- Negative checks proving no Lens governance, Lens branch topology, Lens constitution runtime, release clone, current NextLens top-down runtime, Landscape, Graph, Salmon, promotion, adjacency, or pressure side effect was added.
- Path containment checks for write-capable scripts using normalized `pathlib` resolution.
- Fixture coverage for the story's acceptance criteria when the behavior is deterministic.

## Stories

### E1-S1: Scaffold Standalone Bottom-Up LENS Module Skeleton

Epic: E1 - Installable Standalone Module Boundary.

Depends on: none.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `architecture.md` Starter Template and Module Packaging Evaluation, First Implementation Story, Complete Project Directory Structure; `techplan-adversarial-review.md` M3; `finalizeplan-review.md` H1 and M2.

Implementation scope:

- Create the initial standalone BMad module folder shape in the `NextLens` repository.
- Add `.claude-plugin/marketplace.json`, `skills/bul-setup`, `skills/bul-create-packet`, `skills/bul-validate-packet`, `skills/bul-verify-receipt`, `evals`, `README.md`, and `LICENSE` placeholders or initial files.
- Seed skill `SKILL.md` files with concise purpose, activation boundaries, inputs/outputs, and explicit non-Lens boundary language.

Acceptance criteria:

- Given the dev agent starts implementation, when it creates files, then every implementation file is under `TargetProjects/nextlens/src/NextLens`.
- Given the skeleton is inspected, when maintainers list the module, then the BMad Builder module structure exists with setup, create, validate, verify, eval, README, and license surfaces.
- Given dependency and import checks run, when they scan the skeleton, then no story file, skill, script, or manifest depends on `feature.yaml`, governance publish, Lens branch topology, Lens constitution runtime, release clone paths, or current NextLens top-down runtime modules.
- Given the README seed is read, when it describes the module, then it says Bottom-Up LENS is a standalone BMad module for safe packet creation, not Lens governance behavior.

Test/validation expectations:

- Directory-structure assertion for required folders/files.
- Text/import scan proving forbidden Lens/governance/runtime references are absent except in explicit negative-boundary documentation.
- Module validation placeholder exists and documents the future validation route.

### E1-S2: Register Setup Configuration and Help Discovery

Epic: E1 - Installable Standalone Module Boundary.

Depends on: E1-S1.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime setup may write only BMad setup-owned config/help files in a consuming project. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `architecture.md` `bul-setup`, Module Configuration Defaults, API and Command Surface; `ux-design.md` Design System Foundation; `businessplan-adversarial-review.md` M2; `finalizeplan-review.md` H1 and M2.

Implementation scope:

- Implement setup assets for `module.yaml` and `module-help.csv`.
- Implement setup merge scripts for module config and help registration following BMad setup conventions.
- Register `packet_output_path`, `reports_output_path`, and `default_packet_schema_version`.

Acceptance criteria:

- Given `bul-setup` configures a project, when it merges module settings, then `packet_output_path`, `reports_output_path`, and `default_packet_schema_version` are registered with defaults from architecture.
- Given help discovery loads module help, when actions are listed, then `configure`, `create`, `validate`, and `verify` are present with clear arguments and output-location keys.
- Given setup scripts run repeatedly, when they merge help/config entries, then output is idempotent and avoids zombie duplicate help rows.
- Given setup output is inspected, when negative boundary checks run, then it does not write Lens governance files, release clones, Lens lifecycle metadata, Lens constitution runtime state, or current NextLens top-down runtime files.

Test/validation expectations:

- Unit tests for config merge and help CSV merge behavior.
- Idempotency fixture for repeat setup.
- Forbidden path fixture for setup refusing non-setup output paths.

### E1-S3: Add Module Validation and Trigger Eval Placeholders

Epic: E1 - Installable Standalone Module Boundary.

Depends on: E1-S1, E1-S2.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `architecture.md` Architecture Validation Results, Eval coverage, Development Workflow Integration; `techplan-adversarial-review.md` M2 and gap 1; `finalizeplan-review.md` H2, M2, and gap 3.

Implementation scope:

- Add placeholders for BMad Module Validate or the equivalent module validation route.
- Add eval skeletons for artifact behavior and trigger behavior.
- Include positive Bottom-Up LENS trigger examples and negative Lens lifecycle trigger examples.

Acceptance criteria:

- Given module validation placeholders are inspected, when a maintainer opens release instructions, then the required validation route is named and cannot be skipped for distribution readiness.
- Given trigger eval placeholders are inspected, when prompt fixtures are reviewed, then Bottom-Up LENS prompts activate `bul-*` skills and Lens lifecycle prompts do not.
- Given eval skeletons are reviewed, when negative boundary cases are inspected, then they assert no Lens governance/runtime dependency is required.
- Given the module is not yet fully implemented, when placeholders run or are listed, then they fail/pending clearly rather than silently passing release readiness.

Test/validation expectations:

- Placeholder eval files are parseable by the chosen eval runner.
- Negative trigger fixture list includes Lens feature initialization, governance publish, constitution, and branch topology prompts.
- Validation docs identify required future pass conditions.

### E1-S4: Complete Marketplace and Package Metadata Decisions

Epic: E1 - Installable Standalone Module Boundary.

Depends on: E1-S1.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `architecture.md` Proposed Module Identity, Infrastructure and Distribution Decisions, Gap Analysis Results; `techplan-adversarial-review.md` L2; `finalizeplan-review.md` L1.

Implementation scope:

- Complete package metadata for `.claude-plugin/marketplace.json` and related release docs.
- Record owner, repository, homepage, license, keywords, module name `Bottom-Up LENS`, module code `bul`, and initial version.
- Ensure metadata does not imply Lens governance installation.

Acceptance criteria:

- Given package metadata is inspected, when required fields are checked, then owner, repository, homepage, license, keywords, name, code, and version are explicit.
- Given marketplace metadata is validated, when required fields are missing, then validation fails with actionable guidance.
- Given users read distribution metadata, when they interpret module purpose, then it is clear this is a standalone BMad module that creates/verifies packets, not a Lens governance lane.
- Given dependency checks scan package metadata, when they run, then no current NextLens top-down runtime module is required.

Test/validation expectations:

- Metadata schema or lint check for marketplace fields.
- Negative text scan for misleading governance/runtime install claims.
- License file existence check.

### E2-S1: Lock Handwritten Python Packet Validation Contract

Epic: E2 - Read-Only Validation and Non-Effects Proof.

Depends on: E1-S1.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime validation is read-only unless `--report` writes under configured `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `architecture.md` Technology and Dependency Baseline, Validation Architecture, Gap Analysis Results; `research.md` JSON Schema grounding and Packet Validity Gate; `techplan-adversarial-review.md` M1; `finalizeplan-review.md` M1.

Implementation scope:

- Document and stub the MVP validation mechanism as handwritten Python rules.
- Avoid adding `jsonschema` dependency in MVP.
- Define rule categories and structured error shape before dependent schema fixtures or create scripts.

Acceptance criteria:

- Given validator design is reviewed, when dependency declarations are inspected, then MVP packet validation uses Python standard library first and has no `jsonschema` dependency.
- Given rule categories are listed, when maintainers inspect the contract, then schema version, source mode, selected feature, scope, constraints, assumptions, provenance, receipt reference, topology, and non-effects requirements are covered.
- Given downstream work attempts to write schema tests or create workflow logic, when dependencies are checked, then this story is required first.
- Given future schema interoperability is discussed, when docs are read, then `jsonschema` is listed only as post-MVP or later enhancement after handwritten validation is stable.

Test/validation expectations:

- Dependency check confirming no `jsonschema` import or script dependency.
- Validator rule inventory test or snapshot.
- Structured error-shape unit test.

### E2-S2: Implement Packet Schema Fixtures and Read-Only Validator

Epic: E2 - Read-Only Validation and Non-Effects Proof.

Depends on: E2-S1.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime validation is read-only unless optional reports write under configured `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR33-FR42 and Testability; `architecture.md` Packet Artifact, Validation Architecture, Validation Result Shape; `research.md` Minimum Packet Fields and Packet Validity Gate; `preplan-adversarial-review.md` M2; `finalizeplan-review.md` M1 and M2.

Implementation scope:

- Implement `bul-validate-packet/scripts/validate_packet.py` using the handwritten contract.
- Add valid and invalid packet fixtures.
- Return script result JSON with `packetValid`, `bmadReady`, `hardBlockers`, and `advisories` where appropriate.

Acceptance criteria:

- Given a valid packet fixture, when validation runs, then it returns `packetValid.status=pass`, preserves topology null/unpromoted, and writes nothing.
- Given invalid fixtures for multi-candidate, missing included scope, missing explicit out-of-scope, missing non-inference rules, promoted assumptions, invalid source mode, unsupported schema version, or branch/editor/cwd inference, when validation runs, then it returns `status=fail` with structured errors.
- Given validation runs with `--report`, when a report is requested, then any report is written only under `reports_output_path` and never modifies the packet under inspection.
- Given repository state is compared before and after read-only validation, when validation completes, then no forbidden file changed.

Test/validation expectations:

- Unit tests for every validator rule and structured error.
- Golden fixture tests for valid, invalid multi-candidate, missing out-of-scope, unsupported schema version, and promoted topology.
- No-mutation test around validation and optional report behavior.

### E2-S3: Implement BMAD Readiness as a Separate Result

Epic: E2 - Read-Only Validation and Non-Effects Proof.

Depends on: E2-S2.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime readiness checks are read-only unless optional reports write under configured `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR37-FR39 and NFR separation requirements; `ux-design.md` status label and copy requirements; `research.md` Finding 2 and BMAD Readiness Gate; `preplan-adversarial-review.md` H3; `businessplan-adversarial-review.md` M3.

Implementation scope:

- Implement `readiness_check.py` or equivalent in the validate skill.
- Check acceptance criteria quality, constraints, actor clarity, provenance sufficiency, and non-inference handoff instructions.
- Preserve packet validity as independent from readiness.

Acceptance criteria:

- Given a packet satisfies packet validity but lacks BMAD handoff quality, when readiness runs, then output shows `packetValid.status=pass` and `bmadReady.status=fail` with reasons.
- Given human-readable output renders status, when operators read it, then it uses `Feature packet is valid`, `Feature packet is not ready yet`, `Ready for BMAD: not yet`, or `Ready for BMAD: ready` exactly.
- Given readiness fails, when validation completes, then it does not invalidate packet capture solely because BMAD is not ready and does not trigger BMAD execution.
- Given reporting/handoff consumers inspect JSON, when they read output, then validity and readiness are separate fields.

Test/validation expectations:

- Fixture proving `packetValid=pass` with `bmadReady=fail`.
- Fixture proving both pass when handoff context is sufficient.
- Snapshot tests for status labels and reasons.

### E2-S4: Implement Path Guard and Forbidden-Write Fixtures

Epic: E2 - Read-Only Validation and Non-Effects Proof.

Depends on: E1-S2, E2-S1.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime writes allowed only under configured `packet_output_path` and `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `architecture.md` Security and Write Boundary Decisions, Path Guard Pattern, Write Boundaries; `prd.md` FR28-FR31 and Security requirements; `preplan-adversarial-review.md` H1 and M4; `businessplan-adversarial-review.md` M1; `finalizeplan-review.md` M2 and M3.

Implementation scope:

- Implement a reusable path guard script/helper local to write-capable skills.
- Resolve paths with `Path(...).expanduser().resolve()` before containment checks.
- Add forbidden-write fixtures for denied categories.

Acceptance criteria:

- Given a path under configured output roots, when guard checks containment, then it passes and returns normalized path metadata.
- Given a path outside configured roots or containing denied categories, when guard checks it, then it fails with category, field, message, and recommendation.
- Given a write-capable create story runs, when it plans writes, then it calls this guard before any file write.
- Given forbidden-write fixtures target governance, release, landscape, graph, salmon, promotion, adjacency, pressure, `.git`, Lens lifecycle metadata, or current top-down runtime paths, when tests run, then every fixture is blocked.

Test/validation expectations:

- Unit tests for containment, traversal, symlink/resolution behavior as feasible, and denied path categories.
- Forbidden-write fixture tests used by create workflow.
- Negative acceptance test proving no ad hoc string-only path validation is used where the guard should be called.

### E2-S5: Implement Receipt and Run-Metadata Verification

Epic: E2 - Read-Only Validation and Non-Effects Proof.

Depends on: E2-S4.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime verification is read-only unless optional reports write under configured `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR43-FR48 and Testability; `architecture.md` Receipt Artifact, Run Metadata, Receipt verification, Read-Only Skill Pattern; `research.md` Non-Effects Contract; `businessplan-adversarial-review.md` M1; `techplan-adversarial-review.md` party-mode QA challenge; `finalizeplan-review.md` M3.

Implementation scope:

- Implement `bul-verify-receipt/scripts/verify_receipt.py`.
- Verify receipt claims against run metadata, `writtenFiles`, and `changedFiles`.
- Add false-receipt and forbidden-changed-file fixtures before create can claim accepted success.

Acceptance criteria:

- Given a valid receipt and matching run metadata, when verification runs, then it returns `status=pass` and `Non-effects verified` with checked-file summary.
- Given a receipt claims no Graph write but run metadata includes a graph changed file, when verification runs, then it returns `status=fail`, `Receipt mismatch detected`, violated category, and changed-file evidence.
- Given required metadata is missing, when verification runs, then it fails closed and writes no accepted verification result outside optional report scope.
- Given the full create workflow is implemented later, when receipt verification is unavailable or failing, then create cannot claim accepted-packet success.

Test/validation expectations:

- Unit tests for valid receipt, false receipt, missing metadata, forbidden changed file, and optional report path guard.
- Fixture for `false-graph-receipt.json` and a forbidden-write changed-file manifest.
- CI or repeatable command expectation for verification tests.

### E3-S1: Implement Create Packet Skill Routing and Stage Prompts

Epic: E3 - Confirmed Packet Creation Workflow.

Depends on: E1-S1, E1-S2.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime create writes only under configured `packet_output_path` and `reports_output_path` after validation and confirmation. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `ux-design.md` Experience Mechanics and stage patterns; `architecture.md` `bul-create-packet`, Prompt Stage Labels, API and Command Surface; `prd.md` FR1-FR5; `finalizeplan-review.md` H1 and M2.

Implementation scope:

- Implement `bul-create-packet/SKILL.md` routing to guided/headless prompt instructions.
- Define stable stage prompt files/resources.
- Display explicit context/output/write scope before mutation.

Acceptance criteria:

- Given a user invokes "Start from one feature", when the skill activates, then the create workflow begins and shows context/output/write scope before asking for raw context.
- Given the workflow stages render, when tests inspect stage labels, then they are exactly `context-intake`, `candidate-selection`, `local-sufficiency`, `scope-boundary`, `preview`, `confirmation`, `write`, and `receipt`.
- Given context resolution would rely on branch name, open editor file, or cwd, when the workflow starts, then it blocks and asks for explicit input or module config instead.
- Given Lens lifecycle prompts are evaluated, when trigger evals run, then this standalone module does not activate for governance/constitution/branch topology work.

Test/validation expectations:

- Trigger evals for positive create prompts and negative Lens lifecycle prompts.
- Unit or prompt tests for stage labels and context display.
- No-write test for startup/context blockers.

### E3-S2: Compose Candidate, Sufficiency, and Scope Drafts

Epic: E3 - Confirmed Packet Creation Workflow.

Depends on: E3-S1, E2-S2, E2-S3.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime draft/cache writes, if any, only under configured allowed roots and must not be accepted packet output until confirmation and receipt verification. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `brainstorm.md` Minimum Viable Restraint Loop and hard constraints; `prd.md` FR6-FR20; `ux-design.md` Candidate Selection List, Sufficiency Prompt Block, Scope Boundary Block; `preplan-adversarial-review.md` H1/H2; `businessplan-adversarial-review.md` M3.

Implementation scope:

- Extract candidate feature slices from raw context for operator confirmation.
- Enforce exactly-one selected candidate and preserve deferred candidates as unranked notes.
- Collect actor/user, problem, local outcome, acceptance criteria, constraints, assumptions, included scope, and explicit out-of-scope.

Acceptance criteria:

- Given raw context contains one or more possible features, when candidates are presented, then choices are unranked and do not imply roadmap, dependency order, domain, service, or capability grouping.
- Given zero or multiple selected candidates remain, when the user tries to proceed, then the workflow blocks, explains the protective reason, and writes no packet.
- Given included scope or explicit out-of-scope is missing or generic, when validation runs, then the workflow says the packet is not ready yet, names the smallest corrective action, and writes nothing.
- Given a draft is complete, when it is validated, then raw wording and normalized fields are present, assumptions are unpromoted, and topology remains null/unpromoted.

Test/validation expectations:

- Prompt/eval fixtures for single feature, multi-candidate, missing out-of-scope, and valid-not-BMAD-ready drafts.
- Validator integration tests from E2-S2/E2-S3.
- No-write tests for all blocker paths.

### E3-S3: Render Preview, Dry Run, and Confirmation Token

Epic: E3 - Confirmed Packet Creation Workflow.

Depends on: E3-S2, E2-S4.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime dry-run/preview writes nothing unless optional reports are explicitly requested under configured `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR21-FR26 and FR31; `ux-design.md` Packet Preview Panel, Preview Patterns, Confirmation Prompt; `architecture.md` Confirmation Pattern; `finalizeplan-review.md` L2 and M2.

Implementation scope:

- Render preview with packet summary, validity, readiness, write target, and non-effects.
- Support revise, cancel, and dry-run without packet write.
- Require exact interactive token `CREATE PACKET` and headless `--confirm` for writes.

Acceptance criteria:

- Given a valid draft reaches preview, when preview renders, then it shows selected feature, included scope, explicit out-of-scope, assumptions, acceptance criteria, constraints, provenance, write target, `packetValid`, `bmadReady`, and non-effects checklist.
- Given the user chooses revise or cancel, when the flow exits or returns to earlier stages, then no packet, receipt, run metadata, governance file, release clone, or top-down runtime file is written.
- Given the user presses Enter or supplies any token other than `CREATE PACKET`, when final confirmation is requested, then no write occurs.
- Given headless mode is used, when `--confirm` is absent, then the create workflow blocks before write.

Test/validation expectations:

- Snapshot tests for preview content and status labels.
- Confirmation-token tests for exact `CREATE PACKET` behavior.
- Dry-run/revise/cancel no-write tests.

### E3-S4: Write Packet Atomically and Emit Verified Receipt

Epic: E3 - Confirmed Packet Creation Workflow.

Depends on: E3-S3, E2-S5.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime create writes only packet JSON, run metadata JSON, non-effects receipt JSON, and allowed reports under configured output roots after guard, validation, confirmation, and verifier pass. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR27-FR30 and FR43-FR45; `architecture.md` Fail-Closed Write Pattern, Receipt Artifact, Run Metadata; `businessplan-adversarial-review.md` M1; `techplan-adversarial-review.md` QA challenge; `finalizeplan-review.md` M3.

Implementation scope:

- Compose canonical packet JSON after confirmation.
- Capture pre/post write manifests.
- Write through temp files plus atomic replace where practical.
- Build receipt from observed writes and run metadata, then verify it before claiming accepted success.

Acceptance criteria:

- Given all hard gates pass and confirmation is explicit, when create writes, then packet JSON, run metadata JSON, and non-effects receipt JSON are written only under configured allowed roots.
- Given post-write manifest capture completes, when receipt is built, then receipt `writtenFiles`, `changedFiles`, `forbiddenPathChecks`, and `nonEffects` reflect observed state.
- Given receipt verification fails, when create returns, then it does not claim accepted-packet success and marks the run invalid.
- Given any hard validation, path guard, confirmation, write, or verification step fails, when the flow stops, then it writes no accepted packet result and reports rollback guidance if temporary files remain.

Test/validation expectations:

- Unit tests for compose, write, manifest capture, receipt build, and verifier integration.
- Integration fixture for happy path writing only allowed outputs.
- False-receipt and forbidden-changed-file tests from E2-S5 must pass before this story is accepted.

### E3-S5: Handle Duplicate, Revise, Cancel, and Interrupted Runs Safely

Epic: E3 - Confirmed Packet Creation Workflow.

Depends on: E3-S4.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Runtime duplicate/resume state writes, if any, only under configured output roots and must not mutate existing packets without explicit operator resolution. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR25, FR30, FR32 and NFR reliability; `ux-design.md` Preview Edit Loop and Navigation Patterns; `architecture.md` Compaction survival and fail-closed behavior; `businessplan-adversarial-review.md` M1/M2; `finalizeplan-review.md` M2.

Implementation scope:

- Detect duplicate packet attempts for the same candidate or packet ID.
- Require explicit operator resolution before overwrite/new packet decisions.
- Preserve safe draft/run-state cache behavior without creating accepted packet output.

Acceptance criteria:

- Given a packet already exists for a selected candidate, when create is invoked again, then duplicate detection blocks any write until explicit operator resolution.
- Given the user cancels, revises, or leaves a blocker unresolved, when the workflow exits, then it states what was not written and leaves no accepted packet result.
- Given draft/run-state cache artifacts are used, when they are inspected, then they are under configured allowed roots, contain no secrets, and are not treated as packet validity or topology truth.
- Given an interrupted run resumes, when saved state is loaded, then the workflow resumes at the last safe stage and revalidates before any write.

Test/validation expectations:

- Duplicate fixture tests for same candidate and same packet ID.
- Cancel/revise/interrupted no-accepted-packet tests.
- Secrets/environment-dump scan for run-state artifacts.

### E4-S1: Write Standalone Module README and Command Documentation

Epic: E4 - Examples, Evals, and Release Readiness.

Depends on: E1-S1, E3-S3, E2-S5.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Documentation must not instruct users to write Lens governance, release clones, control-repo implementation surfaces, current NextLens top-down runtime, Landscape, Graph, Salmon, promotion, adjacency, or pressure outputs. Forbidden implementation surfaces remain unchanged.

Source/carry-forward references: `product-brief.md` Core Value Proposition and Key Constraints; `ux-design.md` status/copy/accessibility requirements; `architecture.md` Infrastructure and Distribution Decisions; `techplan-adversarial-review.md` Paige challenge; `finalizeplan-review.md` party-mode challenge.

Implementation scope:

- Write README and command docs for setup/create/validate/verify.
- Explain "Start from one feature", packet validity, BMAD readiness, non-effects, and output paths.
- State the standalone BMad module boundary prominently.

Acceptance criteria:

- Given a new operator reads README, when they follow module docs, then they understand setup, create, validate, verify, packet output, report output, and receipt verification.
- Given docs mention Bottom-Up LENS, when they are read, then they preserve the phrase "Start from one feature" and explain Bottom-Up LENS after the plain-language concept.
- Given docs mention Lens, when they are read, then they explicitly state the module does not publish governance, create Lens branches, enforce Lens constitution runtime, write release clones, or depend on current NextLens top-down runtime modules.
- Given Markdown is rendered, when screen-reader/plain-text checks are considered, then headings, status labels, examples, and paths remain understandable without color.

Test/validation expectations:

- Documentation lint or snapshot for required headings and commands.
- Negative text scan for misleading governance/runtime claims.
- Example paths and status labels match validator/create output.

### E4-S2: Maintain Golden Valid and Invalid Examples

Epic: E4 - Examples, Evals, and Release Readiness.

Depends on: E2-S2, E2-S3, E2-S5, E3-S4.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Fixtures and examples must remain read-only inputs during validation/eval execution except generated reports under configured `reports_output_path`. Forbidden: `feature.yaml`, governance publish, governance repos/mirrors, Lens branch topology, Lens constitution runtime, release clones, `.github`, control-repo implementation paths, current NextLens top-down runtime dependencies, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, and service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR50-FR54 and Testability; `research.md` Minimum Packet Fields and Non-Effects Contract; `architecture.md` Test Organization and eval fixture layout; `preplan-adversarial-review.md` M4; `businessplan-adversarial-review.md` M3; `finalizeplan-review.md` M3.

Implementation scope:

- Add valid and invalid packet examples.
- Add false receipt and forbidden changed-file examples.
- Keep examples synchronized with validator/readiness/verifier behavior.

Acceptance criteria:

- Given examples are listed, when maintainers inspect fixtures, then they include valid packet, invalid multi-candidate, invalid missing explicit out-of-scope, forbidden write, false receipt, and valid-not-BMAD-ready cases.
- Given validator rules change, when fixture tests run, then outdated examples fail until updated.
- Given examples are read by users, when they compare valid and invalid cases, then they can see why packet validity differs from BMAD readiness.
- Given examples include topology fields, when they are inspected, then topology remains null/unpromoted and no Landscape/Graph truth is implied.

Test/validation expectations:

- Golden fixture tests for every listed case.
- Example-doc synchronization check against validator output keys.
- Artifact eval fixtures reuse these examples.

### E4-S3: Add Artifact and Trigger Evals for Module Behavior

Epic: E4 - Examples, Evals, and Release Readiness.

Depends on: E1-S3, E4-S2.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Eval execution must write only allowed eval outputs/reports and configured packet/report roots; it must not write Lens governance, release clones, control-repo planning artifacts, current top-down runtime files, Landscape, Graph, Salmon, promotion, adjacency, or pressure paths.

Source/carry-forward references: `architecture.md` Eval coverage and Test Organization; `finalizeplan-review.md` gap 3; `techplan-adversarial-review.md` gap 1.

Implementation scope:

- Add artifact evals for create, validate, and verify flows.
- Add trigger evals for positive Bottom-Up LENS requests and negative Lens lifecycle requests.
- Assert non-effects and no forbidden activation.

Acceptance criteria:

- Given artifact evals run on valid input, when create/validate/verify completes, then evals assert packet/receipt/run-metadata shapes and non-effects status.
- Given artifact evals run on invalid or false-receipt fixtures, when flows execute, then evals assert fail-closed behavior and no accepted packet success.
- Given trigger evals run on Bottom-Up LENS prompts, when skills activate, then expected `bul-*` skills are selected.
- Given trigger evals run on Lens lifecycle prompts, when routing is evaluated, then Bottom-Up LENS skills do not activate and do not import Lens runtime behavior.

Test/validation expectations:

- Evals for `bul-create-packet`, `bul-validate-packet`, and `bul-verify-receipt`.
- Trigger fixture set includes bottom-up and Lens lifecycle negative prompts.
- Eval result assertions include forbidden-write/no-Lens-boundary checks.

### E4-S4: Finalize Read-Only State Contracts and Package Validation

Epic: E4 - Examples, Evals, and Release Readiness.

Depends on: E1-S4, E4-S1, E4-S2, E4-S3.

Target/write boundary: write only under `TargetProjects/nextlens/src/NextLens`. Package validation and read-only consumers must not write `feature.yaml`, governance repos/mirrors, release clones, Lens branch topology, Lens constitution runtime, current NextLens top-down runtime modules, Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, or service/domain/program truth paths.

Source/carry-forward references: `prd.md` FR55-FR58 and Integration Boundaries; `architecture.md` Read-Only Reporting and Future Handoff Support, Architecture Completion & Handoff; `finalizeplan-review.md` M4 and M5; `techplan-adversarial-review.md` L2.

Implementation scope:

- Confirm package metadata, docs, fixtures, unit tests, evals, and module validation are ready.
- Preserve read-only packet state fields for future reporting/handoff consumers.
- Record full-track artifact equivalence for constitutional planning requirements.

Acceptance criteria:

- Given package validation runs, when the module is release-ready, then module validation, unit tests, fixture tests, and evals pass.
- Given future reporting or BMAD handoff reads packet state, when it consumes status, provenance, validity, and readiness fields, then it can do so without mutating packet state or promoting topology.
- Given constitutional traceability is reviewed, when full-track artifacts are mapped, then PRD/UX/product/research/brainstorm are treated as the business-plan equivalent and `architecture.md` is treated as the tech-plan equivalent.
- Given FinalizePlan readiness is assessed, when this story is complete, then the story bundle is ready for story-file generation, but dev readiness is not claimed until required story files with frontmatter are produced.

Test/validation expectations:

- Full validation command or release checklist passes.
- Read-only consumer contract tests for status/provenance/validity/readiness fields.
- Traceability check for business-plan and tech-plan artifact equivalence.
