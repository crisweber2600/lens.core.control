---
feature: nextlens-src-bottomup
doc_type: epics
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
stepsCompleted: [1, 2, 3, 4]
---

# NextLens Bottom-Up LENS - Epic Breakdown

## Overview

This document provides the FinalizePlan epic breakdown for `nextlens-src-bottomup`. It decomposes the approved full-track planning set into implementation-ready epics for a standalone Bottom-Up LENS BMad module.

Dev implementation target: `NextLens` at `TargetProjects/nextlens/src/NextLens`.

Implementation write boundary: dev stories may write only under `TargetProjects/nextlens/src/NextLens` for this feature's implementation work. They must not write Lens governance metadata, governance docs mirrors, release clones, control-repo planning artifacts, `.github`, Lens branch topology, Lens constitution runtime, existing Lens runtime surfaces, existing NextLens top-down runtime modules, Landscape, Derived Graph, Salmon, promotion, adjacency, pressure, roadmap, service/domain/program truth paths, or any path outside configured Bottom-Up LENS packet/report output roots at module runtime.

Planning write boundary: this bundle is authored only under `docs/nextlens/src/nextlens-src-bottomup`.

## Constitutional Traceability

The resolved constitution requires planning artifacts conceptually equivalent to `business-plan` and `tech-plan`, requires stories before dev, and enforces review.

| Constitutional requirement | Full-track artifact equivalence |
|---|---|
| `business-plan` planning requirement | `product-brief.md`, `research.md`, `brainstorm.md`, `prd.md`, and `ux-design.md` together define product scope, operator workflow, user outcomes, UX copy, non-effects promise, and business/user value. |
| `tech-plan` planning requirement | `architecture.md` defines the standalone BMad module packaging, scripts, data contracts, write boundaries, validation layers, fixtures, evals, and implementation target. |
| `stories` dev requirement | This `epics.md` plus `stories.md` define the implementable story queue. Follow-on story files are still required before Lens dev readiness is claimed. |

## Requirements Inventory

### Functional Requirements

FR1: Operators can start a bottom-up packet creation flow from a clear "Start from one feature" entry point.

FR2: Operators can provide raw feature context for packet creation.

FR3: The system can resolve required feature context from explicit input, session context, or module configuration without relying on Lens lifecycle behavior.

FR4: The system rejects context resolution attempts that rely on branch name, open file, or current working directory inference.

FR5: Operators can see resolved feature context, output path, and write scope before packet write.

FR6: The system can identify possible feature candidates from raw context for operator confirmation.

FR7: Operators can select exactly one candidate for packet creation.

FR8: The system blocks packet creation when no candidate is selected.

FR9: The system blocks packet creation when more than one selected candidate remains.

FR10: Operators can record deferred candidates as unranked notes that do not become roadmap or topology.

FR11: Operators can capture the selected feature's actor or user.

FR12: Operators can capture the selected feature's problem and locally useful outcome.

FR13: Operators can capture acceptance criteria for the selected feature.

FR14: Operators can capture known constraints.

FR15: Operators can capture assumptions as unpromoted.

FR16: Operators can capture included scope.

FR17: Operators can capture explicit out-of-scope.

FR18: The system blocks packet creation when included scope is missing.

FR19: The system blocks packet creation when explicit out-of-scope is missing.

FR20: The system explains fail-closed blocks in user-protective language.

FR21: Operators can preview the packet before it is written.

FR22: The preview can show packet validity status.

FR23: The preview can show the intended write target.

FR24: The preview can show the non-effects that packet creation will preserve.

FR25: Operators can revise packet inputs after preview without writing a packet.

FR26: Operators can provide an explicit confirmation action or token before packet creation.

FR27: The system writes a packet only after confirmation and passing validation.

FR28: The system writes the packet only to the approved archive/staging path.

FR29: The system prevents packet creation from writing to governance mirror, Landscape, Graph, or other forbidden paths in the MVP.

