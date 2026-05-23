---
feature: lens-seed-improvements
doc_type: architecture
status: draft
phase: techplan
updated_at: "2026-05-23T00:00:00Z"
stepsCompleted: [1]
inputDocuments:
  - prd.md
  - ux-design.md
  - research.md
  - product-brief.md
  - product-brief-distillate.md
  - businessplan-adversarial-review.md
  - feature.yaml
  - https://bmad-builder-docs.bmad-method.org/llms-full.txt
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
constitutional_context:
  levels_loaded: [org]
  permitted_track: full
  gate_mode: informational
  enforce_review: true
  enforce_stories: true
---

# Architecture Decision Document: Lens Seed Improvements

## 1. Executive Architecture Summary

Lens Seed Improvements changes Lens from a path-shaped planning model to an identity-shaped planning model. The architecture keeps feature archives as permanent delivery facts under `docs/features/`, introduces living landscape ledgers for service, domain, and program knowledge, and treats projection maps as generated caches derived from authored frontmatter.

The implementation target is `TargetProjects/lens-dev/new-codebase/lens.core.src`. The target already exposes the relevant command family: `lens-doctor`, `lens-map-audit`, `lens-projection-rebuild`, `lens-ledger-promotion`, `lens-salmon-impact`, `lens-topology-design`, and `lens-reporting-snapshot`. TechPlan should enhance those surfaces rather than create parallel workflows.

The user-selected external method source is the BMAD Module Builder reference at `https://bmad-builder-docs.bmad-method.org/llms-full.txt`. This architecture uses its module rules as implementation constraints: plan capabilities first, keep skills progressively disclosed, register commands through `module.yaml` and `module-help.csv`, use setup-skill registration for the multi-skill Lens module, apply anti-zombie registration updates, and validate module structure and help quality before release.

The first implementation slice is command/report/metadata-first. It proves stable IDs, parent references, one pilot ledger, deterministic projection rebuild, topology doctor checks, and advisory Salmon rollup data without building a full workbench UI.

## 2. Constraints And Gates

Resolved Lens context:

- Feature: `lens-seed-improvements`.
- Track: `full`.
- Current phase: `businessplan-complete`.
- Planning docs path: `docs/features/lens-seed-improvements`.
- Target repo: `TargetProjects/lens-dev/new-codebase/lens.core.src`.

Constitution context:

- Org constitution loaded.
- `full` track is permitted.
- Gate mode is informational.
- Review enforcement is enabled.
- Dev story enforcement is enabled.

BusinessPlan review findings carried into this architecture:

- Projection write ownership must be executable, not prose-only.
- `belongs_to: unknown` must become a machine-readable lifecycle rule.
- Salmon rollup UX must be sliced down to a minimal command/report workflow.
- Signal clustering and materiality rules must be deterministic enough for tests.
- Promotion review must not bypass governance boundaries.

## 3. Architectural Decisions

### AD-001: Source Truth Uses Two Authored Trees And One Derived Cache

Decision: use the Two-Tree Model with Derived Map.

- Feature archive tree: permanent delivery facts under `docs/features/<feature_id>`.
- Landscape ledger tree: living service, domain, and program knowledge under `docs/**/ledger/` or another configured `landscape_root`.
- Projection cache: generated `governance-map.json` and `governance-map.md` under the configured reporting or governance projection output path.

Feature archives and ledgers are source truth. Projection files are disposable derived views. No workflow may hand-edit projection output and treat it as authority.

Rationale: this separates delivery evidence from reorganizable product knowledge and prevents folder moves from changing identity.

### AD-002: Stable IDs Replace Path Identity

Decision: every governed authored entity uses a typed `stable_id` whose prefix matches `entity_type`.

Supported MVP entity types:

| Entity | Prefix | Authored location | Parent rule |
| --- | --- | --- | --- |
| `feature` | `feature:` | `docs/features/<feature_id>/feature.yaml` or feature markdown | `belongs_to` may reference `service:`, `domain:`, `program:`, or `unknown` |
| `service` | `service:` | service ledger | parent should be `domain:` when known |
| `domain` | `domain:` | domain ledger | parent should be `program:` when known |
| `program` | `program:` | program ledger | no parent required |

Required source metadata for new two-tree writes:

- `stable_id`
- `entity_type`
- `title`
- `status`
- `publication_state`
- `updated_at`
- `belongs_to` where applicable

Feature records also require `feature_id`, `track`, `phase`, and `docs_path`. Target repos remain optional during planning but required before Dev.

### AD-003: `belongs_to: unknown` Is A Planned Topology Debt State

Decision: `belongs_to: unknown` is allowed for early planning but must be phase-classified.

Rules:

- Intake, PrePlan, BusinessPlan: warning only.
- TechPlan: warning if `architecture.md` records an accepted resolution plan or waiver requirement; blocker if no plan exists.
- FinalizePlan: blocker unless `belongs_to` is resolved or a machine-readable waiver exists.
- Dev and Complete: blocker for published entities unless an explicit constitution-approved exception exists.

Machine-readable waiver fields:

```yaml
topology_waiver:
  code: belongs_to_unknown
  owner: ""
  rationale: ""
  affected_stable_ids: []
  review_point: finalizeplan
  expires_at: ""
  impacted_gate: finalizeplan
```

For `lens-seed-improvements`, this architecture is the accepted TechPlan resolution plan. FinalizePlan must not advance until either `belongs_to` resolves to the pilot ledger stable ID or the waiver metadata above is added and accepted.

### AD-004: Pilot Ledger Is Service-Level And Minimal

Decision: the MVP pilot ledger is a service-level ledger for Lens Workbench capability knowledge.

Proposed stable ID:

```yaml
stable_id: service:lens-workbench
entity_type: service
title: Lens Workbench
belongs_to: unknown
publication_state: draft
features:
  - feature:lens-seed-improvements
```

The pilot ledger should prove that multiple feature records can contribute durable knowledge to one living service surface without moving feature archives. Domain and program ledgers remain later scope unless needed to resolve parentage for published projection readiness.

### AD-005: Projection Rebuild Owns Generated Map Writes

Decision: projection rebuild is the only owner of generated governance map output.

Target implementation surface:

- `skills/lens-projection-rebuild/scripts/lens_projection.py` in `lens.core.src`, if absent.
- Existing `lens-projection-rebuild/SKILL.md` remains the user-facing workflow.
- Shared metadata contract remains in `skills/lens-setup/assets/metadata-schema.md`.

Command shape:

```bash
python skills/lens-projection-rebuild/scripts/lens_projection.py rebuild <project-root> \
  --work-intake-path <path> \
  --feature-archive-path <path> \
  --landscape-root <path> \
  --reporting-output-path <path> \
  [--check] [--write] [--explain <stable_id>] [--include-drafts] [--json]
```

Mode semantics:

- `--check`: generate in memory, compare with committed derived files, report drift, do not write.
- `--write`: write generated `governance-map.json` and `governance-map.md` only when doctor blockers are absent or an explicit force flag is accepted.
- `--explain <stable_id>`: show source path, source fields, parent edge, child edges, ledger membership, and diagnostics for one entity.
- `--json`: emit machine-readable summary and findings.
- `--include-drafts`: include draft entities for planning previews and label outputs draft-inclusive.

Projection output must include:

- `generated_at`
- source paths read
- entity records keyed by `stable_id`
- parent edges
- child edges
- feature-to-ledger memberships
- relationship edges from `related_to`, `depends_on`, and Salmon references
- diagnostics with severity, finding code, source path, and recommendation
- a clear marker that the file is derived and not source truth

Determinism requirement: for identical source metadata and config, rebuild output order and content must be stable.

