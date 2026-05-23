---
feature: lens-seed-improvements
doc_type: prd
status: draft
goal: "Define requirements for Lens two-tree topology, derived projection, audit, and Salmon foundations."
key_decisions:
  - "Features are permanent delivery facts under docs/features and do not move."
  - "Landscape ledgers hold reorganizable service, domain, and program knowledge."
  - "Governance topology maps are derived caches rebuilt from source metadata."
  - "Stable IDs and belongs_to parent references replace path-as-identity."
  - "Planning status is explicit metadata rather than branch location."
open_questions:
  - "When does belongs_to: unknown become a blocking state?"
  - "Where should the derived projection cache live, and which command owns writes?"
  - "Which pilot ledger proves the model with minimal scope?"
  - "Which topology doctor findings block phase advancement?"
  - "What Salmon materiality threshold changes advisory signals into blockers?"
depends_on:
  - nextlens-src-topdownlens
blocks: []
updated_at: "2026-05-22T00:00:00Z"
---

# Product Requirements Document: Lens Seed Improvements

## 1. Overview

Lens Seed Improvements defines the product requirements for removing or superseding the old fixed `domain > service > feature` construct in Lens planning. The feature establishes a Two-Tree Model with Derived Map so new Lens work can be resolved by stable metadata instead of path, branch, domain, or service assumptions.

The model separates three concerns that are currently blended together:

- Feature records are permanent delivery facts under `docs/features/`.
- Landscape ledgers are reorganizable service, domain, and program knowledge stores.
- Governance topology maps are derived projection caches rebuilt from source metadata.

The first delivery slice must prove the topology shift without requiring a broad historical migration. It must support stable IDs, explicit parent references, permanent feature records, one pilot ledger, projection rebuild, topology doctor checks, and advisory Salmon signals.

## 2. Problem Statement

Lens currently over-identifies folder structure, branch topology, and fixed domain/service placement with identity and status. This creates several product problems:

- Features are treated as isolated knowledge pockets even after their learnings should inform services, domains, or programs.
- Paths act like identity, so moving or reorganizing knowledge appears to change what the work is.
- Planning branches can be mistaken for lifecycle status, even when status should be explicit metadata.
- New work can be blocked when it does not naturally fit an existing `domain > service > feature` hierarchy.
- Governance maps, feature records, and living project knowledge can drift because authority boundaries are unclear.

The immediate failure mode is visible in this feature: the work exists to remove the legacy domain/service gate, but the current lifecycle can block it because a domain/service placement is missing. The product must let Lens represent unknown or evolving topology honestly without requiring fake placeholders.

## 3. Goals and Non-Goals

### Goals

G-1. Establish stable typed identity for new feature and landscape records.

G-2. Keep feature records permanently under `docs/features/` and prevent topology changes from moving delivery evidence.

G-3. Introduce reorganizable landscape ledgers for service, domain, and program knowledge.

G-4. Treat governance topology maps as generated caches derived from source metadata.

G-5. Replace path, branch, domain, and service inference with explicit metadata resolution.

G-6. Provide topology doctor diagnostics with phase-aware severity.

G-7. Introduce Salmon as advisory upstream-impact signaling before any blocking policy is enforced.

G-8. Preserve compatibility for legacy reads while making new two-tree writes stricter.

### Non-Goals

NG-1. Do not migrate every historical feature, domain, service, or governance artifact in the MVP.

NG-2. Do not build a complete graph platform as the first release.

NG-3. Do not build a reporting UI as part of this feature. Auspex-style reporting is a downstream consumer context only.

NG-4. Do not make Salmon universally blocking in the MVP.

NG-5. Do not require a mature program/domain/service hierarchy before new feature work can start.

NG-6. Do not hand-edit derived governance maps as source truth.

## 4. Personas and Stakeholders

- Lens maintainer: owns lifecycle trust, governance integrity, and day-to-day operation of planning flows.
- Planning conductor: resolves feature context, validates phase gates, and delegates authoring work.
- Downstream BMAD delegate: needs predecessor, sibling, ownership, and phase context without guessing from paths.
- Architect or developer delegate: needs stable requirements and topology context before implementation planning.
- Governance reviewer: checks that published metadata and projection outputs are auditable and consistent.
- Reporting consumer: reads derived projection data for stakeholder status, lineage, ownership, and impact views.

