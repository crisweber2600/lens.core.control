---
feature: goal-based-development
doc_type: research
status: approved
phase: preplan
goal: "Ground goal based development in Lens lifecycle needs, traceability patterns, and implementation risks."
key_decisions:
  - "Favor a lightweight goal thread over a standalone artifact until lifecycle impact is proven."
  - "Treat goal drift as an explicit decision that must be recorded."
  - "Use validation evidence as the measurable endpoint for each goal."
open_questions:
  - "Should goal IDs be mandatory before FinalizePlan?"
  - "Should goal satisfaction be a hard gate or an adversarial-review criterion first?"
  - "Which existing validators should own goal trace checks?"
depends_on:
  - brainstorm.md
blocks: []
updated_at: '2026-05-24T12:08:12Z'
---

# Research - Goal Based Development

## Research Question

How should Lens support development that begins with explicit goals, preserves goal intent through lifecycle artifacts, and validates completion against evidence?

## Source Context

- Intake archive: `docs/features/goal-based-development/`
- Raw intent: "goal based development"
- Current lifecycle track: `full`
- Current phase: `preplan`
- Constitution context: org-only rules resolved; review and stories are enforced, with informational gate mode.
- Existing related archive scan found no direct goal-based development feature.

## Findings

### 1. The Core Problem Is Trace Loss

Lens already serializes work context into feature archives, planning artifacts, stories, reviews, and completion evidence. The risk is that the original goal can become implicit after handoffs. When goals are not explicit, later workers can complete tasks while missing the outcome the user cared about.

Implication: goal based development should emphasize durable traceability over a new methodology label.

### 2. Goal Refinement Must Be Auditable

Planning often improves the user's raw goal. Refinement is useful, but silent replacement is dangerous. The archive should preserve raw intent, refined goals, and the reason for each refinement.

Implication: goals need state transitions such as proposed, accepted, refined, deferred, satisfied, invalidated, or replaced.

### 3. Stories Need Goal Context, But Not Goal Ownership

Stories are execution slices. They should cite the goals they advance and their evidence expectations, but the feature archive should remain the durable root of goal truth.

Implication: story templates should reference goal IDs rather than redefining goals in each story.

### 4. Evidence Is The Natural Completion Boundary

Goal based development becomes actionable only when each goal has observable evidence. Evidence may be automated tests, manual workflow checks, review findings, docs updates, user acceptance, or reporting snapshots.

Implication: acceptance criteria should include evidence links, not just task lists.

### 5. Governance Publication Should Remain Boundary-Safe

Goal records created during planning are draft control-repo artifacts. Durable knowledge should move into living ledgers only through promotion after completion.

Implication: goal based development should not introduce direct governance writes or generated projection edits.

## Candidate Lifecycle Touchpoints

| Phase | Goal Responsibility |
| --- | --- |
| Intake | Capture raw goal and initial assumptions. |
| PrePlan | Refine goals, identify users, define evidence candidates. |
| BusinessPlan | Convert goals into product requirements and UX outcomes. |
| TechPlan | Map goals to architecture constraints and validation strategy. |
| FinalizePlan | Link goals to epics, stories, readiness, and sprint status. |
| Dev | Execute stories with goal context and collect evidence. |
| Complete | Verify goals against evidence and record unresolved deltas. |
| Promotion | Move completed, durable goal learnings into ledgers. |

## Risks

- Adding too much ceremony could slow small changes.
- Goal IDs could become stale if refinement is not easy.
- Hard gates too early could block discovery work.
- Too much goal data in every artifact could create duplication.
- Goal satisfaction may be subjective unless evidence is specific.

## Recommendations

1. Start with a hybrid goal thread rooted in the feature archive.
2. Require explicit goal references in product brief, stories, validation evidence, and completion review before making goal tracing a hard validator.
3. Preserve raw intent and refined goals separately.
4. Treat goal drift as a decision with rationale.
5. Define goal satisfaction as evidence-backed, not assertion-backed.

## Research Conclusion

Goal based development should be introduced as lifecycle traceability: stable goals originate in the work archive, are refined in planning, are linked to stories, and are closed only when evidence shows the intended outcome was achieved.
