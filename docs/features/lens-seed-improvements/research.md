---
stepsCompleted: [1, 2, 3]
inputDocuments: ["brainstorm.md", "feature.yaml"]
workflowType: "research"
lastStep: 3
research_type: "technical"
research_topic: "Lens Two-Tree topology redesign"
research_goals:
  - "Identify the core implementation surfaces needed for stable IDs, parent refs, first-class service/domain/program entities, and docs/features permanence."
  - "Define a plausible projection rebuild command design that treats the governance map as a derived cache from frontmatter."
  - "Define topology doctor/audit checks for orphans, broken parent refs, ledger drift, and unpromoted completed features."
  - "Define initial Salmon workflow semantics as advisory-by-default upstream-impact signaling with recursive consistency checks."
  - "Identify compatibility risks with existing Lens lifecycle, validators, and legacy domain/service assumptions."
  - "Recommend an MVP sequencing approach that lets new work use the two-tree model before migrating historical content."
user_name: "BMad"
date: "2026-05-22"
web_research_enabled: false
source_verification: false
source_limitation: "Web search unavailable in this session; research is grounded in local Lens artifacts and user-provided brainstorming notes."
feature_id: "lens-seed-improvements"
doc_type: "research"
status: "draft"
---

# Research Report: Technical

## Research Overview

This PrePlan technical research examines the implementation implications of replacing the old fixed `domain > service > feature` construct with the Two-Tree Model with Derived Map for Lens/BMAD. The research is grounded only in the accepted brainstorm artifact and the feature metadata for `lens-seed-improvements`.

The core direction is to separate immutable delivery evidence from reorganizable human knowledge. Features remain permanent delivery facts under `docs/features/` and never move. Service, domain, and program knowledge move into first-class landscape ledgers that can be reorganized as understanding matures. Stable IDs and explicit `belongs_to` parent references replace path identity. The governance map becomes a derived cache rebuilt from frontmatter, not a hand-authored authority.

This is not an implementation specification and does not assert that code has already changed. It captures the technical surfaces, compatibility risks, and sequencing decisions needed to inform the product brief and later architecture work.

## Methodology and Source Limitation

Inputs used:

- `brainstorm.md`: accepted brainstorming output for re-examining Lens/BMAD topology.
- `feature.yaml`: feature metadata confirming `lens-seed-improvements`, full track, PrePlan phase, draft publication state, and an intentional blocker around the current domain/service/feature gate.

Web search and external source verification were unavailable in this session. The normal BMAD technical research workflow expects web research, but this delegate was instructed to proceed autonomously with local grounding. Therefore, this report records `web_research_enabled: false` and `source_verification: false` in frontmatter and should be treated as a local architectural research artifact, not an externally verified technology survey.

## Current Technical Problem

Lens currently carries assumptions that over-identify path and branch topology with knowledge authority. The fixed `domain > service > feature` hierarchy makes feature folders convenient isolation units, but it also traps durable service, domain, and program knowledge inside feature-local pockets. That creates weak sibling discovery, weak predecessor context, and pressure to preserve old paths because paths behave like identity.

The current feature metadata also shows the practical failure mode: this feature is intentionally `intake-blocked` by a `domain_service_feature_gate` because the PrePlan conductor expects governance feature-index plus domain/service constitution scope. For this redesign, routing through a fake legacy domain/service placeholder would preserve the problem instead of removing it.

The technical problem is therefore not merely folder naming. It is the lack of separate identities for delivery facts, landscape knowledge, lifecycle state, and derived routing views.

## Architecture Implications

The Two-Tree Model implies two durable source trees and one derived projection:

- Feature archive: permanent feature delivery records under `docs/features/`.
- Landscape ledgers: reorganizable service, domain, and program knowledge ledgers under a landscape docs area.
- Governance map: derived cache rebuilt from frontmatter across features and ledgers.

Core implementation surfaces likely needed:

- Metadata parser for feature and ledger frontmatter.
- Stable ID registry or index builder that maps IDs to paths without making paths authoritative.
- Parent reference resolver for `belongs_to` relationships.
- First-class entity model for `feature`, `service`, `domain`, and `program`.
- Projection rebuild command that emits the governance map from source metadata.
- Topology doctor or audit command that checks consistency without mutating source documents.
- Lifecycle context resolver updates so new work can resolve by stable ID and metadata instead of domain/service path assumptions.
- Validator updates so `docs/features/` permanence and derived projection semantics are enforced.
- Salmon workflow surface for upstream-impact signaling and recursive consistency checks.

The design should preserve the distinction from the brainstorm: features are immutable facts, the landscape is an interpretation, and the map is a cache.

## Metadata and Schema Direction

Minimum feature metadata should support permanent delivery identity and lifecycle state:

- `stable_id`: typed stable identifier, for example `feature:lens-seed-improvements`.
- `entity_type`: `feature`.
- `feature_id`: human-readable slug retained for compatibility.
- `title`: display name.
- `belongs_to`: parent service, domain, program, or `unknown` during intake when the parent is not resolved.
- `docs_path`: permanent path under `docs/features/<feature_id>`.
- `status`, `phase`, `track`, and `publication_state`: explicit lifecycle and publication state, replacing branch topology as status authority.
- `related_to`, `depends_on`, `salmon_upstream`, and `salmon_status`: relationship and upstream-impact surfaces.

Minimum landscape ledger metadata should make service, domain, and program entities first-class:

- `stable_id`: typed stable identifier, for example `service:<slug>`, `domain:<slug>`, or `program:<slug>`.
- `entity_type`: `service`, `domain`, or `program`.
- `title`: display name.
- `belongs_to`: parent domain or program when applicable.
- `ledger_path`: current ledger location.
- `features`: list of feature stable IDs associated with the ledger.
- `children`: optional child service/domain/program stable IDs.
- `status` or `publication_state`: draft/published metadata for ledgers.

The schema should allow additive depth. A feature can begin with `belongs_to: unknown` or a single service parent, then later connect into a domain or program without moving the feature folder. This lets new work use the two-tree model before historical content is migrated.

## Projection Rebuild Command

A plausible command could be shaped as `lens projection rebuild`, with read-only scan inputs and an explicit output to the derived governance map cache. The command should not treat the existing map as source truth.

Conceptual behavior:

1. Scan feature frontmatter under `docs/features/**/feature.yaml` or equivalent feature metadata files.
2. Scan landscape ledger frontmatter for service, domain, and program entities.
3. Build an ID-to-path index for every stable ID.
4. Resolve `belongs_to`, `features`, `children`, `related_to`, and `depends_on` references.
5. Emit a derived governance projection containing entity records, parent/child graph edges, feature-to-ledger membership, lifecycle state, and publication state.
6. Report unresolved references, duplicate stable IDs, stale cache signatures, and parent/child mismatches.

Useful command modes:

- `--check`: validate that the generated projection matches the committed cache without writing.
- `--write`: update the derived cache through the approved governance boundary when allowed.
- `--explain <stable_id>`: show source metadata, derived parents, children, and projection path for one entity.
- `--json`: emit machine-readable diagnostics for CI, doctor, or reporting consumers.

The rebuild command should be deterministic. If two inputs declare conflicting ownership, it should fail with diagnostics instead of guessing. If a feature has no parent yet, it should preserve the feature as an orphan or intake item and let doctor decide severity based on phase and status.

## Topology Doctor Checks

The topology doctor should be a read-only diagnostic surface that can run locally before publication or phase advancement. It should focus on structural integrity, ledger drift, and lifecycle readiness.

Recommended checks:

- Duplicate stable IDs across features and ledgers.
- Missing required metadata fields for each `entity_type`.
- Feature paths outside `docs/features/` for new two-tree work.
- Feature folders moved after creation, detected by `docs_path` mismatch.
- `belongs_to` references that do not resolve to a known stable ID.
- Parent references pointing to the wrong entity type.
- Parent lists that omit a child which claims that parent.
- Ledger `features` lists that reference unknown or moved features.
- Feature frontmatter that claims a parent but no corresponding ledger relationship exists.
- Orphan features past an allowed intake phase or beyond a configured grace state.
- Derived governance map drift compared with current frontmatter.
- Completed features whose durable learnings have not been promoted into a service/domain/program ledger.
- Published ledgers that contain draft-only feature claims without explicit allowance.
- Salmon upstream notes that were recorded but not checked.

Severity should be context-aware. Orphans may be warnings during intake and blockers during publication or completion. Ledger drift should block publication of the derived projection but may remain advisory during early authoring. Completed but unpromoted features should start as warnings, then become blockers once a promotion policy is established.

## Salmon Workflow Design

Salmon should begin as advisory-by-default upstream-impact signaling. It should help teams notice when a downstream feature changes assumptions that affect service, domain, program, or related feature knowledge.

Initial Salmon semantics:

- A feature or ledger can record an upstream signal in `salmon_upstream` with a target stable ID, summary, impact category, materiality, and status.
- The default status is advisory, not blocking.
- A recursive consistency check walks upward through `belongs_to` parents and downward through ledger children and feature memberships.
- The check reports impacted ledgers, related features, stale claims, and missing promotion notes.
- Salmon blocks only when the check discovers material impact, such as contradictory published ledger truth, broken parent identity, invalid security or API assumptions, or lifecycle-critical dependency mismatch.

