---
stepsCompleted: [1, 2, 3, 4]
inputDocuments: ["user-provided brainstorming session notes"]
session_topic: "Re-examining Lens/BMAD project artifact topology for organic, multi-feature, team-scale work"
session_goals:
  - "Discover alternatives and augmentations that solve knowledge-scatter and feature-pocket-universe problems."
selected_approach: "progressive-flow"
techniques_used:
  - "First Principles Thinking"
  - "Morphological Analysis"
  - "Concept Blending"
  - "Solution Matrix"
ideas_generated:
  - "Replace path-as-identity with stable IDs and explicit belongs_to parent references."
  - "Supersede the fixed domain > service > feature construct with a two-tree model."
  - "Keep features as immutable delivery facts under docs/features while treating the landscape as an interpretation."
  - "Use reorganizable service, domain, and program ledgers for accumulated knowledge."
  - "Derive the governance map from frontmatter instead of hand-authoring it as source truth."
  - "Replace planning branch dependence with explicit draft and published metadata."
  - "Add a projection rebuild command and lightweight topology doctor to expose orphans and inconsistencies."
  - "Treat Salmon as a first-class upstream-impact signal and recursive consistency-check workflow."
  - "Use Auspex MVP1 reporting UI as an adjacent consumer of topology and reporting needs."
context_file: ""
feature_id: lens-seed-improvements
doc_type: brainstorm
status: draft
---

# Brainstorming Session Results

## Session Overview

This completed brainstorming session re-examined the Lens/BMAD artifact topology for organic, multi-feature, team-scale work. The session focused on removing or superseding the old fixed domain > service > feature construct because it traps durable knowledge inside feature folders and makes sibling or predecessor context hard to find during authoring.

The primary goals were to solve two failure modes: accumulated service and domain truth scattered across feature folders or branches, and cross-feature dependency context trapped in isolated feature pockets at authoring time. Auspex MVP1 reporting UI was captured as adjacent context: it is a stakeholder-facing reporting example and a likely consumer of improved topology and reporting needs, not the primary brainstorm goal.

## Problem Framing

The current topology over-identifies feature folders and planning branches as knowledge boundaries. That makes delivery work easy to isolate, but it makes durable knowledge hard to consolidate after features complete or evolve.

The problem to remove or supersede is the assumption that domain > service > feature is the natural and fixed hierarchy for all knowledge. Features are delivery facts, but service, domain, and program knowledge need to remain findable, trustworthy, composable, owned, and useful for humans long after any one feature is finished.

## First Principles Findings

The session found that useful project knowledge must be findable, trustworthy, composable, owned, durable after feature completion, and consolidated for humans. These principles pushed against several false assumptions: that a feature is the right unit of knowledge ownership, that planning branches are a good isolation boundary for docs, that features simply complete and close, that each feature is self-contained, and that contributors already know all dependencies.

Features are immutable facts; the landscape is an interpretation; the map is a cache. This distinction lets feature evidence remain stable while human-facing ledgers reorganize as the product understanding matures.

## Morphological Analysis Outcomes

The session separated topology dimensions that had been conflated:

- Delivery/WIP: feature-scoped work and temporary authoring context.
- Accumulated technical truth: service ledgers.
- User journey and cross-service capability: domain ledgers.
- Cross-domain assembly: program or product ledgers.
- Query and routing layer: derived governance projection.
- Consistency workflow: Salmon upstream-impact signaling and recursive review.

The preferred architecture uses a multi-layer model: Feature for delivery and WIP, Service for accumulated technical truth, Domain for user journey and cross-service capability, and Program/Product for cross-domain assembly.

## Candidate Direction: Two-Tree Model with Derived Map

The highest-scoring direction was the Two-Tree Model with Derived Map. It scored 24 in the solution matrix, ahead of the current model, governance-only docs, and a pure graph.

In this model, docs/features is a permanent flat archive. Features live permanently under docs/features/ and never move. Service, domain, and program knowledge lives in reorganizable landscape ledgers under docs/<landscape>, which may be arranged top-down as service, domain/service, or program/domain/service depending on the maturity of the work.

Stable IDs and belongs_to parent references replace path-as-identity. Optional additive depth allows a quick script to grow into service, domain, and program structures without forcing a mature operating model too early.

## Governance Map and Projection

The governance map is derived from frontmatter and is never hand-authored source truth. The map is a cache, not an authority.

