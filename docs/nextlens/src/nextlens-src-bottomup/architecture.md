---
feature: nextlens-src-bottomup
doc_type: architecture
status: in-progress
phase: techplan
track: full
goal: "Define technical architecture for a standalone Bottom-Up LENS BMad module MVP."
depends_on:
  - docs/nextlens/src/nextlens-src-bottomup/prd.md
  - docs/nextlens/src/nextlens-src-bottomup/ux-design.md
  - docs/nextlens/src/nextlens-src-bottomup/product-brief.md
  - docs/nextlens/src/nextlens-src-bottomup/research.md
  - docs/nextlens/src/nextlens-src-bottomup/brainstorm.md
inputDocuments:
  - docs/nextlens/src/nextlens-src-bottomup/prd.md
  - docs/nextlens/src/nextlens-src-bottomup/ux-design.md
  - docs/nextlens/src/nextlens-src-bottomup/product-brief.md
  - docs/nextlens/src/nextlens-src-bottomup/research.md
  - docs/nextlens/src/nextlens-src-bottomup/brainstorm.md
  - docs/nextlens/src/nextlens-src-bottomup/businessplan-adversarial-review.md
inputReferences:
  - https://bmad-builder-docs.bmad-method.org/llms-full.txt
constitutional_context:
  permitted_tracks: [express, full]
  planning_required_artifacts: [business-plan, tech-plan]
  dev_required_artifacts: [stories]
  gate_mode: informational
  enforce_stories: true
  enforce_review: true
stepsCompleted:
  - 1
  - 2
  - 3
  - 4
  - 5
  - 6
  - 7
  - 8
workflowType: architecture
lastStep: 8
status: complete
completedAt: 2026-05-20T00:00:00Z
project_name: "Bottom-Up LENS BMad Module"
user_name: "BMad"
date: 2026-05-20
updated_at: 2026-05-20T00:00:00Z
---

# Architecture Decision Document - Bottom-Up LENS BMad Module

_This document builds collaboratively through step-by-step discovery. Sections are appended as we work through each architectural decision together._

## Initialization

Architecture workspace initialized for `nextlens-src-bottomup` with the explicit design pivot that the implementation target is a brand-new BMad module, not an extension of the current Lens runtime constructs.

### Documents Found

- PRD: `docs/nextlens/src/nextlens-src-bottomup/prd.md`
- UX Design: `docs/nextlens/src/nextlens-src-bottomup/ux-design.md`
- Product Brief: `docs/nextlens/src/nextlens-src-bottomup/product-brief.md`
- Research: `docs/nextlens/src/nextlens-src-bottomup/research.md`
- Brainstorm: `docs/nextlens/src/nextlens-src-bottomup/brainstorm.md`
- BusinessPlan Review: `docs/nextlens/src/nextlens-src-bottomup/businessplan-adversarial-review.md`

### Lens Context

- Feature: `nextlens-src-bottomup`
- Domain: `nextlens`
- Service: `src`
- Track: `full`
- Write scope: `docs/nextlens/src/nextlens-src-bottomup`
- Target module model: standalone BMad module following the BMad Builder documentation.
- Implementation boundary: do not depend on existing Lens control/governance/release topology, Lens feature lifecycle commands, Lens constitution resolution, or current NextLens runtime constructs.

### BMad Builder Guide Context

The architecture must follow the BMad Builder module guidance from `https://bmad-builder-docs.bmad-method.org/llms-full.txt`:

- Package capabilities as an installable BMad module with skills plus registration artifacts.
- Use `module.yaml` and `module-help.csv` for configuration and help discovery.
- Use a dedicated setup skill for multi-skill modules; use standalone self-registration only for single-skill modules.
- Prefer workflow skills for bounded artifact-producing processes and agents only when a persistent persona or memory adds value.
- Apply progressive disclosure: keep `SKILL.md` as router/overview, place stage detail in prompt/reference files, and use scripts for deterministic validation.
- Use Python scripts for deterministic checks, path handling, schema validation, and artifact emission; avoid non-portable shell logic.
- Include evals before distribution, with artifact and trigger tests where practical.
- Keep configuration small; expose install-time module config only for cross-cutting values and per-skill customization only where users would otherwise fork.

## Project Context Analysis

### Requirements Overview

**Functional requirements:**

The BusinessPlan defines 58 functional requirements. Architecturally, these group into seven capability areas:

1. **Bottom-up entry and context intake:** provide a clear “Start from one feature” entry surface, accept raw feature context, resolve only explicit/local module context, show output path/write scope, and reject identity inferred from branch, editor state, or current directory.
2. **Candidate identification and selection:** extract possible feature candidates, let the operator select exactly one, preserve deferred candidates as unranked notes, and block no-selection or multi-selection states.
3. **Local sufficiency and scope safety:** capture actor/user, problem, locally useful outcome, acceptance criteria, constraints, assumptions, included scope, explicit out-of-scope, and protective fail-closed guidance.
4. **Packet preview, confirmation, and write:** support preview/dry-run, revise/cancel/confirm, duplicate detection, confirmation token/action, atomic packet write, and approved-path-only emission.
5. **Packet schema and validation:** validate without mutation, enforce schema/source mode, preserve raw and normalized wording, keep topology null/unpromoted, and report `packet_valid` separately from `bmad_ready`.
6. **Non-effects receipt and verification:** emit machine-readable receipts, record reproducible run metadata, compare claims against changed files, and invalidate false receipt claims or forbidden write attempts.
7. **Documentation, examples, reporting, and future handoff:** ship valid/invalid examples, preserve traceability to source inputs, expose read-only status/provenance/readiness, and make future BMAD handoff consume readiness without reinterpreting packet validity.