FR30: The system fails closed and writes nothing when hard validation constraints fail.

FR31: Operators can run preview or dry-run validation without writing a packet.

FR32: The system detects duplicate packet attempts for the same candidate and requires explicit operator resolution before any write.

FR33: The system can validate a packet without mutating state.

FR34: The system can enforce packet schema version and source mode.

FR35: The system can enforce required packet fields for identity, selected feature, scope, constraints, assumptions, provenance, receipt, and topology.

FR36: The system keeps topology fields null or unpromoted during MVP packet creation.

FR37: The system reports `packet_valid` status with validation reasons.

FR38: The system reports minimal `bmad_ready` status with reasons independently from packet validity.

FR39: Operators can see when a packet is valid but not BMAD-ready.

FR40: The system preserves raw user wording alongside normalized packet fields for auditability.

FR41: The system distinguishes intentionally unknown values from missing or empty required values during validation.

FR42: The system exposes packet lifecycle metadata states without treating them as Living Landscape truth.

FR43: The system emits a machine-readable non-effects receipt for each packet creation run.

FR44: The receipt states whether adjacency records, pressure detection, promotion candidates, Salmon signals, Landscape updates, and Graph updates were emitted.

FR45: The system records reproducible run metadata for receipt verification.

FR46: Operators or support users can verify a receipt against run metadata and changed files.

FR47: The system marks a run invalid when receipt claims conflict with observed changes.

FR48: The system blocks or fails runs that attempt forbidden side-effect writes.

FR49: Operators can access documentation that explains Bottom-Up LENS as "Start from one feature".

FR50: Operators can access a valid packet example.

FR51: Operators can access invalid packet examples for multi-candidate input and missing explicit out-of-scope.

FR52: Operators can access an example showing `packet_valid=true` and `bmad_ready=false`.

FR53: Downstream planning users can trace packet requirements to source inputs, provenance, selected-candidate rationale, and decision rationale.

FR54: Maintainers can preserve golden valid and invalid packet fixtures for schema and validator behavior.

FR55: Reporting consumers can read packet status, provenance, validity, and BMAD readiness.

FR56: Reporting consumers cannot mutate packet state or promote topology.

FR57: Future BMAD handoff can consume packet validity and BMAD readiness states.

FR58: Future promotion, adjacency, pressure, Salmon, Landscape, and Graph workflows can distinguish archive evidence from promoted truth.

### Non-Functional Requirements

NFR1: Packet validation is idempotent and never mutates state.

NFR2: Packet creation is atomic from the operator's perspective; failed validation or failed write leaves no accepted packet result.

NFR3: The module fails closed when any hard safety condition cannot be proven.

NFR4: Unsupported schema versions fail with actionable guidance.

NFR5: Validation distinguishes intentionally unknown values from missing or empty required values.

NFR6: Receipt verification detects mismatches between receipt claims and observed changed files.

NFR7: Packet validity and BMAD readiness are reproducible from saved packet and run metadata.

NFR8: Packet creation does not write governance mirrors, release clones, Landscape, Graph, Salmon, promotion, adjacency, pressure, or current Lens/NextLens top-down runtime surfaces.

NFR9: Run metadata and receipts do not expose secrets, credentials, tokens, or sensitive local environment values.

NFR10: Preview and validation remain fast enough for interactive command/prompt-native use.

NFR11: Receipt verification scales with changed-file manifests rather than repository history scans.

NFR12: User-facing messages are deterministic, keyboard-friendly, plain-language, and do not rely on color alone.

NFR13: UX copy distinguishes "Feature packet is valid" from "Ready for BMAD: not yet / ready".

NFR14: Automated tests cover happy path, fail-closed blockers, forbidden writes, false receipts, valid-not-BMAD-ready packets, and golden fixtures.

### Additional Requirements

AR1: Build a standalone BMad module, not a Lens lifecycle extension.