## 5. Core Concepts and Definitions

- Feature tree: the permanent archive of feature delivery facts under `docs/features/<feature_id>`.
- Landscape ledgers: reorganizable service, domain, and program documents that accumulate durable knowledge across features.
- Stable ID: a typed identifier such as `feature:lens-seed-improvements` or `service:lens-core` that remains valid even if files move.
- `belongs_to`: an explicit parent reference from a feature or ledger to another stable ID, or to `unknown` during allowed early phases.
- Derived projection cache: a generated governance topology map built from feature and ledger frontmatter.
- Source metadata: authoritative frontmatter in feature records and landscape ledgers.
- Topology doctor: a read-only diagnostic command or workflow that reports metadata, projection, and lifecycle consistency findings.
- Salmon signal: an upstream-impact note that records discovered consequences for related features, services, domains, or programs.
- Publication state: explicit metadata such as `draft` or `published` that replaces branch location as status authority.
- Legacy topology: existing domain/service/feature structures that may still be read during migration but are not the write target for new two-tree work.

## 6. Functional Requirements

FR-1. Stable IDs: Every new feature and landscape ledger created through the two-tree model must include a unique `stable_id` with a typed prefix.

FR-2. Entity type: Every new source record must include `entity_type`, with MVP-supported values of `feature`, `service`, `domain`, and `program`.

FR-3. Permanent feature path: Every new feature record must declare a `docs_path` under `docs/features/<feature_id>` and must not be moved when parent topology changes.

FR-4. Parent references: Every new feature and ledger must include `belongs_to` as either a resolvable stable ID or the explicit value `unknown`.

FR-5. Unknown parent policy: `belongs_to: unknown` is allowed during intake, PrePlan, and BusinessPlan as a warning. It must be resolved by TechPlan or explicitly waived before FinalizePlan. A waiver must include owner, reason, expiry or review point, and impacted phase gate.

FR-6. Explicit lifecycle state: New two-tree records must store planning phase, track, status, and publication state as metadata. Conductors and validators must not infer those values from branch names or folder location.

FR-7. Landscape ledgers: The MVP must include one pilot landscape ledger that can associate multiple feature stable IDs with a service, domain, or program knowledge surface.

FR-8. Ledger relationships: A landscape ledger must support parent references, child references when applicable, linked feature IDs, ledger path, and publication state.

FR-9. Projection authority: Source metadata is authoritative. A governance topology projection is a generated cache only and must be rebuilt or published by an approved projection rebuild or publish command through approved boundaries.

FR-10. Projection rebuild: The projection rebuild capability must scan feature and ledger metadata, build an ID-to-path index, resolve parent and relationship edges, detect drift, and emit deterministic diagnostics.

FR-11. Projection write boundary: Manual edits to a derived projection cache must be treated as invalid unless they are produced by the approved projection rebuild or publish command.

FR-12. Projection explainability: The projection capability must provide an explain-by-ID view that identifies the source metadata used to produce an entity, parent edge, child edge, and feature-to-ledger relationship.

FR-13. Topology doctor: The product must provide read-only doctor checks for duplicate IDs, missing fields, broken parent refs, wrong parent types, feature path mismatch, orphaned records, ledger drift, projection drift, and unchecked Salmon signals.

FR-14. Phase-aware doctor severity: Doctor findings must classify as info, warning, or blocker based on lifecycle phase and publication state.

FR-15. Legacy compatibility: The system must read existing legacy topology where needed during migration, but new two-tree write flows must create feature records under `docs/features/` and must use stable IDs and parent metadata.

FR-16. Salmon advisory mode: The MVP must allow Salmon signals to be recorded and checked as advisory findings by default.

FR-17. Salmon blocking threshold: Salmon findings may block only when they identify material discovered impact that contradicts published ledger truth, breaks stable identity or parent resolution, invalidates lifecycle-critical assumptions, or exposes implementation-critical dependency, security, or API inconsistency.

FR-18. Reporting consumption: Reporting tools may consume projection outputs, but the MVP must not make reporting UI behavior the primary product scope.

FR-19. Audit trail: Projection rebuild, doctor, and Salmon checks must report enough source paths and stable IDs for a reviewer to trace each finding back to source metadata.