### AD-006: Topology Doctor Is Read-Only And Phase-Aware

Decision: `lens-doctor` remains a read-only triage workflow and should share the projection parser/resolver rather than duplicate logic.

Finding schema:

```yaml
code: duplicate_stable_id | missing_required_field | invalid_stable_id_prefix | broken_belongs_to | parent_type_mismatch | parent_cycle | docs_path_mismatch | projection_drift | unchecked_salmon | invalid_waiver | unpromoted_completed_feature
severity: info | warning | blocker
phase_relevance: intake | preplan | businessplan | techplan | finalizeplan | dev | complete | projection
stable_id: ""
source_path: ""
message: ""
recommendation: ""
```

Phase severity policy:

| Finding | Early planning | TechPlan | FinalizePlan | Published projection |
| --- | --- | --- | --- | --- |
| `belongs_to: unknown` | warning | warning with resolution plan, otherwise blocker | blocker unless waiver | blocker |
| duplicate stable ID | blocker | blocker | blocker | blocker |
| broken resolved parent | warning or blocker by publication state | blocker | blocker | blocker |
| projection drift | info before cache exists | warning | blocker before publication | blocker |
| unchecked advisory Salmon | info or warning | warning | warning unless material | warning |
| material Salmon | warning until reviewed | blocker unless resolved/waived | blocker | blocker |

Doctor output is consumed by projection rebuild, map audit, Salmon impact, and future CI checks. Doctor must not mutate feature archives, ledgers, projections, governance metadata, or config.

### AD-007: Salmon Starts Advisory But Has Testable Materiality

Decision: Salmon is represented as source metadata first and workbench UI later.

Signal schema:

```yaml
salmon_upstream:
  - id: salmon:<feature-id>:<slug>
    source_stable_id: feature:<feature-id>
    target_stable_id: service:<service-id> | domain:<domain-id> | program:<program-id> | feature:<feature-id> | unknown
    category: topology | api | security | dependency | lifecycle | promotion | docs
    summary: ""
    materiality: advisory | material
    status: advisory | material | blocked | waived | resolved
    owner: ""
    rationale: ""
    affected_stable_ids: []
    review_point: ""
    created_at: ""
    reviewed_at: ""
```

Legal state transitions:

- `advisory -> material` when a materiality rule matches.
- `material -> blocked` when unresolved material impact affects a configured gate.
- `material -> resolved` when source or ledger truth is corrected.
- `blocked -> waived` when waiver metadata is complete.
- `blocked -> resolved` when the blocking issue is fixed.
- `waived -> resolved` when the underlying issue is later fixed.

Materiality threshold:

A Salmon signal is material only when it is evidence-backed and at least one condition is true:

- contradicts published ledger truth
- breaks stable identity or parent resolution
- invalidates lifecycle-critical assumptions
- exposes implementation-critical dependency, security, or API inconsistency
- blocks approved promotion into a ledger

Clustering rules for the first slice are deterministic and explainable:

- Strong: same target stable ID plus same category across two or more features.
- Medium: same parent ledger or same doctor finding code.
- Weak: text similarity or adjacency only; report as suggestion, never auto-promote.

The first implementation produces report data for the UX flow. It does not build the Salmon Inbox or Impact Cluster Board UI.

### AD-008: Ledger Promotion Is Explicit And Audited

Decision: promotion from feature facts to landscape ledgers is explicit. It is never an automatic side effect of projection rebuild.

`lens-ledger-promotion` owns promotion planning and optional ledger edits. It must preserve provenance on every promoted entry using fields such as `source_feature`, `source_signals`, and `promotion_rationale`.

Promotion review requires:

- source feature stable IDs and paths
- signal IDs where applicable
- target ledger stable ID and path
- material Salmon review status
- doctor status
- projection preview or explain output
- owner/steward acceptance for the ledger entry

Feature archive paths remain unchanged after promotion.