AR2: Package as a multi-skill module with `bul-setup`, `bul-create-packet`, `bul-validate-packet`, and `bul-verify-receipt`.

AR3: Place deterministic validation, path checks, writes, receipt generation, and receipt verification in tested Python 3.11+ scripts.

AR4: Use JSON as the canonical machine artifact for packets, receipts, and run metadata.

AR5: Use lower camelCase JSON fields and stable result shapes for scripts.

AR6: Use PEP 723 script dependencies only if standard library validation cannot satisfy the MVP; the first validator story locks handwritten Python validation.

AR7: Use explicit stage labels: `context-intake`, `candidate-selection`, `local-sufficiency`, `scope-boundary`, `preview`, `confirmation`, `write`, and `receipt`.

AR8: Enforce path containment with `pathlib.Path(...).expanduser().resolve()` and deny `.git`, governance, release, landscape, graph, salmon, promotion, adjacency, and pressure paths.

AR9: Require the confirmation token `CREATE PACKET` for interactive writes and an explicit `--confirm` flag for headless writes.

AR10: Keep helper code local or duplicated until module validation proves a supported shared-helper shape.

AR11: Include module validation and BMad evals before declaring the module distributable.

AR12: Fill marketplace metadata, license, owner, repository, homepage, and keywords before packaging validation.

### UX Design Requirements

UX-DR1: Use a command/prompt-native operator workflow design system, not a separate browser UI.

UX-DR2: Use "Start from one feature" as the first visible entry label before introducing Bottom-Up LENS terminology.

UX-DR3: Show current context, output path, and write scope before any mutation.

UX-DR4: Present candidate choices as unranked options and preserve deferred candidates only as notes.

UX-DR5: Collect one focused sufficiency or scope answer per prompt and distinguish missing, unknown, and deferred values.

UX-DR6: Use blocker messages with why blocked, what to do next, and what was not written.

UX-DR7: Provide preview/dry-run output with packet validity, BMAD readiness, will-write, and will-not-write sections.

UX-DR8: Require deliberate confirmation and allow revise, cancel, or confirm from preview.

UX-DR9: Show non-effects receipt summaries with checked files and mismatch details without requiring log inspection.

UX-DR10: Use deterministic, keyboard-friendly menus, descriptive headings, text statuses, and screen-reader-friendly Markdown.

UX-DR11: Preserve exact plain-language status patterns: `Feature packet is valid`, `Feature packet is not ready yet`, `Ready for BMAD: not yet`, `Ready for BMAD: ready`, `Non-effects verified`, and `Receipt mismatch detected`.

## Review Carry-Forward Map

| Source | Finding | Epic/story allocation |
|---|---|---|
| preplan review | H1, H2, M4: no Salmon, adjacency, pressure, promotion, Landscape, Graph, or topology expansion; prove no forbidden outputs. | E2-S4, E2-S5, E3-S4, E4-S2 |
| preplan review | H3: keep packet validity separate from BMAD readiness. | E2-S1, E2-S3, E4-S2 |
| preplan review | M2: schema must become a validation contract. | E2-S1, E2-S2 |
| businessplan review | M1: define receipt verification, forbidden-path detection, and repeatable validation. | E2-S4, E2-S5 |
| businessplan review | M2: choose MVP write path and archive compatibility without violating write scope. | E1-S2, E2-S4, E3-S4 |
| businessplan review | M3: preserve `packet_valid=true` / `bmad_ready=false` examples. | E2-S3, E4-S2 |
| techplan review | M1: lock validation mechanism before schema tests and dependent scripts. | E2-S1 |
| techplan review | M2: sequence setup/structure validation before shared helper refactoring. | E1-S3, E4-S4 |
| techplan review | M3: no Lens constructs or current NextLens top-down runtime dependencies. | Every story; especially E1-S1, E1-S2, E3-S4 |
| techplan review | L2: marketplace metadata and license are deferred but must be owned. | E1-S4, E4-S4 |
| finalizeplan review | H1: name `NextLens` / `TargetProjects/nextlens/src/NextLens` and repeat forbidden surfaces. | This file, `stories.md`, and every story card |
| finalizeplan review | H2: allocate predecessor and current review findings into concrete implementation work. | This map plus all story source/carry-forward fields |
| finalizeplan review | M1: first validator story locks handwritten Python validation. | E2-S1 |
| finalizeplan review | M2: scaffold and implementation stories include non-Lens negative acceptance criteria. | Every story card |
| finalizeplan review | M3: receipt/run-metadata verification and false-receipt/forbidden-write fixtures precede accepted create success. | E2-S4, E2-S5 before E3-S4 |
| finalizeplan review | M4: trace PRD/UX to business-plan and architecture to tech-plan. | Constitutional traceability section and E4-S4 |
| finalizeplan review | M5: story bundle exists before dev readiness. | `stories.md`; follow-on story files still required before dev readiness |