**Non-functional requirements:**

- Reliability: validation is idempotent, writes are atomic, unsupported schema versions fail with guidance, and packet validity/readiness are reproducible from saved metadata.
- Security/write safety: packet creation must not write governance mirrors, release clones, Landscape, Graph, Salmon, or any forbidden path. Run metadata must not expose secrets.
- Usability/accessibility: command/prompt-native text flow, deterministic menus, no color-only status, plain-language blockers, explicit write target, and “valid packet” versus “Ready for BMAD” separation.
- Performance: preview/validation must be interactive; receipt verification scales with the run changed-file set, not repository history.
- Testability: golden fixtures and automated tests cover happy path, fail-closed blockers, denied side effects, false receipts, and valid-but-not-BMAD-ready output.

### Standalone BMad Module Interpretation

The implementation must translate the product’s Lens-origin terminology into a standalone BMad module architecture. The module may use BMad concepts such as skills, workflows, setup registration, help discovery, module configuration, progressive disclosure, and evals. It must not depend on existing Lens control-repo planning, Lens governance publication, `feature.yaml`, Lens branch topology, Lens constitution enforcement, release clone conventions, or the current NextLens top-down runtime.

The likely architectural implication is a new module repository or module folder that follows BMad Builder distribution conventions:

- `.claude-plugin/marketplace.json` at the repository root for installable plugin discovery.
- `skills/` containing the module’s skill folders.
- A setup skill if the module contains multiple skills.
- `assets/module.yaml` and `assets/module-help.csv` in the setup skill for registration and help entries.
- Python scripts with PEP 723 metadata for deterministic validation, emission, receipt generation, and receipt verification.
- `evals/` containing artifact and trigger evals before distribution.

### Scale & Complexity

- **Primary domain:** BMad module / developer workflow tooling.
- **Complexity level:** medium-high, because the MVP is text-first but makes safety claims that must be machine-verifiable.
- **Estimated architectural components:** module setup/registration, workflow entry skill, candidate intake/selection, packet composer, schema validator, readiness evaluator, output path guard, packet writer, receipt builder, receipt verifier, documentation/example fixtures, and eval suite.
- **Interaction complexity:** guided command/prompt flow with preview/revise/cancel/confirm states.
- **Data complexity:** low volume, high integrity. Artifacts are small JSON/YAML/Markdown files, but schema semantics and non-effects proof must be precise.
- **Integration complexity:** limited for MVP. It should not call BMAD execution automatically; it only prepares packet/readiness artifacts that future BMAD workflows can consume.

### Technical Constraints & Dependencies

- Use BMad Builder module structure and registration contracts rather than Lens lifecycle contracts.
- Use standard BMad skills as the packaging unit; choose workflow skills for bounded processes and avoid memory agents unless persistent personalization becomes necessary.
- Keep `SKILL.md` bodies concise and route detailed stage instructions to `prompts/` or `references/`.
- Place deterministic logic in Python scripts, not prompt prose or shell pipelines.
- Prefer Python standard library; add dependencies only when the standard library cannot satisfy schema/YAML needs. Current BMad guidance allows PEP 723 `uv run --script` dependencies for cases such as `pyyaml` or `jsonschema`.
- Treat module configuration as install-time, cross-cutting settings only. Per-skill customization should be opt-in and minimal.
- Preserve write boundaries inside the module’s configured packet output root and explicitly deny all derived topology, promotion, and signal paths during MVP packet creation.
- Include documentation examples and golden fixtures as first-class architecture assets, not afterthoughts.

### Cross-Cutting Concerns Identified

- **Write-scope enforcement:** every write-capable script must normalize paths, validate containment, and record what it wrote.
- **Non-effects proof:** the receipt must be generated from observed planned/actual writes, not a manually authored promise.
- **Fail-closed behavior:** failed hard gates write no accepted packet and return protective guidance.
- **Packet validity versus BMAD readiness:** two independent validator results with separate reasons and user-facing labels.
- **Identity and provenance:** stable packet IDs, source mode, raw context references, selected-candidate rationale, deferred candidate notes, and confirmation evidence.
- **Compaction survival:** long guided runs should use the output packet draft or run-state artifact as the cache, following BMad’s document-as-cache pattern.
- **Eval coverage:** artifact evals must inspect packet/receipt outputs, and trigger evals must ensure the module skill fires for bottom-up packet requests but not for unrelated Lens lifecycle commands.

## Starter Template and Module Packaging Evaluation

### Primary Technology Domain

This project is a **BMad module** rather than a web app, service, or Lens runtime extension. The correct “starter” is therefore not a framework generator. It is the BMad Builder module structure:

```text
bottom-up-lens-module/
├── .claude-plugin/
│   └── marketplace.json
├── skills/
│   ├── bul-setup/
│   │   ├── SKILL.md
│   │   ├── assets/
│   │   │   ├── module.yaml
│   │   │   └── module-help.csv
│   │   └── scripts/
│   │       ├── cleanup-legacy.py
│   │       ├── merge-config.py
│   │       └── merge-help-csv.py
│   ├── bul-create-packet/
│   ├── bul-validate-packet/
│   └── bul-verify-receipt/
├── evals/
├── README.md
└── LICENSE
```

### Module Packaging Decision

**Selected approach:** multi-skill module with dedicated setup skill.

**Rationale:**

- The MVP has more than one natural user capability: create packet, validate packet, verify receipt, and eventually prepare BMAD handoff/readiness.
- BMad Builder guidance recommends a setup skill for folders of 2+ skills.
- The setup skill centralizes module registration, help discovery, install-time configuration, and future dependency checks.
- Focused workflow/utility skills keep each user journey composable and easier to evaluate.
- This avoids a monolithic single skill whose `SKILL.md` would become a Lens-like conductor.