### AD-009: BMAD Module Builder Rules Govern Lens Module Enhancement

Decision: `lens.core.src` remains a multi-skill BMad module, so enhancements must follow the setup-skill/module registration pattern.

Module Builder rules applied to Lens:

- Use `module.yaml` for module identity, prompts, skills, and versioned registration intent.
- Use `module-help.csv` for discoverable capabilities, args, phase ordering, outputs, and dependencies.
- Use a setup skill for multi-skill registration rather than self-registering each Lens skill.
- Use anti-zombie merge behavior when setup merges module config or help entries: remove previous rows for module code before inserting current rows.
- Keep SKILL frontmatter precise because it is always in context.
- Keep SKILL bodies focused and push detailed procedures into `references/`, `assets/`, `scripts/`, or prompt stage files.
- Validate module structure before release with checks for missing files, orphan help entries, duplicate menu codes, broken references, inaccurate descriptions, missing capabilities, and weak entry quality.

TechPlan consequence: any new or changed Lens command must update `module.yaml`, `module-help.csv`, prompt stubs, setup assets, and tests together.

## 4. Target Implementation Surfaces

Primary target repo: `TargetProjects/lens-dev/new-codebase/lens.core.src`.

Expected Dev changes:

| Surface | Change |
| --- | --- |
| `skills/lens-setup/assets/metadata-schema.md` | Extend and normalize the two-tree metadata, waiver, and Salmon schema. |
| `skills/lens-projection-rebuild/scripts/lens_projection.py` | Add or complete stdlib parser, resolver, doctor, rebuild, explain, check, and JSON output engine. |
| `_bmad/lens-work/skills/lens-projection-rebuild/SKILL.md` | Align command docs with implemented modes and generated output contracts. |
| `_bmad/lens-work/skills/lens-doctor/SKILL.md` | Align read-only doctor contract with phase-aware severity and shared finding schema. |
| `_bmad/lens-work/skills/lens-map-audit/SKILL.md` | Consume shared inventory and finding schema for audit reports. |
| `_bmad/lens-work/skills/lens-salmon-impact/SKILL.md` | Consume Salmon schema and classify advisory/material/blocking impact. |
| `_bmad/lens-work/skills/lens-ledger-promotion/SKILL.md` | Require promotion provenance and material Salmon review before apply. |
| `_bmad/lens-work/skills/lens-topology-design/SKILL.md` | Own explicit parentage and ledger placement decisions; do not write lifecycle state. |
| `_bmad/lens-work/module.yaml` | Register any new prompts/skills or changed capability surfaces. |
| `_bmad/lens-work/module-help.csv` | Keep command discovery, dependency ordering, args, and outputs current. |
| `_bmad/lens-work/README.md` | Document the two-tree command sequence at a high level. |
| tests | Add parser, resolver, projection, doctor, Salmon, and module-registration tests. |

Implementation should prefer one shared parser/resolver module over per-skill parsing. Skill text should describe orchestration and safety; deterministic behavior should live in scripts with tests.

## 5. Component Architecture

### Metadata Inventory Component

Responsibilities:

- scan feature archives and landscape ledgers
- parse YAML frontmatter and standalone `feature.yaml`
- normalize paths relative to project root
- validate required fields by entity type
- build stable ID inventory

Outputs:

- entities by stable ID
- duplicate ID diagnostics
- missing field diagnostics
- source path index

### Relationship Resolver Component

Responsibilities:

- resolve `belongs_to`
- validate allowed parent type
- detect parent cycles
- derive child edges
- derive feature-to-ledger memberships
- collect `related_to`, `depends_on`, and Salmon edges

Outputs:

- parent graph
- child graph
- membership graph
- relationship graph
- unresolved reference diagnostics

### Doctor Component

Responsibilities:

- classify inventory and resolver diagnostics by phase, publication state, and track
- identify projection-blocking conditions
- classify draft advisories separately from published blockers
- emit compact Markdown and JSON summaries