## FR Coverage Map

FR1: Epic 3 - create workflow entry (`E3-S1`).

FR2: Epic 3 - raw context intake and draft composer (`E3-S2`).

FR3: Epic 3 - explicit/module context resolution (`E3-S1`, `E3-S2`).

FR4: Epic 2 and Epic 3 - validator and workflow reject branch/editor/cwd inference (`E2-S2`, `E3-S1`).

FR5: Epic 3 - preview and write target display (`E3-S3`).

FR6-FR10: Epic 3 - candidate identification, exactly-one selection, deferred candidates (`E3-S2`).

FR11-FR20: Epic 2 and Epic 3 - sufficiency, scope, protective blockers (`E2-S2`, `E3-S2`).

FR21-FR32: Epic 2 and Epic 3 - preview, confirmation, write containment, duplicate handling (`E2-S4`, `E3-S3`, `E3-S4`, `E3-S5`).

FR33-FR42: Epic 2 - read-only validation, schema version/source mode, topology null/unpromoted, readiness separation (`E2-S1`, `E2-S2`, `E2-S3`).

FR43-FR48: Epic 2 and Epic 3 - receipt, run metadata, verification, forbidden writes (`E2-S4`, `E2-S5`, `E3-S4`).

FR49-FR54: Epic 4 - README, examples, golden fixtures, traceability (`E4-S1`, `E4-S2`).

FR55-FR58: Epic 4 - read-only status and future handoff fields without mutation (`E4-S4`).

UX-DR1-UX-DR11: Epic 3 and Epic 4 - command-native UX, copy, preview, blockers, receipt summaries, docs (`E3-S1`, `E3-S2`, `E3-S3`, `E4-S1`, `E4-S2`).

AR1-AR12: Epic 1, Epic 2, Epic 3, and Epic 4 - module scaffold, scripts, validation, packaging, evals, metadata.

## Epic List

### Epic 1: Installable Standalone Module Boundary

Operators and maintainers can install, discover, and validate a standalone Bottom-Up LENS BMad module whose structure and documentation make clear that implementation belongs in `NextLens` at `TargetProjects/nextlens/src/NextLens` and must not reuse Lens governance/runtime constructs.

**FRs covered:** FR1, FR3, FR4, FR5, FR49, FR53, FR54.

**Stories:** E1-S1 through E1-S4.

### Epic 2: Read-Only Validation and Non-Effects Proof

Operators and support users can validate packets, BMAD readiness, path plans, and receipts without mutation before any create workflow can claim accepted-packet success.

**FRs covered:** FR11-FR20, FR21-FR24, FR28-FR31, FR33-FR48, FR52, FR54.

**Stories:** E2-S1 through E2-S5.

### Epic 3: Confirmed Packet Creation Workflow

Operators can start from one feature, select exactly one candidate, collect local sufficiency and scope boundaries, preview the packet and non-effects, confirm deliberately, and write a packet only when validation, path guards, and receipt verification allow it.