### Proposed Module Identity

- **Module name:** Bottom-Up LENS
- **Module code:** `bul`
- **Module type:** standalone BMad module, not an expansion module.
- **Description:** Creates and verifies one safe bottom-up feature packet without inventing topology or emitting downstream side effects.
- **Initial version:** `1.0.0` for the first distributable module package.

### Skill Set Considered

| Skill | Type | Purpose | Keep in MVP? |
|---|---|---|---|
| `bul-setup` | Setup skill | Register module config and help entries. | Yes |
| `bul-create-packet` | Complex workflow | Guided/headless packet creation from raw context through preview, confirmation, write, and receipt. | Yes |
| `bul-validate-packet` | Simple utility/workflow | Validate an existing packet or packet draft without mutation. | Yes |
| `bul-verify-receipt` | Simple utility/workflow | Verify non-effects receipt against run metadata and changed files. | Yes |
| `bul-bmad-readiness` | Simple utility/workflow | Report BMAD readiness independently from packet validity. | Fold into validation for MVP; split later if needed |
| `bul-report-status` | Simple utility/workflow | Read-only packet status summary for reporting consumers. | Post-MVP |

### Technology and Dependency Baseline

- **Runtime:** Python scripts invoked from BMad skills.
- **Supported Python:** Python 3.11+ for dataclasses, `tomllib`, and modern typing.
- **Primary dependencies:** standard library first.
- **YAML support:** use PyYAML only when YAML read/write is required. PyPI currently lists PyYAML `6.0.3` as latest stable; if used in scripts, declare with PEP 723 metadata such as `dependencies = ["PyYAML>=6.0.3"]`.
- **Schema validation:** prefer explicit Python validation for the first MVP contract if it stays small; use `jsonschema` only if JSON Schema interoperability becomes a required deliverable.
- **No Node/web starter:** no React/Next/Vite/CLI starter is required for MVP.

### Architectural Decisions Provided by the Starter Shape

**Language and runtime:** Python for deterministic logic; Markdown skill prompts for judgment and guided flow.

**Build tooling:** no package build step required for MVP. Distribution is file/folder-based through BMad custom module install conventions.

**Project organization:** skills live under `skills/`; deterministic code under each skill’s `scripts/`; templates and examples under `resources/`, `templates/`, or `assets/`; evals under `evals/<skill-name>/`.

**Testing framework:** Python script unit tests under `scripts/tests/`; module/skill evals under `evals/` for artifact and trigger coverage.

**Help discovery:** `module-help.csv` registers each capability with menu codes, arguments, dependency ordering, output-location keys, and outputs.

**Configuration:** `module.yaml` should keep install-time variables small and cross-cutting.

### Module Configuration Defaults

Recommended initial setup variables:

| Key | Purpose | Default |
|---|---|---|
| `packet_output_path` | Root where bottom-up packet artifacts are written. | `{project-root}/docs/bottom-up-lens` |
| `reports_output_path` | Root for validation/verification reports. | `{project-root}/_bmad-output/bottom-up-lens` |
| `default_packet_schema_version` | Default schema version for new packets. | `bul.feature-packet.v1` |

Avoid install-time questions for confirmation wording, stage ordering, validation strictness, or UX copy. Those are module identity and should remain baked into skill instructions. If later teams need different copy/templates, expose narrow per-skill `customize.toml` surfaces rather than module-level switches.

### First Implementation Story

The first implementation story should scaffold the module package skeleton, not implement packet logic. It should create:

1. `.claude-plugin/marketplace.json`
2. `skills/bul-setup/` with `module.yaml`, `module-help.csv`, and merge scripts
3. empty/initial skill folders for `bul-create-packet`, `bul-validate-packet`, and `bul-verify-receipt`
4. a minimal README that describes installation and the no-Lens-construct boundary
5. initial structural validation/eval placeholders

## Core Architectural Decisions

### Decision Priority Analysis

**Critical decisions that block implementation:**

1. Build a standalone BMad module, not a Lens lifecycle extension.
2. Package as a multi-skill module with a dedicated setup skill.
3. Make `bul-create-packet` the primary guided/headless workflow.
4. Put deterministic validation, path checks, write checks, receipt generation, and receipt verification in Python scripts.
5. Enforce fail-closed writes through path containment and pre/post write manifests.
6. Keep packet validity and BMAD readiness as separate validator outputs.
7. Prevent all MVP writes to governance, Landscape, Graph, Salmon, promotion, adjacency, or pressure paths.

**Important decisions that shape architecture:**

1. Use progressive disclosure with `SKILL.md` as router and stage prompts in `prompts/`.
2. Use document/run-state cache artifacts for long packet creation sessions.
3. Use golden fixtures and evals as release gates.
4. Keep module configuration small and cross-cutting.
5. Use explicit schema versioning from the first packet.

**Deferred decisions:**

1. Full BMAD execution from packet is post-MVP.
2. Read-only reporting UI/status dashboard is post-MVP.
3. Adjacency, repeated pressure, Salmon routing, promotion, Landscape, and Graph workflows are post-MVP.
4. JSON Schema dependency is deferred unless implementation needs external schema tooling.

### Module and Skill Architecture

#### `bul-setup`

**Role:** setup skill that registers the module and help entries.

**Responsibilities:**

- Write/merge module config into `_bmad/config.yaml` and `_bmad/config.user.yaml` using BMad setup conventions.
- Merge help entries into `_bmad/module-help.csv` using the anti-zombie pattern.
- Create configured output directories.
- Optionally check for Python and `uv` availability if the installer environment does not already guarantee them.
- Avoid collecting secrets or credentials.

