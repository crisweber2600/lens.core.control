---
title: "Product Brief Distillate: Lens Seed Improvements"
type: "llm-distillate"
source: "product-brief.md"
created: "2026-05-22"
purpose: "Token-efficient context for downstream PRD creation"
---

# Product Brief Distillate: Lens Seed Improvements

## Requirements Hints

- New two-tree work should create permanent feature records under `docs/features/<feature_id>` and should not move those records when service, domain, or program organization changes.
- Feature and ledger metadata should include stable typed IDs so identity does not depend on file paths or branch names.
- `belongs_to` parent references should be explicit, typed, and resolvable; unresolved parents should be represented deliberately rather than hidden behind fake legacy placeholders.
- Publication and authoring state should be explicit metadata, such as draft and published state, instead of inferred from planning branch topology.
- The governance map should be generated from frontmatter inputs and treated as a derived cache, not hand-authored source truth.
- Projection tooling should be deterministic and able to explain which source frontmatter produced each derived entity or relationship.
- Topology diagnostics should be runnable locally before publication or phase advancement.
- Salmon should become a first-class upstream-impact signal, but start advisory and become blocking only for material discovered impacts.

## Technical Context

- Current feature metadata already models the desired direction with `stable_id: feature:lens-seed-improvements`, `entity_type: feature`, `belongs_to: unknown`, `publication_state: draft`, and `docs_path: docs/features/lens-seed-improvements`.
- Current `status: intake-blocked` and blocker `domain_service_feature_gate` prove that legacy domain/service requirements are actively obstructing this redesign.
- Minimum feature metadata likely includes stable ID, entity type, feature slug, title, parent reference, permanent docs path, lifecycle phase, track, publication state, dependencies, relationships, and Salmon fields.
- Minimum ledger metadata likely includes stable ID, entity type, title, parent reference, current ledger path, linked feature IDs, optional child IDs, and publication state.
- Projection rebuild likely scans feature and ledger frontmatter, builds an ID-to-path index, resolves parent/child and relationship edges, and emits or checks the derived governance map.
- Useful projection modes include check, write through approved governance boundaries, explain-by-ID, and JSON diagnostics for CI or reporting consumers.
- Topology doctor should check duplicate IDs, missing required fields, moved feature paths, broken parents, wrong parent entity types, parent/child mismatches, stale projection cache, orphaned records, unpromoted completed learnings, draft/published mismatches, and unchecked Salmon signals.
- Compatibility posture should be additive reads for legacy topology and stricter writes for new two-tree work.

## Scope Signals

- MVP in scope: stable IDs, explicit parent refs, new work under `docs/features/`, one pilot ledger, deterministic projection rebuild, lightweight topology doctor, draft/published metadata for new flows.
- MVP out of scope: full historical migration, mature program/domain ledger hierarchy, pure graph platform, heavy Salmon blocking policy, broad reporting UI buildout.
- Likely next scope after MVP: selective migration of high-value legacy knowledge, expanded service/domain/program ledgers, Salmon recursive consistency checks, projection consumers for stakeholder reporting.
- Auspex MVP1 reporting UI is an example downstream consumer of topology and status projection, not the primary product direction.

## Rejected or Deprioritized Ideas

- Keep current fixed `domain > service > feature`: rejected because it preserves feature-pocket knowledge scatter and path-as-identity.
- Route this redesign through a fake legacy domain/service placeholder: rejected because it would preserve the gate the feature is meant to remove.
- Governance-only human docs: deprioritized because it risks bypassing local authoring context and creating another manually maintained truth source.
- External docs or hybrid scratch spaces: rejected because they increase fragmentation and weaken auditability.
- Pure graph model for MVP: deprioritized because it is powerful long term but too abstract and heavy for the immediate adoption path.
- Salmon as always-blocking governance: rejected for MVP because it would likely create noise and adoption resistance before impact materiality rules are proven.

## Adoption and Compatibility Notes

- The brief should persuade maintainers that this is a lifecycle reliability problem, not a folder naming preference.
- Downstream agents need queryable predecessor, sibling, and ownership context without relying on human memory or branch inference.
- Validators and conductors must learn to resolve feature context through stable ID and metadata rather than requiring domain/service path placement.
- Constitution and hard-gate resolution needs an explicit policy for `belongs_to: unknown`, service-only parents, domain/service parents, and program/domain/service parents.
- Derived map tooling should make cache authority visible in names, diagnostics, and docs so maintainers do not hand-edit it as source truth.
- New two-tree flows should not wait for every legacy artifact to migrate.

## Open Questions

- What exact metadata field names should become canonical: existing `stable_id` and `entity_type`, or a refined schema for all entity types?
- Which ledger type should be the first pilot: service ledger, domain ledger, or a minimal landscape ledger that can later split by type?
- What phase or publication gate should turn orphaned `belongs_to: unknown` from warning into blocker?
- Where should the derived governance map cache live, and which approved boundary is allowed to update it?
- What is the minimum constitution behavior when a feature has no resolved service or domain parent?
- Which completed features should be first candidates for durable learning promotion?
- What materiality threshold should make Salmon block instead of advise?

## Known Research Limitations

- External web research was unavailable; all conclusions are grounded in local Lens artifacts and user-provided brainstorming notes.
- No implementation code survey was performed in this product brief pass; technical surfaces are planning hypotheses from the research artifact.
- No stakeholder interviews were conducted beyond the provided brainstorm and feature metadata.
- The current feature is intentionally blocked by the legacy gate, so later PRD and architecture work must distinguish desired target behavior from current conductor behavior.