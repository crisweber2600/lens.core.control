---
feature: goal-based-development
doc_type: adversarial-review
phase: preplan
source: phase-complete
verdict: pass-with-warnings
status: responses-recorded
review_format: abc-choice-v1
counts:
  critical: 0
  high: 1
  medium: 3
  low: 1
updated_at: '2026-05-24T12:08:12Z'
---

# Adversarial Review: goal-based-development / preplan

**Reviewed:** 2026-05-24T12:08:12Z
**Source:** phase-complete
**Overall Rating:** pass-with-warnings

## Summary

The PrePlan package is reviewable and coherent: it identifies trace loss as the core problem, proposes a hybrid goal thread rooted in the feature archive, and defers implementation until BusinessPlan and TechPlan define exact artifact and validator changes. The phase can move forward, but the next phase must tighten user interaction, schema ownership, and rollout boundaries before goal traceability becomes a lifecycle gate.

## Findings

### Critical

| # | Dimension | Finding | Recommendation |
|---|-----------|---------|----------------|
| - | - | No critical findings. Required PrePlan artifacts are present and no blocker prevents BusinessPlan. | Continue with documented warnings. |

### High

| # | Dimension | Finding | Recommendation |
|---|-----------|---------|----------------|
| H1 | Coverage Gaps | The package recommends a hybrid goal thread but does not yet define the user-visible workflow for creating, refining, accepting, or rejecting goals. Without that, BusinessPlan could over-focus on metadata and under-specify the operator/reviewer experience. | BusinessPlan PRD and UX design must define the goal lifecycle from the user's perspective, including raw-goal capture, refinement approval, goal drift handling, and completion evidence review. |

### Medium / Low

| # | Severity | Dimension | Finding | Recommendation |
|---|----------|-----------|---------|----------------|
| M1 | Medium | Logic Flaws | The artifacts state that goals should have stable IDs but also recommend avoiding a new required artifact until proven necessary. That tension is acceptable for PrePlan, but unresolved ownership could fragment goal truth across `feature.yaml`, `work.md`, stories, and reviews. | BusinessPlan should identify the canonical goal source and TechPlan should decide the storage/schema boundary. |
| M2 | Medium | Complexity and Risk | The scope spans intake, planning, story generation, dev sessions, validation, reviews, reporting, promotion, and Salmon impact. This is larger than a single small workflow tweak. | TechPlan should sequence the implementation into thin, independently testable lifecycle slices and define which command changes are first. |
| M3 | Medium | Assumptions and Blind Spots | The package assumes evidence mapping can become useful without immediately becoming burdensome. The minimum viable evidence standard is not yet defined. | BusinessPlan should define "good enough" evidence for early adoption and when missing evidence is advisory versus blocking. |
| L1 | Low | Cross-Feature Dependencies | No related work is confirmed, but overlap with work intake, seed improvements, story templates, and reporting is plausible. | Fetch related context again during BusinessPlan and record only confirmed relationships. |

## Accepted Risks

- PrePlan intentionally leaves target repositories unresolved until BusinessPlan and TechPlan clarify whether the first change belongs in lifecycle orchestration, work intake, story generation, dev-session tracking, reporting, or documentation.
- PrePlan intentionally treats goal traceability as an emerging model, not a hard gate yet, to avoid prematurely blocking discovery and lightweight work.

## Party-Mode Challenge

Mary (Business Analyst): The treasure is visible, but the map is incomplete. You have a compelling "goal thread" concept, yet BusinessPlan must discover the actual interaction moments: who writes the first goal, who approves refinements, and what happens when a reviewer disagrees that evidence satisfies the goal.

Winston (Architect): The risk is schema sprawl. If every artifact gains its own goal fields without a canonical root and migration rule, the system will accumulate contradictory goal states. TechPlan needs a clear source of truth and validator boundary before implementation.

Quinn (QA): Evidence-backed completion is only meaningful if evidence is falsifiable. "Goal satisfied" cannot be a checkbox; it needs observable checks, reviewer prompts, and failure cases for partial or invalidated goals.

## Gaps You May Not Have Considered

1. How goal IDs are created, renamed, merged, or retired without breaking story and review links.
2. Whether goal satisfaction should be reviewer-entered, validator-derived, or both.
3. How to handle enabling stories that support infrastructure for goals but do not directly satisfy a user-facing goal.
4. How reporting should show draft goal progress without turning draft goals into governance commitments.
5. Whether small express-track work should get a lighter goal model than full-track work.

## Open Questions Surfaced

- What is the canonical source of goal truth: `feature.yaml`, `work.md`, a dedicated goal artifact, or a hybrid with explicit precedence?
- Which user role can refine or accept goal changes at each lifecycle phase?
- What fields make up the minimum goal schema?
- Should missing goal evidence be advisory at first, or a hard gate before phase completion?
- Which lifecycle command should be changed first to prove the workflow with minimal blast radius?