**FRs covered:** FR1-FR32, FR37-FR45.

**Stories:** E3-S1 through E3-S5.

### Epic 4: Examples, Evals, and Release Readiness

Maintainers can keep documentation, examples, golden fixtures, artifact evals, trigger evals, and package metadata synchronized so the module is distributable and future consumers can read packet/readiness state without mutating topology.

**FRs covered:** FR49-FR58 plus NFR14 and AR11-AR12.

**Stories:** E4-S1 through E4-S4.

## Epic 1: Installable Standalone Module Boundary

Goal: deliver the module skeleton, setup/config/help surfaces, validation placeholders, and package metadata decisions that make Bottom-Up LENS installable as a standalone BMad module while keeping the Lens boundary explicit.

### Story E1-S1: Scaffold Standalone Bottom-Up LENS Module Skeleton

As a NextLens module maintainer, I want the Bottom-Up LENS module skeleton created in the `NextLens` repository, so that implementation starts from BMad Builder conventions rather than Lens lifecycle conventions.

**Acceptance Criteria:**

**Given** the dev agent is implementing `nextlens-src-bottomup`, **When** it creates the initial module skeleton, **Then** all implementation writes occur only under `TargetProjects/nextlens/src/NextLens`.

**Given** the skeleton exists, **When** maintainers inspect it, **Then** it contains `.claude-plugin/marketplace.json`, `skills/bul-setup`, `skills/bul-create-packet`, `skills/bul-validate-packet`, `skills/bul-verify-receipt`, `evals`, `README.md`, and `LICENSE` placeholders or initial files.

**Given** the skeleton is reviewed, **When** dependencies are inspected, **Then** it has no `feature.yaml`, governance publish flow, Lens branch topology, Lens constitution runtime, release clone dependency, or current NextLens top-down runtime dependency.

### Story E1-S2: Register Setup Configuration and Help Discovery

As a BMad operator, I want setup to register output roots and help entries, so that Bottom-Up LENS can be discovered and configured without Lens governance side effects.

**Acceptance Criteria:**

**Given** `bul-setup` runs in a consuming project, **When** it merges configuration, **Then** it registers `packet_output_path`, `reports_output_path`, and `default_packet_schema_version` using setup-only writes to BMad config/help files.

**Given** help discovery is inspected, **When** module help is loaded, **Then** the actions `configure`, `create`, `validate`, and `verify` are available with clear arguments.

**Given** setup is reviewed, **When** negative boundary checks run, **Then** setup does not write governance mirrors, release clones, Lens lifecycle metadata, Lens constitution runtime state, or current NextLens top-down runtime files.

### Story E1-S3: Add Module Validation and Trigger Eval Placeholders

As a maintainer, I want early module validation and trigger eval placeholders, so that later implementation can prove package shape and invocation boundaries before release.

**Acceptance Criteria:**

**Given** the scaffold is present, **When** module validation placeholders are inspected, **Then** they identify the BMad Module Validate command or equivalent validation route that must pass before distribution.

**Given** trigger eval placeholders exist, **When** they are reviewed, **Then** they include positive Bottom-Up LENS trigger phrases and negative Lens lifecycle trigger phrases.

**Given** eval placeholders are reviewed, **When** negative boundary checks run, **Then** they assert no `feature.yaml`, governance publish, Lens branch topology, Lens constitution runtime, release clone, or current NextLens top-down runtime dependency is required.

### Story E1-S4: Complete Marketplace and Package Metadata Decisions

As a distributor, I want package metadata decisions captured in the module, so that the installable module can pass validation without cleanup drift.

**Acceptance Criteria:**

**Given** package metadata is finalized, **When** `.claude-plugin/marketplace.json` and related docs are reviewed, **Then** owner, repository, homepage, license, keywords, module name `Bottom-Up LENS`, module code `bul`, and version are explicit.

**Given** packaging validation runs, **When** metadata is incomplete, **Then** validation fails with actionable guidance before release.