FR-20. Waiver visibility: Any waiver for unresolved parentage, projection drift, or Salmon materiality must be machine-readable and visible to lifecycle validation.

## 7. Non-Functional Requirements

NFR-1. Determinism: Projection rebuild must produce the same output for the same source metadata and configuration.

NFR-2. Auditability: Every derived entity and relationship must be traceable to one or more source metadata records.

NFR-3. Boundary safety: Projection cache writes must happen only through approved rebuild or publish boundaries and must not require hand-copy publication.

NFR-4. Incremental adoption: New two-tree flows must work before broad historical migration is complete.

NFR-5. Backward-compatible reads: Legacy topology should remain readable where needed so existing planning artifacts are not stranded.

NFR-6. Strict new writes: New two-tree writes must follow stable ID, parent reference, permanent feature path, and publication metadata requirements.

NFR-7. Clear diagnostics: Doctor and projection errors must include stable ID, source path, finding code, severity, phase relevance, and suggested next action.

NFR-8. Low ceremony: Early phases must allow honest uncertainty through `belongs_to: unknown` warnings instead of requiring fake domain or service placeholders.

NFR-9. Local validation: Core checks must be runnable locally before governance publication or phase advancement.

NFR-10. Minimal first slice: The MVP must avoid requiring mature domain/program hierarchy, full migration, or always-blocking Salmon behavior.

## 8. MVP Scope and Phasing

### MVP Scope

MVP-1. Stable IDs and entity types for new feature and ledger records.

MVP-2. Explicit `belongs_to` parent references, including controlled `unknown` handling.

MVP-3. Permanent feature archive writes under `docs/features/`.

MVP-4. One pilot landscape ledger with linked feature IDs.

MVP-5. Deterministic projection rebuild and check behavior.

MVP-6. Topology doctor checks for duplicate IDs, broken parents, path mismatch, orphan policy, and projection drift.

MVP-7. Explicit planning and publication metadata for new two-tree flows.

MVP-8. Advisory Salmon signal recording and recursive consistency checks.

### Later Scope

L-1. Selective historical migration of high-value completed features.

L-2. Expanded service, domain, and program ledger hierarchy.

L-3. Mature ledger promotion policy for completed feature learnings.

L-4. Rich reporting or Auspex-style stakeholder UI built on projection data.

L-5. Stronger Salmon blocking automation after materiality rules are proven.

## 9. User Journeys

UJ-1. New feature intake with unknown parent:

1. A conductor creates a new feature record under `docs/features/<feature_id>`.
2. The record receives a stable feature ID, explicit lifecycle metadata, and `belongs_to: unknown`.
3. The topology doctor reports the unknown parent as a warning during intake, PrePlan, and BusinessPlan.
4. The feature can continue through early planning without a fake domain/service placeholder.
5. TechPlan requires the parent to resolve or a waiver to be recorded before FinalizePlan.

UJ-2. Feature attaches to a service ledger:

1. A maintainer identifies the correct service ledger for the feature.
2. The feature updates `belongs_to` to the service stable ID.
3. The pilot service ledger lists the feature stable ID.
4. Projection rebuild resolves the parent/child and feature-to-ledger relationship.
5. Doctor confirms there is no parent mismatch or projection drift.

UJ-3. Landscape reorganizes without moving feature evidence:

1. A service ledger changes parent domain or program as understanding matures.
2. The feature record remains under its original `docs/features/<feature_id>` path.
3. Projection rebuild updates derived parent and child edges.
4. Downstream delegates resolve current topology through stable IDs and metadata.

UJ-4. Salmon advisory signal:

1. A feature records a Salmon signal against a related service, domain, program, or feature stable ID.
2. Salmon checks walk parent and child relationships in the derived projection.
3. Non-material impacts appear as advisory findings.
4. Material impacts become blockers only when they meet the defined threshold.

UJ-5. Reporting consumer reads projection:

1. A reporting surface reads the projection output or its diagnostics.
2. The report shows feature status, ownership, parentage, and impact context.
3. The report does not become the source of topology truth.

## 10. Data and Metadata Requirements

DM-1. Feature metadata must include `stable_id`, `entity_type`, `feature_id`, `title`, `belongs_to`, `docs_path`, `status`, `phase`, `track`, and `publication_state`.