#### `bul-create-packet`

**Role:** primary complex workflow.

**Responsibilities:**

- Accept raw context interactively or in headless mode.
- Extract candidate feature slices using prompt judgment, then persist candidate draft state.
- Require exactly one selected candidate.
- Collect sufficiency and scope fields.
- Render preview showing packet validity, BMAD readiness, write target, and non-effects.
- Require explicit confirmation before write.
- Call deterministic scripts for validation, path checks, packet composition, atomic write, receipt generation, and run manifest persistence.
- Stop without accepted packet output on hard-gate failure.

#### `bul-validate-packet`

**Role:** read-only validation workflow/utility.

**Responsibilities:**

- Load an existing packet or draft packet.
- Validate schema, non-inference constraints, packet validity, and BMAD readiness.
- Return structured JSON and optional Markdown report.
- Guarantee no mutation of packet or source fixtures.

#### `bul-verify-receipt`

**Role:** read-only receipt verification workflow/utility.

**Responsibilities:**

- Load a receipt and its run metadata.
- Compare claimed writes/non-effects against observed changed-file manifest.
- Fail if forbidden paths changed, if required metadata is absent, or if receipt claims conflict with observed changes.
- Return structured JSON and optional Markdown report.

### Data Architecture

#### Packet Artifact

Use JSON for the canonical packet artifact because the packet is machine-read first and must be stable for validation and future automation. Human-readable summaries can be generated as Markdown, but JSON is the source artifact.

Minimum top-level shape:

```yaml
schemaVersion: bul.feature-packet.v1
packetId: <uuid>
kind: feature_packet
sourceMode: bottom_up
status: confirmed
createdAt: <iso8601>
updatedAt: <iso8601>
selectedFeature: {...}
scope: {...}
constraints: {...}
assumptions: {...}
provenance: {...}
validation: {...}
bmadReadiness: {...}
nonEffectsReceiptRef: <path>
topology: {...}
```

Topology must remain null/unpromoted during MVP packet creation:

```yaml
topology:
  belongsTo:
    system: null
    domain: null
    service: null
    capability: null
  assumptionsPromoted: false
  landscapePromoted: false
  graphEdgesEmitted: false
```

#### Receipt Artifact

Use JSON for the canonical receipt artifact. It must contain:

- `receiptId`, `packetId`, `runId`, `createdAt`.
- `writtenFiles`: actual packet/receipt/report paths written by the run.
- `changedFiles`: normalized changed-file manifest captured by the run.
- `forbiddenPathChecks`: pass/fail details by denied category.
- `nonEffects`: booleans for adjacency, pressure, promotion, Salmon, Landscape, Graph, roadmap, service/domain/program ownership.
- `verified`: initial self-verification status when available.

#### Run Metadata

Use JSON for run metadata because verification compares exact path lists and state transitions. If a human-readable log is desired, generate it from the JSON metadata.

### Validation Architecture

Validation is layered:

1. **Schema validation:** required fields, types, schema version, UUID, timestamps.
2. **Packet validity validation:** exactly one selected feature, local value, included scope, explicit out-of-scope, non-inference rules, confirmation, and unpromoted topology.
3. **BMAD readiness validation:** acceptance criteria quality, constraints quality, actor/user clarity, provenance sufficiency, and handoff anti-inference instructions.
4. **Path/write validation:** output root containment and denied path categories.
5. **Receipt verification:** claims match changed files and no forbidden non-effects occurred.

Packet validity can pass while BMAD readiness fails. Receipt mismatch is always invalid, not warning-only.

### API and Command Surface

Register these user-facing help entries:

| Capability | Skill | Action | Args |
|---|---|---|---|
| Setup Bottom-Up LENS | `bul-setup` | `configure` | `[setup|configure]` |
| Create Feature Packet | `bul-create-packet` | `create` | `[--headless] [--context <path>] [--output-path <path>]` |
| Validate Feature Packet | `bul-validate-packet` | `validate` | `<packet-path> [--report]` |
| Verify Non-Effects Receipt | `bul-verify-receipt` | `verify` | `<receipt-path> [--run-metadata <path>] [--report]` |

`bul-create-packet` should expose a clear user phrase: “Start from one feature.” Internally, it can accept `--headless` for evals and automation. It must not auto-trigger BMAD execution after packet creation.

### Security and Write Boundary Decisions

- All configured output paths must be resolved with `pathlib.Path` and normalized before use.
- Writes are allowed only under `packet_output_path` or `reports_output_path`.
- The module must deny writes to paths containing or resolving under these categories: `.git`, governance, landscape, graph, salmon, promotion, adjacency, pressure, release, or arbitrary absolute paths outside configured roots.
- Scripts must write through temp files plus atomic replace where practical.
- Run metadata must avoid environment dumps and never record secrets.

### Infrastructure and Distribution Decisions

- Distribution uses BMad custom module plugin conventions via `.claude-plugin/marketplace.json`.
- No background service, web app, database, or external API is required for MVP.
- CI/release readiness should run Python unit tests plus BMad evals.
- README must document installation, setup, commands, outputs, and explicit non-dependence on Lens governance constructs.

### Decision Impact Analysis

**Implementation sequence:**

1. Scaffold module packaging and setup registration.
2. Implement shared script library for paths, schema constants, validation results, and JSON IO.
3. Implement packet schema validator and golden fixtures.
4. Implement receipt/run metadata model and verifier.
5. Implement `bul-validate-packet` read-only skill.
6. Implement `bul-verify-receipt` read-only skill.
7. Implement `bul-create-packet` guided/headless workflow and wire scripts.
8. Add evals and distribution docs.

**Cross-component dependencies:**