**Given** package metadata is authored, **When** boundary checks run, **Then** it does not advertise Lens governance installation, Lens branch topology, or NextLens top-down runtime coupling.

## Epic 2: Read-Only Validation and Non-Effects Proof

Goal: create the deterministic proof foundation before accepted packet creation. This epic intentionally precedes the full create workflow's accepted-write success path.

### Story E2-S1: Lock Handwritten Python Packet Validation Contract

As a validator maintainer, I want the MVP validation mechanism locked to handwritten Python rules, so that schema behavior is concrete before tests and packet workflows depend on it.

**Acceptance Criteria:**

**Given** the first validator story is implemented, **When** maintainers inspect validator design docs and script stubs, **Then** MVP validation uses handwritten Python standard-library-first rules and does not add a `jsonschema` dependency.

**Given** future schema interoperability is discussed, **When** the validator contract is read, **Then** `jsonschema` remains a future option only after MVP handwritten validation passes fixtures.

**Given** the validator contract is tested, **When** a dependent schema test or create workflow is attempted first, **Then** the dependency order requires this story to be complete.

### Story E2-S2: Implement Packet Schema Fixtures and Read-Only Validator

As an operator, I want packet validation to run without mutation, so that I can trust packet validity before any write.

**Acceptance Criteria:**

**Given** `bul-validate-packet` receives a packet or draft path, **When** validation runs, **Then** it enforces schema version, `sourceMode=bottom_up`, required identity, selected feature, scope, constraints, assumptions, provenance, receipt reference, and null/unpromoted topology fields.

**Given** invalid fixtures contain multiple selected candidates, missing included scope, missing explicit out-of-scope, promoted assumptions, missing non-inference rules, or branch/editor/cwd identity inference, **When** validation runs, **Then** it returns `status=fail` with structured errors and writes nothing.

**Given** validator tests inspect repository changes, **When** validation completes, **Then** no packet, receipt, report, Lens governance file, release clone file, or current NextLens top-down runtime file is modified.

### Story E2-S3: Implement BMAD Readiness as a Separate Result

As a Lens/BMad operator, I want packet validity and BMAD readiness reported separately, so that valid archive evidence does not imply downstream execution readiness.

**Acceptance Criteria:**

**Given** a valid packet has weak acceptance criteria or missing handoff context, **When** readiness validation runs, **Then** the result can be `packetValid.status=pass` and `bmadReady.status=fail` with reasons.

**Given** status summaries render, **When** operators read them, **Then** they use the labels `Feature packet is valid`, `Feature packet is not ready yet`, `Ready for BMAD: not yet`, and `Ready for BMAD: ready`.

**Given** readiness tests run, **When** fixtures are inspected, **Then** at least one fixture proves a valid packet can be saved while `bmadReady.status=fail` and no BMAD execution is triggered.

### Story E2-S4: Implement Path Guard and Forbidden-Write Fixtures

As a safety reviewer, I want path plans denied before writes, so that packet creation cannot modify forbidden surfaces.

**Acceptance Criteria:**

**Given** a write path is proposed, **When** the path guard resolves it, **Then** it allows only paths contained by configured `packet_output_path` or `reports_output_path` for runtime output and rejects arbitrary absolute paths or traversal.

**Given** fixtures target `.git`, governance, release, landscape, graph, salmon, promotion, adjacency, pressure, roadmap, service/domain/program truth, Lens lifecycle metadata, or current NextLens top-down runtime paths, **When** the guard runs, **Then** it blocks the write and returns a structured failure.

**Given** this story is complete, **When** dependent create stories run, **Then** they use the path guard rather than ad hoc string checks.

### Story E2-S5: Implement Receipt and Run-Metadata Verification

As a support user, I want receipt claims verified against run metadata and changed files, so that false non-effects claims are hard failures.

**Acceptance Criteria:**