DM-2. Feature metadata should support `related_to`, `depends_on`, `target_repos`, `salmon_upstream`, and `salmon_status`.

DM-3. Ledger metadata must include `stable_id`, `entity_type`, `title`, `belongs_to`, `ledger_path`, linked `features`, and `publication_state`.

DM-4. Ledger metadata should support `children`, owner or steward metadata, promotion notes, and status.

DM-5. Stable IDs must be unique across feature and ledger source records scanned by the projection rebuild.

DM-6. Parent references must use stable IDs except for the explicit sentinel value `unknown`.

DM-7. Projection output must include derived entity records, source paths, parent edges, child edges, feature-to-ledger relationships, lifecycle state, publication state, and diagnostics.

DM-8. Projection diagnostics must include stable ID, finding code, severity, source path, affected relationship, and recommended resolution.

DM-9. Waivers must be represented as metadata or a referenced artifact that can be read by lifecycle validation.

DM-10. Metadata schemas must distinguish source fields from derived projection fields so users do not confuse cache output with authority.

## 11. Validation, Audit, and Doctor Requirements

VD-1. The topology doctor must be read-only and must not mutate source records or projection caches.

VD-2. Doctor severity categories must include `info`, `warning`, and `blocker`.

VD-3. During intake, PrePlan, and BusinessPlan, `belongs_to: unknown` must be reported as a warning, not a blocker.

VD-4. During TechPlan, unresolved `belongs_to: unknown` must block TechPlan completion unless resolution is scheduled and accepted by the lifecycle policy.

VD-5. Before FinalizePlan, unresolved `belongs_to: unknown` must block unless an explicit waiver exists.

VD-6. During Dev and Complete, duplicate stable IDs, broken resolved parent references, moved feature paths, and projection drift must block phase advancement or completion.

VD-7. Projection drift must block publication of a derived projection cache.

VD-8. Ledger promotion gaps should start as warnings in the MVP unless a later promotion policy marks them blocking.

VD-9. Unchecked Salmon advisory signals should warn during planning and block only when the materiality threshold is met.

VD-10. Doctor output must be suitable for local runs, CI checks, and lifecycle gate reporting.

VD-11. Audit output must explain whether a finding comes from source metadata, projection cache mismatch, lifecycle policy, or waiver policy.

## 12. Salmon Workflow Requirements

SW-1. Salmon must support recording an upstream-impact signal with target stable ID, summary, impact category, materiality, status, source feature or ledger, and review timestamp.

SW-2. Salmon status must support at least `advisory`, `material`, `resolved`, and `waived`.

SW-3. Salmon checks must traverse upward through `belongs_to` parents and downward through ledger children and feature memberships when projection data is available.

SW-4. Advisory Salmon findings must not block early planning by default.

SW-5. Material Salmon findings must block only when they contradict published ledger truth, invalidate lifecycle-critical assumptions, break stable identity or parent relationships, or expose implementation-critical dependency, security, or API inconsistency.

SW-6. Salmon waivers must identify owner, rationale, affected stable IDs, and review point.

SW-7. Salmon diagnostics must be consumable by the topology doctor and projection consumers.

## 13. Compatibility and Migration Requirements

CM-1. Existing legacy topology must remain readable for context discovery during migration.

CM-2. New two-tree feature writes must not require domain/service folder placement.

CM-3. New two-tree feature writes must not create fake domain or service placeholders to satisfy legacy gates.

CM-4. Legacy feature-index behavior must be superseded or derived from projection data for new two-tree flows.

CM-5. Planning branch location must not be treated as authoritative lifecycle status for new two-tree records.

CM-6. Historical migration must be selective and value-driven, starting with records that are actively reused or needed for pilot ledgers.

CM-7. Compatibility docs and diagnostics must identify when a legacy assumption is being read for context versus enforced for new writes.

CM-8. Migration must preserve feature delivery evidence and must not rewrite history to fit the new topology.

## 14. Reporting and Projection Consumers

RP-1. Projection consumers may include lifecycle validation, topology doctor, CI diagnostics, stakeholder snapshots, and future Auspex-style reporting.

RP-2. Reporting consumers must treat projection data as a cache backed by source metadata.