- `bul-create-packet` depends on validator and receipt scripts.
- `bul-validate-packet` and `bul-verify-receipt` must remain usable independently for support/QA.
- `module-help.csv` ordering should place setup before create, create before validate/verify recommendations, but validation and verification should be available anytime.

## Implementation Patterns & Consistency Rules

### Pattern Categories Defined

Critical conflict points identified:

1. Skill folder naming and action naming.
2. Module config key naming and output path resolution.
3. JSON field naming and validator result shape.
4. Packet, receipt, and run metadata file naming.
5. Prompt stage names and status labels.
6. Python script result and error conventions.
7. Test fixture and eval layout.

### Naming Patterns

#### Module and Skill Naming

- Module code is `bul`.
- Skill folders use `{modulecode}-{capability}`.
- Setup skill is always `bul-setup`.
- Workflow/utility skill names are action-oriented: `bul-create-packet`, `bul-validate-packet`, `bul-verify-receipt`.
- Do not use the reserved `bmad-` prefix for these user-built skills.

#### Action Naming

- Use short verbs for help CSV `action`: `configure`, `create`, `validate`, `verify`.
- Use phrase labels for users: “Create Feature Packet,” “Validate Feature Packet,” “Verify Non-Effects Receipt.”
- Use “Start from one feature” as the primary entry copy for creation.

#### File Naming

- Packet: `feature-packet.json` for single output folders, or `feature-packet-{packetId}.json` when multiple packets share a directory.
- Receipt: `non-effects-receipt-{runId}.json`.
- Run metadata: `run-metadata-{runId}.json`.
- Markdown reports: `{action}-report-{runId}.md`.
- Golden fixtures: `valid-*.json`, `invalid-*.json`, and `receipt-*.json`.

### Structure Patterns

#### Skill Structure

Each user-facing skill follows this pattern:

```text
bul-{skill}/
├── SKILL.md
├── prompts/          # for staged workflow instructions when needed
├── resources/        # examples, schemas, copy guidelines, reference docs
├── scripts/          # deterministic operations
│   └── tests/        # unit tests for scripts
└── templates/        # generated report or packet templates if needed
```

`SKILL.md` must stay concise: purpose, activation routing, inputs/outputs, write boundaries, and which prompt/script to load. Detailed stage behavior belongs in `prompts/`.

#### Shared Code Pattern

Avoid hidden shared imports across skill folders unless packaging explicitly provides a shared runtime location. For MVP simplicity, either:

1. keep common script helpers duplicated in each skill when tiny; or
2. create a clearly named shared folder such as `skills/bul-common/scripts/` only if the Module Builder/installer path supports it and help registration marks it internal.

Preferred MVP choice: duplicate small helpers or place shared helpers in each skill’s scripts to keep skills portable.

### Format Patterns

#### JSON Field Naming

- Use lower camelCase for JSON fields: `schemaVersion`, `packetId`, `sourceMode`, `selectedFeature`, `includedScope`, `explicitOutOfScope`, `packetValid`, `bmadReady`.
- Use snake_case only for Python variable names.
- Status values use lowercase kebab or snake tokens consistently by context:
  - JSON enum/status: `pass`, `fail`, `warning`, `blocked`, `confirmed`, `cancelled`.
  - Source mode: `bottom_up`.
  - Schema versions: dotted names such as `bul.feature-packet.v1`.

#### Script Result Shape

Every script that emits structured output returns JSON with this base shape:

```json
{
  "status": "pass|warning|fail|blocked",
  "action": "validate|verify|compose|write",
  "errors": [],
  "warnings": [],
  "artifacts": {},
  "nextAction": "..."
}
```

Errors use:

```json
{
  "code": "missing_explicit_out_of_scope",
  "field": "scope.explicitOutOfScope",
  "message": "Explicit out-of-scope is required before packet creation.",
  "recommendation": "Add at least one concrete adjacent item that is out of scope."
}
```

#### Validation Result Shape

Packet validation includes both validity and readiness:

```json
{
  "packetValid": { "status": "pass|fail", "reasons": [] },
  "bmadReady": { "status": "pass|fail|warning", "reasons": [] },
  "hardBlockers": [],
  "advisories": []
}
```

### Communication Patterns

#### Prompt Stage Labels

Use stable stage labels across human output, run metadata, and tests:

1. `context-intake`
2. `candidate-selection`
3. `local-sufficiency`
4. `scope-boundary`
5. `preview`
6. `confirmation`
7. `write`
8. `receipt`

#### User-Facing Status Labels

- Packet validity: `Feature packet is valid` / `Feature packet is not ready yet`.
- BMAD readiness: `Ready for BMAD: ready` / `Ready for BMAD: not yet`.
- Receipt: `Non-effects verified` / `Receipt mismatch detected`.
- Recoverable blockers should use “not ready yet” and include the next fix.
- False receipts and forbidden writes should use “invalid” and hard-stop language.

### Process Patterns

#### Fail-Closed Write Pattern

All write-capable flows use this sequence:

1. Build draft packet in memory or temporary run state.
2. Validate packet and path plan.
3. Render preview with will-write and will-not-write sections.
4. Obtain explicit confirmation.
5. Capture pre-write manifest for configured output roots.
6. Write packet and receipt through temp file plus atomic replace.
7. Capture post-write manifest.
8. Verify changed files are only allowed outputs.
9. Emit receipt from observed changes.

If any hard check fails, write no accepted packet. If a temporary file exists from a failed write, report rollback guidance and mark `packetValid.status=fail`.

#### Path Guard Pattern

All paths must be resolved with `Path(...).expanduser().resolve()` before containment checks. A write target is allowed only if it is relative to the configured output root after resolution.

Forbidden path category checks must be data-driven so tests can assert them. At minimum deny path segments or resolved paths associated with:

- `.git`
- `governance`
- `release`
- `landscape`
- `derived-graph` or `graph`
- `salmon`
- `promotion`
- `adjacency`
- `pressure`

#### Confirmation Pattern

The create workflow must not treat Enter as confirmation for the final write unless the preview clearly declares the default. Preferred MVP: require an explicit token such as `CREATE PACKET` in interactive mode and a `--confirm` flag in headless mode.

#### Read-Only Skill Pattern

`bul-validate-packet` and `bul-verify-receipt` are read-only by default. If `--report` is requested, they may write a report only under `reports_output_path`. They must never modify the packet or receipt under inspection.

### Enforcement Guidelines

All AI agents implementing this module must:

- Follow BMad module packaging conventions rather than Lens lifecycle conventions.
- Keep each skill independently understandable and portable.
- Use JSON as canonical machine artifact format.
- Treat Markdown summaries as generated views, not source truth.
- Preserve packet validity and BMAD readiness as separate states.
- Write no topology, promotion, Salmon, adjacency, pressure, Landscape, or Graph artifacts in MVP packet creation.
- Add unit tests for every deterministic script that can block or write.
- Add artifact evals for generated packet/receipt behavior and trigger evals for skill invocation behavior.

### Good Examples

- `bul-create-packet` calls `scripts/validate_packet.py` and `scripts/write_packet.py` instead of embedding validation rules only in prose.
- `bul-verify-receipt` returns `status=fail` when a graph file appears in `changedFiles`, even if the receipt claims `graphUpdatesEmitted=false`.
- A packet with sufficient scope but weak BMAD handoff details returns `packetValid=pass` and `bmadReady=fail`.

### Anti-Patterns

- Reusing Lens `feature.yaml`, governance publish scripts, control branches, or constitution gates inside the new module.
- Emitting a packet and later discovering the out-of-scope list was empty.
- Treating deferred candidates as a backlog, roadmap, dependency list, or adjacency graph.
- Storing receipt claims as prose only.
- Allowing `--headless` create to write without an explicit confirmation flag.
- Making module setup ask for many toggles that change the workflow’s identity.

## Project Structure & Boundaries

### Complete Project Directory Structure

```text
bottom-up-lens-module/
├── .claude-plugin/
│   └── marketplace.json
├── skills/
│   ├── bul-setup/
│   │   ├── SKILL.md
│   │   ├── assets/
│   │   │   ├── module.yaml
│   │   │   └── module-help.csv
│   │   └── scripts/
│   │       ├── cleanup-legacy.py
│   │       ├── merge-config.py
│   │       ├── merge-help-csv.py
│   │       └── tests/
│   │           ├── test_merge_config.py
│   │           └── test_merge_help_csv.py
│   ├── bul-create-packet/
│   │   ├── SKILL.md
│   │   ├── prompts/
│   │   │   ├── create-guided.md
│   │   │   └── create-headless.md
│   │   ├── resources/
│   │   │   ├── copy-guidelines.md
│   │   │   ├── packet-fields.md
│   │   │   └── non-effects-contract.md
│   │   ├── scripts/
│   │   │   ├── compose_packet.py
│   │   │   ├── path_guard.py
│   │   │   ├── write_packet.py
│   │   │   ├── build_receipt.py
│   │   │   └── tests/
│   │   │       ├── test_compose_packet.py
│   │   │       ├── test_path_guard.py
│   │   │       ├── test_write_packet.py
│   │   │       └── test_build_receipt.py
│   │   └── templates/
│   │       └── preview-template.md
│   ├── bul-validate-packet/
│   │   ├── SKILL.md
│   │   ├── resources/
│   │   │   ├── schema-reference.md
│   │   │   └── readiness-rules.md
│   │   ├── scripts/
│   │   │   ├── validate_packet.py
│   │   │   ├── readiness_check.py
│   │   │   └── tests/
│   │   │       ├── test_validate_packet.py
│   │   │       └── test_readiness_check.py
│   │   └── templates/
│   │       └── validation-report-template.md
│   └── bul-verify-receipt/
│       ├── SKILL.md
│       ├── resources/
│       │   └── receipt-verification-rules.md
│       ├── scripts/
│       │   ├── verify_receipt.py
│       │   └── tests/
│       │       └── test_verify_receipt.py
│       └── templates/
│           └── verification-report-template.md
├── evals/
│   ├── bul-create-packet/
│   │   ├── evals.json
│   │   ├── triggers.json
│   │   └── files/
│   │       ├── raw-single-feature.md
│   │       ├── raw-multi-candidate.md
│   │       └── raw-missing-out-of-scope.md
│   ├── bul-validate-packet/
│   │   ├── evals.json
│   │   └── files/
│   │       ├── valid-packet.json
│   │       ├── invalid-multi-candidate.json
│   │       └── valid-not-bmad-ready.json
│   └── bul-verify-receipt/
│       ├── evals.json
│       └── files/
│           ├── valid-receipt.json
│           ├── false-graph-receipt.json
│           └── run-metadata.json
├── README.md
└── LICENSE
```

### Architectural Boundaries

#### Module Boundary

The module owns packet creation and packet safety only. It does not own BMAD execution, feature implementation, Lens governance, or topology promotion.

#### Skill Boundaries

- `bul-create-packet` owns interactive/headless creation and may write packet, receipt, and run metadata under configured output roots.
- `bul-validate-packet` owns read-only packet validation and optional report generation.
- `bul-verify-receipt` owns read-only receipt verification and optional report generation.
- `bul-setup` owns registration and install-time configuration only.

#### Data Boundaries