**Given** `bul-verify-receipt` receives a receipt and run metadata, **When** verification runs, **Then** it compares `writtenFiles`, `changedFiles`, and `nonEffects` claims against forbidden path categories.

**Given** a false-receipt fixture claims no graph update but changed files include a graph path, **When** verification runs, **Then** it returns `status=fail`, labels the run invalid, and reports the violated non-effect.

**Given** create workflow integration is attempted, **When** this verifier is absent or failing, **Then** the create workflow cannot claim accepted-packet success.

## Epic 3: Confirmed Packet Creation Workflow

Goal: implement the operator journey from "Start from one feature" through candidate selection, local sufficiency, scope safety, preview, confirmation, atomic write, and receipt, using the validators and verifier from Epic 2.

### Story E3-S1: Implement Create Packet Skill Routing and Stage Prompts

As an operator, I want a guided/headless create workflow with stable stage labels, so that the packet creator is understandable and testable.

**Acceptance Criteria:**

**Given** a user asks to start from one feature, **When** `bul-create-packet` activates, **Then** it routes through `context-intake`, `candidate-selection`, `local-sufficiency`, `scope-boundary`, `preview`, `confirmation`, `write`, and `receipt` stages.

**Given** context is displayed, **When** the stage begins, **Then** it shows explicit/module context, output path, and runtime write scope before mutation and rejects branch, open-file, or cwd inference.

**Given** trigger evals inspect activation, **When** Lens lifecycle requests are used, **Then** they do not activate this standalone module or require Lens governance/runtime constructs.

### Story E3-S2: Compose Candidate, Sufficiency, and Scope Drafts

As an operator, I want raw context transformed into one bounded packet draft, so that the selected feature can be validated without premature topology.

**Acceptance Criteria:**

**Given** raw context contains possible feature slices, **When** candidate extraction runs, **Then** it presents unranked candidates, requires exactly one selection, and stores deferred candidates only as unranked notes.

**Given** the selected candidate lacks actor, problem, useful outcome, acceptance criteria, constraints, assumptions, included scope, or explicit out-of-scope, **When** sufficiency/scope stages run, **Then** they collect or block with protective guidance and write no packet.

**Given** a draft is composed, **When** it is inspected, **Then** it preserves raw user wording alongside normalized fields and keeps topology null/unpromoted.

### Story E3-S3: Render Preview, Dry Run, and Confirmation Token

As an operator, I want a preview and deliberate confirmation before any write, so that I can revise, cancel, or confirm safely.

**Acceptance Criteria:**

**Given** a packet draft is valid enough to preview, **When** preview renders, **Then** it shows selected feature, included scope, explicit out-of-scope, assumptions, acceptance criteria, constraints, provenance, intended write target, packet validity, BMAD readiness, and non-effects checklist.

**Given** the operator chooses dry-run or revise, **When** the flow returns to earlier stages, **Then** no packet, receipt, run metadata, governance file, release clone file, or top-down runtime file is written.

**Given** the operator confirms interactively, **When** the final write is requested, **Then** the token must be exactly `CREATE PACKET`; headless mode requires an explicit `--confirm` flag.

### Story E3-S4: Write Packet Atomically and Emit Verified Receipt

As an operator, I want confirmed packet creation to write one accepted packet with receipt proof, so that the safety claim is machine-verifiable.

**Acceptance Criteria:**

**Given** validation, readiness reporting, path guard, confirmation, and receipt verifier pass, **When** packet creation writes, **Then** it writes packet JSON, run metadata JSON, and non-effects receipt JSON only under configured allowed roots using temp files and atomic replace where practical.

**Given** post-write manifest capture runs, **When** changed files are compared, **Then** only allowed packet/report outputs appear and the receipt is generated from observed changes, not prose promises.

**Given** receipt verification fails, **When** the workflow completes, **Then** the accepted packet result is not claimed and the run is marked invalid.

### Story E3-S5: Handle Duplicate, Revise, Cancel, and Interrupted Runs Safely