The projection rebuild command should scan features and landscape ledgers, rebuild an ID-to-path index and ownership graph, cross-validate parent and child declarations, and report orphans, inconsistencies, and drift. Minimum metadata examples include feature stable id, kind, status, belongs_to, and docs_path, plus service/domain/program id, kind, belongs_to, features, and ledger_path.

This approach makes the feature archive durable while allowing human-facing topology to evolve through explicit metadata instead of file movement.

## Salmon Workflow

Salmon is a first-class upstream-impact signal and recursive consistency-check workflow. An upstream note should trigger a recursive consistency check upward and downward through the topology.

Salmon should be advisory by default and block only when discovered impact is material. This keeps it useful for surfacing cross-feature, service, domain, and program impacts without turning every small note into heavyweight governance.

## Solution Matrix

The Two-Tree Model with Derived Map scored highest because it preserves immutable feature evidence, supports reorganizable human knowledge ledgers, removes path identity, and allows a derived governance view to expose inconsistency.

Other options were rejected or deprioritized:

- Current model: preserves existing behavior but keeps knowledge scattered and feature-bound.
- Governance-only human docs: centralizes some truth but risks bypassing local authoring context and creating another manually maintained source of truth.
- External docs or hybrid scratch spaces: increase fragmentation and weaken auditability.
- Pure graph: powerful long term but too abstract and heavy for the immediate MVP.

## MVP Recommendation

The recommended MVP sequence is:

1. Add stable IDs and belongs_to parent references.
2. Add a projection rebuild command.
3. Put new work under docs/features.
4. Create one pilot ledger.
5. Add a lightweight lens-doctor check.
6. Add Salmon later.
7. Expand domain and program ledgers as needed.

This sequence deliberately avoids perfecting the mature operating model first. It builds the topology that matches current work evolution and leaves room for stronger controls once the patterns prove themselves.

## Risks and Mitigations

Dual truth drift is the main risk. Mitigate it by treating feature frontmatter and ledgers as source inputs while treating the governance map as a derived projection that can be rebuilt and checked.

Projection drift is another risk. Mitigate it with a rebuild command, topology doctor, and audit checks that report stale cache state, orphaned IDs, and mismatched parent declarations.

Over-modeling is a risk if the topology tries to encode every future relationship too early. Mitigate it by starting with stable IDs, belongs_to, docs/features, and one pilot ledger, then adding depth only when the work demands it.

Salmon overload is a risk if every upstream note blocks work. Mitigate it by making Salmon advisory by default and blocking only material discovered impacts.

Migration fatigue is a risk. Mitigate it through incremental adoption: new work first, one pilot ledger, then targeted migration of high-value existing knowledge.

## Strategic Recommendation

Do not perfect the mature operating model first. Build the smallest topology shift that reflects how Lens work is already evolving: immutable feature facts, reorganizable landscape knowledge, and a derived governance projection.

The strategic direction is to stop path identity, introduce first-class service/domain/program entities, add a projection rebuild command, add a topology doctor or audit surface, replace planning branch assumptions with explicit draft and published metadata, and introduce Salmon as the upstream-impact workflow once the basic topology is stable.

Planning branch dependence should be removed or replaced by explicit draft/published metadata. This keeps authoring state visible without making branch topology the source of truth for planning status.

## Handoff Notes

This artifact should be treated as the durable brainstorming source for preplan follow-up work. The next planning artifacts should carry forward these core decisions:

- The old fixed domain > service > feature construct is the problem to remove or supersede.
- Features are immutable facts; the landscape is an interpretation; the map is a cache.
- Features live permanently under docs/features/ and never move.
- Service/domain/program knowledge lives in reorganizable landscape ledgers.
- The governance map is derived from frontmatter and is never hand-authored source truth.
- Planning branch dependence should be removed or replaced by explicit draft/published metadata.
- Stable IDs and belongs_to parent refs must replace path-as-identity.
- Salmon is a first-class upstream-impact signal and recursive consistency-check workflow.
- The MVP sequence is stable IDs plus parent refs, projection rebuild command, docs/features for new work, one pilot ledger, lightweight lens-doctor, Salmon later, and expanded domain/program ledgers as needed.

Auspex MVP1 reporting UI remains adjacent context. It is useful as an example consumer of stakeholder reporting and topology projection, but it should not redefine the primary topology problem.