- Packet JSON is the canonical packet source.
- Receipt JSON is the canonical non-effects proof source.
- Run metadata JSON is the canonical verification input.
- Markdown reports are derived views.
- Future BMAD handoff artifacts are downstream consumers, not MVP mutation targets.

#### Write Boundaries

Allowed writes:

- `{packet_output_path}/**`
- `{reports_output_path}/**`
- `_bmad/config.yaml`, `_bmad/config.user.yaml`, and `_bmad/module-help.csv` during setup only.

Denied writes:

- Existing Lens governance mirrors.
- Release clone paths.
- `.git` internals.
- Landscape, Graph, Salmon, promotion, adjacency, pressure, roadmap, service/domain/program truth paths.
- Any path outside configured output roots during packet creation/validation/verification.

### Requirements to Structure Mapping

#### Bottom-Up Entry and Context Intake

- Skill: `skills/bul-create-packet/`
- Prompt stages: `prompts/create-guided.md`, `prompts/create-headless.md`
- Script support: `compose_packet.py`
- Requirements covered: FR1-FR5

#### Candidate Identification and Selection

- Skill: `skills/bul-create-packet/`
- Prompt/resource support: `resources/copy-guidelines.md`, `resources/packet-fields.md`
- Requirements covered: FR6-FR10

#### Local Sufficiency and Scope Safety

- Skill: `skills/bul-create-packet/`
- Shared validation logic: `skills/bul-validate-packet/scripts/validate_packet.py`, `readiness_check.py`
- Requirements covered: FR11-FR20

#### Packet Preview, Confirmation, and Write

- Skill: `skills/bul-create-packet/`
- Scripts: `path_guard.py`, `write_packet.py`, `build_receipt.py`
- Template: `preview-template.md`
- Requirements covered: FR21-FR32

#### Packet Schema and Validation

- Skill: `skills/bul-validate-packet/`
- Scripts: `validate_packet.py`, `readiness_check.py`
- Resources: `schema-reference.md`, `readiness-rules.md`
- Requirements covered: FR33-FR42

#### Non-Effects Receipt and Verification

- Skills: `bul-create-packet`, `bul-verify-receipt`
- Scripts: `build_receipt.py`, `verify_receipt.py`
- Resource: `receipt-verification-rules.md`
- Requirements covered: FR43-FR48

#### Documentation and Examples

- Files: `README.md`, resources docs, eval fixtures.
- Requirements covered: FR49-FR54

#### Read-Only Reporting and Future Handoff Support

- MVP support: packet/readiness/receipt fields and optional Markdown reports.
- Post-MVP extension: `bul-report-status` or downstream reporting integration.
- Requirements covered: FR55-FR58

### Integration Points

#### Internal Communication

- Skills communicate through file artifacts, not process memory.
- `bul-create-packet` writes packet/receipt/run metadata.
- `bul-validate-packet` and `bul-verify-receipt` read those artifacts by explicit path.
- Setup registers output path config keys for all skills.

#### External Integrations

- BMad installer/custom module conventions consume `.claude-plugin/marketplace.json`.
- BMad help consumes `module-help.csv` after setup.
- BMad config files supply output paths.
- Future BMAD workflows may read packet/readiness fields but are not invoked by MVP.

#### Data Flow

```text
raw context
  -> candidate draft state
  -> selected candidate
  -> sufficiency/scope answers
  -> packet draft
  -> validation + readiness results
  -> preview
  -> confirmation
  -> packet JSON + run metadata JSON
  -> non-effects receipt JSON
  -> optional reports
```

### File Organization Patterns

#### Configuration Files

- Setup assets live under `skills/bul-setup/assets/`.
- Runtime project config lives under the consuming project’s `_bmad/` after setup.
- Do not store user runtime config in the module repository.

#### Source Organization

- Prompt judgment lives in `prompts/` and `resources/`.
- Deterministic logic lives in `scripts/` with tests next to scripts.
- Templates are used only for derived Markdown output, not canonical JSON.

#### Test Organization

- Unit tests live under each skill’s `scripts/tests/`.
- BMad evals live under `evals/<skill-name>/`.
- Fixtures live under `evals/<skill-name>/files/` and are read-only inputs.

### Development Workflow Integration

#### Development Commands

- Run Python unit tests for all scripts with pytest or standard `unittest` from the module root.
- Run BMad Module Validate before distribution.
- Run BMad evals for artifact and trigger behavior before release.

#### Build Process Structure

No compiled build is required. The release artifact is the repository/module folder itself.

#### Deployment/Distribution Structure

Distribution occurs by pushing the module repository and installing via BMad custom module support from a Git URL or local path.

## Architecture Validation Results

### Coherence Validation ✅

**Decision compatibility:**

The architecture is internally coherent after the standalone BMad module pivot. The selected packaging model, skill split, Python script boundary, JSON canonical artifacts, and BMad Builder distribution conventions reinforce each other. No decision depends on Lens governance or current NextLens runtime constructs.

**Pattern consistency:**

The implementation patterns support the architectural decisions:

- Multi-skill setup maps to BMad Builder module guidance.
- Progressive disclosure keeps workflow instructions maintainable.
- Script-based validators enforce the non-effects and fail-closed claims.
- JSON result shapes create stable contracts across create, validate, and verify skills.
- Evals directly test the product claims.

**Structure alignment:**

The directory structure supports all chosen module capabilities. Each user journey maps to a skill folder. Deterministic logic and unit tests are colocated. Evals are organized per skill. Setup assets are isolated in the setup skill.

### Requirements Coverage Validation ✅

**Functional requirements coverage:**