RP-3. A reporting consumer must be able to link status, parentage, ownership, and impact information back to source stable IDs and paths.

RP-4. Reporting UI, dashboard design, and stakeholder visualization are out of MVP scope unless separately planned.

RP-5. Projection formats should support machine-readable consumption without requiring consumers to parse human prose.

## 15. Acceptance Criteria

AC-1. Given a new two-tree feature record with stable ID, entity type, docs path, lifecycle metadata, and `belongs_to: unknown`, when doctor runs during BusinessPlan, then the unknown parent is reported as a warning and does not block BusinessPlan completion.

AC-2. Given a new two-tree feature record with `belongs_to: unknown`, when TechPlan completion is checked, then the parent must be resolved or the lifecycle must record an accepted path to resolution before FinalizePlan.

AC-3. Given a feature reaches FinalizePlan with `belongs_to: unknown`, when no explicit waiver exists, then lifecycle validation blocks advancement.

AC-4. Given source metadata for one feature and one pilot ledger with matching parent and feature references, when projection rebuild runs twice, then both runs produce equivalent projection output and no drift diagnostics.

AC-5. Given two source records with the same stable ID, when topology doctor runs, then it reports a blocker with both source paths.

AC-6. Given a feature record declares a `docs_path` outside `docs/features/`, when it is evaluated as new two-tree work, then doctor reports a blocker.

AC-7. Given a derived projection cache differs from current source metadata, when projection check runs, then it reports projection drift and blocks projection publication.

AC-8. Given a user manually edits the derived projection cache, when projection check compares cache to source metadata, then the change is treated as drift unless reproduced by the approved rebuild or publish command.

AC-9. Given a service ledger changes its parent domain, when projection rebuild runs, then related feature records remain in their original `docs/features/` paths and only derived relationships change.

AC-10. Given a Salmon signal is advisory and no material threshold is met, when doctor runs during planning, then it reports a warning or info finding and does not block.

AC-11. Given a Salmon signal identifies a contradiction with published ledger truth, when lifecycle validation runs, then it reports a blocker until resolved or waived.

AC-12. Given a reporting consumer reads projection data, when it displays parentage or status, then it can trace each value to source stable IDs and paths.

AC-13. Given a legacy feature exists outside the two-tree model, when compatibility discovery runs, then it may be read for context but is not used as the required write pattern for new two-tree work.

AC-14. Given a new feature lacks stable ID or entity type, when doctor runs, then it reports a blocker for new two-tree writes.

AC-15. Given this feature's PRD is reviewed, when scope is evaluated, then MVP scope is limited to stable IDs, parent refs, permanent feature path, pilot ledger, projection rebuild, doctor checks, and advisory Salmon foundations.

## 16. Risks and Open Questions

### Risks

R-1. Dual truth drift: Source metadata, ledgers, and projection cache can disagree if cache authority is unclear.

R-2. Legacy validator lock-in: Existing domain/service gates can continue to block new two-tree flows.

R-3. Parent ambiguity: `belongs_to` can become unclear without typed IDs, allowed parent rules, and waiver policy.

R-4. MVP overload: Stable IDs, ledgers, projection rebuild, doctor checks, and Salmon can become too large if delivered as one implementation unit.

R-5. Salmon noise: Advisory signals can become ignored if materiality rules are vague, or too disruptive if every signal blocks.

R-6. Migration fatigue: Broad historical migration can delay adoption if required before new work benefits.

R-7. Reporting distraction: Auspex-style reporting can pull scope away from topology reliability if treated as the primary deliverable.

### Open Questions

OQ-1. What final phase rule should govern `belongs_to: unknown` during TechPlan completion versus FinalizePlan waiver review?

OQ-2. Where should the derived projection cache live, and which approved command owns writes in each environment?

OQ-3. Which pilot ledger, service, domain, or minimal landscape ledger, proves the model with the least scope?

OQ-4. Which doctor findings are blockers for each phase after the MVP policy is validated?

OQ-5. What exact Salmon materiality threshold should be encoded first?

OQ-6. Which legacy validators must be superseded immediately for new two-tree work, and which can remain as read-only compatibility checks?

OQ-7. What is the first high-value historical feature candidate for later ledger promotion?