As an operator, I want duplicate and interrupted runs handled without hidden writes, so that packet identity remains stable and auditable.

**Acceptance Criteria:**

**Given** a packet for the same selected candidate already exists, **When** create runs again, **Then** it detects the duplicate and requires explicit operator resolution before any write.

**Given** the user cancels or leaves a recoverable blocker unresolved, **When** the workflow exits, **Then** it reports what was not written and leaves no accepted packet result.

**Given** draft/run-state cache artifacts are used, **When** they are inspected, **Then** they are inside configured allowed roots, contain no secrets, and do not imply Landscape/Graph/topology promotion.

## Epic 4: Examples, Evals, and Release Readiness

Goal: make the module understandable, testable, and distributable with synchronized examples, fixtures, evals, metadata, and read-only state contracts.

### Story E4-S1: Write Standalone Module README and Command Documentation

As an operator, I want documentation that explains Bottom-Up LENS plainly, so that I understand this is a standalone BMad module and not Lens governance.

**Acceptance Criteria:**

**Given** README and command docs are reviewed, **When** users read them, **Then** they explain "Start from one feature", setup, create, validate, verify, output paths, packet validity, BMAD readiness, and non-effects receipts.

**Given** docs mention Lens-origin concepts, **When** they are reviewed, **Then** they clarify that the module does not publish governance, create Lens branches, enforce Lens constitution runtime, write release clones, or depend on current NextLens top-down runtime modules.

**Given** accessibility guidelines are checked, **When** generated Markdown is viewed, **Then** headings, path tokens, statuses, and examples are readable in VS Code, GitHub Markdown, terminal output, and plain text.

### Story E4-S2: Maintain Golden Valid and Invalid Examples

As a maintainer, I want examples and fixtures synchronized with validators, so that docs and tests prove the same contract.

**Acceptance Criteria:**

**Given** examples are inspected, **When** maintainers review fixtures, **Then** they include valid packet, invalid multi-candidate, invalid missing explicit out-of-scope, forbidden write, false receipt, and valid-not-BMAD-ready cases.

**Given** validator rules change, **When** fixture tests run, **Then** examples fail until they are updated to match the handwritten Python validation contract.

**Given** examples mention future handoff or reporting, **When** they are read, **Then** they preserve archive evidence as distinct from Living Landscape truth and Derived Graph projection.

### Story E4-S3: Add Artifact and Trigger Evals for Module Behavior

As a release reviewer, I want BMad evals that prove artifact behavior and trigger boundaries, so that the module is safe to distribute.

**Acceptance Criteria:**

**Given** artifact evals run, **When** create, validate, and verify flows execute against fixtures, **Then** evals assert packet/receipt/run-metadata shape, forbidden-write denial, false-receipt failure, and `packetValid=pass` with `bmadReady=fail` behavior.

**Given** trigger evals run, **When** Bottom-Up LENS prompts are supplied, **Then** the correct `bul-*` skills activate.

**Given** trigger evals run, **When** Lens lifecycle prompts such as feature initialization, governance publish, constitution resolution, or branch topology work are supplied, **Then** the Bottom-Up LENS module does not activate or import Lens runtime behavior.

### Story E4-S4: Finalize Read-Only State Contracts and Package Validation

As a maintainer, I want final release validation to preserve read-only future handoff and reporting contracts, so that downstream consumers can read packet state without mutation.

**Acceptance Criteria:**

**Given** package validation runs, **When** the module is ready for distribution, **Then** module validation, unit tests, fixture tests, and evals pass before release.

**Given** read-only state fields are inspected, **When** future BMAD handoff or reporting consumes them, **Then** they can read status, provenance, packet validity, and BMAD readiness without mutating packet state or promoting topology.

**Given** full-track constitution traceability is reviewed, **When** release readiness is assessed, **Then** PRD/UX are mapped to the `business-plan` planning requirement and `architecture.md` is mapped to the `tech-plan` requirement.