- FR1-FR5: covered by `bul-create-packet` entry/context intake and setup-configured output path display.
- FR6-FR10: covered by candidate selection stage and deferred candidate notes.
- FR11-FR20: covered by sufficiency/scope stages and validator rules.
- FR21-FR32: covered by preview/confirmation/write path and duplicate detection design.
- FR33-FR42: covered by `bul-validate-packet`, schema/reference resources, and readiness separation.
- FR43-FR48: covered by receipt builder and `bul-verify-receipt`.
- FR49-FR54: covered by README, resources, examples, fixtures, and evals.
- FR55-FR58: covered by read-only packet/readiness/receipt fields; richer reporting and BMAD handoff are explicitly post-MVP.

**Non-functional requirements coverage:**

- Reliability and integrity are covered through idempotent validators, atomic writes, explicit schema versions, and reproducible run metadata.
- Security and write-scope safety are covered through configured roots, denied categories, path normalization, no secrets in metadata, and setup-only config writes.
- Performance is covered by small local artifacts and verification scoped to changed-file manifests.
- Usability/accessibility is covered through prompt-native stages, deterministic menus, plain-language labels, preview, and blocker copy patterns.
- Testability is covered through unit tests, golden fixtures, and BMad artifact/trigger evals.

### Implementation Readiness Validation ✅

**Decision completeness:**

All critical architecture decisions are documented with enough specificity for implementation: module shape, skill boundaries, data contracts, validation layers, command surface, path guard behavior, and distribution model.

**Structure completeness:**

The project tree names specific folders, files, scripts, templates, resources, and eval fixture locations. It is complete enough for the first scaffolding story.

**Pattern completeness:**

Naming, JSON shape, script result shape, stage labels, write safety, confirmation, read-only flows, and eval requirements are explicit. These are the main areas where multiple AI agents could otherwise diverge.

### Gap Analysis Results

**Critical gaps:** none detected after the standalone module pivot.

**Important gaps to resolve during implementation:**

1. Decide whether schema validation remains handwritten Python or adopts `jsonschema` after the first schema draft exists.
2. Decide the exact confirmation token text during `bul-create-packet` implementation.
3. Decide whether shared script helpers should be duplicated per skill or packaged as a private/internal `bul-common` skill after Module Builder validation confirms the supported shape.
4. Decide exact marketplace metadata: owner, repository URL, license, homepage, and keywords.

**Nice-to-have future enhancements:**

- HTML report generation for validation/verification summaries.
- Read-only reporting/status skill.
- BMAD handoff artifact generator.
- Promotion/adjacency/pressure/Salmon/Landscape/Graph modules or separate features after evidence exists.

### Validation Issues Addressed

The main validation issue was the initial Lens-centric architecture direction. It was corrected by explicitly making the target a standalone BMad module and adding a hard boundary against current Lens constructs.

### Architecture Completeness Checklist

**✅ Requirements Analysis**

- [x] Project context analyzed from PRD, UX, PrePlan artifacts, review findings, and BMad Builder guide.
- [x] Scale and complexity assessed.
- [x] Technical constraints identified.
- [x] Cross-cutting safety concerns mapped.

**✅ Architectural Decisions**

- [x] Standalone BMad module selected.
- [x] Multi-skill setup architecture selected.
- [x] Skill boundaries documented.
- [x] Packet, receipt, validation, and readiness data contracts defined.
- [x] Write-scope and non-effects constraints defined.

**✅ Implementation Patterns**

- [x] Naming conventions established.
- [x] Structure patterns defined.
- [x] JSON and script result formats specified.
- [x] Process patterns documented.

**✅ Project Structure**

- [x] Complete directory structure defined.
- [x] Skill/data/write boundaries established.
- [x] Requirements mapped to skills and files.
- [x] Eval and fixture layout included.

### Architecture Readiness Assessment

**Overall status:** READY FOR FINALIZEPLAN / IMPLEMENTATION PLANNING

**Confidence level:** High for MVP architecture. Medium for exact script helper packaging until Module Builder validation confirms the preferred shared-code shape.

**Key strengths:**

- Clear non-Lens boundary.
- Strong safety model for non-effects and fail-closed behavior.
- BMad-native packaging, registration, help, and eval structure.
- Separate validity/readiness states preserve product intent.
- Concrete project tree supports consistent implementation by agents.

**Areas for future enhancement:**

- Richer reporting output after core JSON contracts stabilize.
- Optional BMAD handoff generation as a separate capability.
- Post-MVP evidence-driven topology modules.

### Implementation Handoff

AI agents implementing this architecture must:

- Treat this document as the source of technical decisions.
- Follow BMad Builder module conventions.
- Avoid Lens governance/control/release constructs entirely.
- Implement deterministic logic in tested Python scripts.
- Preserve fail-closed write behavior and non-effects verification.
- Add evals before declaring the module distributable.

**First implementation priority:** scaffold the standalone BMad module skeleton with `bul-setup`, `bul-create-packet`, `bul-validate-packet`, `bul-verify-receipt`, `.claude-plugin/marketplace.json`, README, LICENSE, and initial eval folders.

## Architecture Completion & Handoff

Architecture is complete for the revised target: a brand-new standalone BMad module for Bottom-Up LENS packet creation. The completed plan translates the Lens-origin product requirement into BMad Builder-native module packaging and preserves the core product promise: start from one feature, write one safe packet, and prove no downstream topology side effects occurred.

Recommended next workflow: proceed to FinalizePlan to produce epics, stories, implementation readiness, sprint status, and story files from this architecture.

### Applicable Hard-Gate Requirements

- Planning artifacts required by constitution: `business-plan`, `tech-plan`.
- Dev artifacts required by constitution: `stories`.
- Review is enforced.
- Stories are enforced before dev.
- Gate mode is informational; no hard pre-authoring violation was detected for TechPlan architecture authoring.