This preserves the brainstorm decision that Salmon is first-class but not heavyweight by default. It should surface cross-feature and upstream consequences without turning every small note into a governance stop sign.

## Lifecycle and Validator Compatibility

The largest compatibility risk is existing Lens lifecycle code that expects a resolved domain and service before a feature can proceed. The current feature metadata already captures this as a blocker. The new model needs compatibility rules that let `feature` be the durable delivery unit while `service`, `domain`, and `program` become optional or progressively resolved parent entities.

Likely compatibility surfaces:

- Feature context resolution must stop inferring identity from path, branch, or domain/service folder placement.
- Phase conductors must accept stable ID and metadata resolution as authoritative.
- Constitution and hard-gate resolution must define what happens when `belongs_to` is `unknown`, service-only, domain/service, or program/domain/service.
- Validators must distinguish source metadata from derived governance projection.
- Existing feature-index checks must either be superseded or rebuilt from projection data.
- Planning branch checks must give way to explicit `publication_state`, draft metadata, and lifecycle fields.
- Existing docs paths and legacy feature folders need backward-compatible reads while new work writes only to permanent `docs/features/` paths.
- Reports and dashboards should consume projection data, not manually curated hierarchy files.

The safest compatibility posture is additive read support and stricter write support: read legacy structures during migration, but make new work use stable IDs, permanent feature paths, and derived projection semantics.

## MVP Implementation Sequence

Recommended MVP sequence:

1. Introduce stable IDs and `belongs_to` metadata for new feature and ledger records.
2. Preserve new features permanently under `docs/features/` and stop moving feature evidence as topology changes.
3. Add one pilot service ledger as the first reorganizable landscape knowledge surface.
4. Build a deterministic projection rebuild command that reads frontmatter and emits or checks the derived governance map.
5. Add a lightweight topology doctor with duplicate ID, broken parent, orphan, and projection drift checks.
6. Update lifecycle resolution so new work can proceed through the two-tree model without fake domain/service placeholders.
7. Replace planning branch status assumptions with explicit draft/published metadata for new two-tree flows.
8. Add Salmon advisory signals and recursive consistency checks after the base projection and doctor are stable.
9. Migrate historical content selectively, starting with high-value completed features and ledgers with active reuse.

This sequence lets new work use the improved topology before historical migration. It avoids forcing a mature program/domain/service structure before Lens has observed enough real usage to justify it.

## Technical Risks

- Dual truth drift: feature metadata, ledgers, and projection cache can disagree if the cache is edited manually or rebuilds are skipped.
- Legacy validator lock-in: existing domain/service gates can continue blocking two-tree work unless lifecycle resolution is updated.
- Parent ambiguity: `belongs_to` can become unclear if multiple parent types are allowed without typed IDs and cardinality rules.
- Over-modeling: adding program, domain, service, graph, and Salmon semantics at once could make the MVP too heavy.
- Migration fatigue: requiring historical cleanup before new work can proceed would slow adoption.
- Incomplete promotion: completed features may remain useful only as isolated delivery records if no ledger promotion habit exists.
- Cache authority confusion: users may continue treating the governance map as source truth unless tooling names and diagnostics reinforce derived-cache semantics.

## Recommendations

Start with the minimum topology shift that changes behavior: stable IDs, explicit parent references, permanent feature folders, one pilot ledger, deterministic projection rebuild, and a lightweight doctor. This is enough to remove path-as-identity and supersede the fixed `domain > service > feature` assumption for new work.

Keep the governance map derived. Do not ask users or agents to hand-author it as an authority. All map writes should come from projection rebuild through approved governance boundaries, and all checks should be able to explain which frontmatter source produced each derived edge.

Make Salmon advisory until the system can distinguish material discovered impacts from ordinary informational notes. Blocking behavior should be reserved for contradictions or unresolved impacts that would invalidate published ledger truth, lifecycle gates, or implementation-critical assumptions.

Treat historical migration as a later, targeted effort. New two-tree work should not wait for all old content to be reclassified. The system should read legacy assumptions where needed but write new records using the two-tree model.

## Handoff Notes

Carry these points into the product brief and architecture phases:

- The old fixed `domain > service > feature` construct is the problem to remove or supersede.
- Features stay permanently under `docs/features/` and never move.
- Service, domain, and program knowledge belongs in reorganizable landscape ledgers.
- Stable IDs and `belongs_to` parent references replace path identity.
- The governance map is a derived cache from frontmatter, not source truth.
- Planning branches should give way to explicit draft and published metadata.
- The topology doctor should expose orphans, broken refs, ledger drift, and unpromoted completed features.
- Salmon starts as advisory upstream-impact signaling with recursive consistency checks and blocks only material discovered impacts.
- MVP adoption should prioritize new work first, then selective historical migration.