Outputs:

- status: `pass`, `pass-with-advisories`, or `blocked`
- finding list
- projection readiness flag

### Projection Component

Responsibilities:

- build deterministic projection documents from the inventory and relationship graph
- write generated artifacts only in approved output paths
- compare generated output with committed cache in check mode
- explain one stable ID by source metadata

Outputs:

- `governance-map.json`
- `governance-map.md`
- drift diagnostics
- explain-by-ID payload

### Salmon Component

Responsibilities:

- parse `salmon_upstream` entries
- classify materiality
- group signals into deterministic clusters
- trace upstream and downstream impact through projection relationships
- report rollup candidates for ledger promotion

Outputs:

- Salmon impact report
- advisory/material/blocking counts
- candidate larger item summaries
- waiver completeness diagnostics

## 6. Data Contracts

### Projection JSON Contract

```json
{
  "module": "lens",
  "report_type": "governance_projection",
  "derived": true,
  "generated_at": "",
  "source_paths": [],
  "entities": {},
  "edges": {
    "parent": [],
    "child": [],
    "membership": [],
    "related": [],
    "salmon": []
  },
  "diagnostics": [],
  "include_drafts": false
}
```

### Diagnostic JSON Contract

```json
{
  "code": "",
  "severity": "warning",
  "phase_relevance": "techplan",
  "stable_id": "",
  "source_path": "",
  "message": "",
  "recommendation": "",
  "evidence": []
}
```

### Candidate Larger Item Contract

```json
{
  "candidate_id": "candidate:<slug>",
  "title": "",
  "source_signals": [],
  "source_features": [],
  "target_ledger_stable_id": "",
  "cluster_confidence": "strong",
  "highest_status": "advisory",
  "promotion_readiness": "needs-review"
}
```

Candidate records are report outputs until explicitly promoted. They are not ledger truth.

## 7. Governance And Write Boundaries

Allowed writes:

- Planning artifacts under `docs/features/lens-seed-improvements` during this TechPlan.
- Future Dev implementation edits under `TargetProjects/lens-dev/new-codebase/lens.core.src`.
- Generated projection artifacts only through projection rebuild output paths.
- Ledger edits only through explicit `lens-ledger-promotion` or `lens-topology-design` apply paths.
- Governance mirrors only through `publish-to-governance` and sanctioned Lens git orchestration.

Prohibited writes:

- direct hand-copy into governance feature docs
- direct edits to generated projection output as source truth
- moving feature archive folders to express topology
- using reporting snapshots as authoring authority
- adding fake domain or service placeholders only to satisfy legacy gates

## 8. Minimal First Implementation Slice

The first slice should produce a working command/report path:

1. Update metadata schema for stable IDs, `belongs_to`, waivers, and Salmon signals.
2. Add shared inventory and resolver script support.
3. Implement doctor checks for duplicate IDs, missing fields, invalid prefixes, broken parents, unknown parent phase severity, and waiver completeness.
4. Implement projection rebuild `--check`, `--write`, `--explain`, and `--json` for feature and pilot ledger entities.
5. Implement Salmon parsing and deterministic cluster report for same target/category and same doctor finding code.
6. Update module registration artifacts and setup behavior using BMAD Module Builder rules.
7. Add tests and run module validation before release.

The first slice does not build a dashboard, graph editor, or full Salmon Inbox. The UX design remains the target workflow model for later UI surfaces.

## 9. Testing And Validation Strategy

Unit tests:

- frontmatter parser handles markdown and YAML feature files
- stable ID prefix validation matches entity type
- required fields are enforced by entity type and publication state
- `belongs_to` resolver handles known, unknown, broken, wrong-type, and cyclic parents
- waiver completeness validation checks owner, rationale, affected IDs, review point, and impacted gate
- Salmon state transitions reject illegal transitions

CLI contract tests:

- `doctor` emits stable JSON with expected finding codes and severities
- `rebuild --check` exits non-zero or reports drift when committed cache differs
- `rebuild --write` writes deterministic JSON and Markdown when blockers are absent
- `rebuild --explain <stable_id>` reports source path and derived edges
- `--include-drafts` labels output as draft-inclusive

Integration tests:

- feature with `belongs_to: unknown` warns during BusinessPlan and TechPlan with architecture plan, then blocks FinalizePlan without waiver or resolution
- pilot ledger links `feature:lens-seed-improvements` without moving feature docs
- Salmon signals from three features cluster into one candidate larger item report
- projection consumers can trace every edge to source stable IDs and paths

Module validation:

- `module.yaml` includes changed Lens skills and prompt stubs
- `module-help.csv` has no duplicate menu codes or orphan skills
- setup registration uses anti-zombie replacement for Lens rows
- skill descriptions are concise and triggerable
- large procedural detail is in scripts, assets, references, or prompts instead of oversized SKILL bodies

Lifecycle validation:

- BusinessPlan review remains `responses-recorded`
- TechPlan completion requires `architecture.md` and `techplan-adversarial-review.md`
- FinalizePlan input contract sees PrePlan, BusinessPlan, and TechPlan artifacts

## 10. Acceptance Check Mapping

| Requirement | Architecture response |
| --- | --- |
| PRD FR-1 through FR-4 | Stable ID, entity type, permanent feature path, and parent reference model defined. |
| PRD FR-5 | Phase-aware `belongs_to: unknown` and waiver contract defined. |
| PRD FR-7 through FR-12 | Pilot ledger, projection authority, rebuild, write boundary, and explain mode defined. |
| PRD FR-13 through FR-14 | Doctor finding schema and phase-aware severity defined. |
| PRD FR-16 through FR-17 | Salmon advisory mode, materiality threshold, and blocking rules defined. |
| PRD FR-19 through FR-20 | Source path diagnostics and waiver visibility defined. |
| UX AC-1 through AC-3 | Signal, cluster, and candidate larger item contracts defined for report-first implementation. |
| UX AC-4 through AC-6 | Blocked, waived, doctor findings, and promotion gating rules defined. |
| UX AC-7 through AC-10 | Ledger preview/projection/reporting authority boundaries mapped to command outputs. |
| UX AC-11 through AC-12 | Status labels and text alternatives are deferred to UI, with report data retaining status and reason fields. |

## 11. Risks And Mitigations

| Risk | Mitigation |
| --- | --- |
| Projection cache becomes a second source truth | Mark generated files as derived, block hand-authored cache writes, and require explain-by-ID source paths. |
| MVP grows into a full UI build | Deliver command/report/metadata slice first; keep UX panels as later consumers. |
| `belongs_to: unknown` remains unresolved indefinitely | Require architecture plan in TechPlan and waiver or resolution before FinalizePlan. |
| Salmon becomes noisy | Keep advisory default, require evidence-backed materiality, and expose weak clusters as suggestions only. |
| Module registration drifts from changed skills | Apply Module Builder registration rules, update `module.yaml` and `module-help.csv`, and validate before release. |
| Legacy validators keep enforcing domain/service paths | Add compatibility reads but strict two-tree writes; route new writes by stable ID and docs path. |
| Duplicate parsing logic diverges across skills | Centralize inventory and resolver behavior in shared script modules. |

## 12. Dev Handoff

Dev should implement this as a module enhancement in `TargetProjects/lens-dev/new-codebase/lens.core.src`, not in the control repo or release clone. Start with tests around the shared parser/resolver and doctor severity policy, then wire projection rebuild and Salmon reports. Update registration artifacts and setup behavior in the same change set as any command-surface changes.

Before FinalizePlan, resolve or waive this feature's `belongs_to: unknown` state with the machine-readable waiver contract or by attaching the feature to the pilot ledger stable ID.