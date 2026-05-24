---
feature: goal-based-development
doc_type: product-brief
status: approved
phase: preplan
goal: "Define a Lens capability for durable, evidence-backed goal traceability across the development lifecycle."
key_decisions:
  - "Use a hybrid goal thread rooted in the feature archive."
  - "Make goal refinement explicit and reviewable."
  - "Defer hard validation gates until BusinessPlan and TechPlan define exact artifact changes."
open_questions:
  - "What exact metadata schema should represent goals?"
  - "Which lifecycle phase first requires stable goal IDs?"
  - "How should reports display goal progress without overclaiming draft state?"
depends_on:
  - brainstorm.md
  - research.md
blocks: []
updated_at: '2026-05-24T12:08:12Z'
---

# Product Brief - Goal Based Development

## Problem

Lens work can start from a clear user goal, but the goal can fade as planning documents, stories, implementation tasks, and validation evidence accumulate. This creates a risk that teams complete artifacts and code changes without proving the original intended outcome was satisfied.

## Product Goal

Create a goal based development capability that makes goals durable, traceable, refinable, and evidence-backed from intake through completion.

## Users

- Operators who intake work and need raw intent preserved.
- Analysts and planners who refine goals into requirements.
- Developers who need to understand why a story matters.
- Reviewers who need to detect goal mismatch or insufficient evidence.
- Stakeholders who need completion reporting tied to outcomes.

## Desired Outcomes

- Every meaningful work unit can preserve raw intent and refined goals in files.
- Planning can intentionally refine, split, defer, or reject goals.
- Stories can cite the goals they advance.
- Validation evidence can be mapped to goal satisfaction.
- Completion review can identify satisfied, partial, deferred, or invalidated goals.
- Future workers can understand the goal thread without chat history.

## Scope

### In Scope

- Goal model discovery and schema proposal.
- Lifecycle artifact touchpoints from intake through completion.
- Story and dev handoff expectations.
- Review and reporting implications.
- Boundary-safe persistence in feature archives before governance promotion.

### Out Of Scope

- Direct implementation during PrePlan.
- Direct governance mirror edits.
- Generated projection changes before promotion design.
- Target repository assignment before technical planning.

## Proposed Capability

Use a hybrid goal thread:

1. The feature archive records raw intent, refined goals, assumptions, and goal decisions.
2. Planning artifacts reference goal IDs and explain refinements.
3. Stories cite goal IDs and define evidence expectations.
4. Dev execution records validation evidence against those goals.
5. Completion review verifies whether each goal is satisfied, partially satisfied, deferred, or invalidated.

## Success Metrics

- A future worker can identify the current approved goals for a feature in under one minute.
- Each implementation story can name at least one goal it advances or explicitly justify why it is enabling work.
- Completion evidence maps to every accepted goal.
- Adversarial review can detect goal drift or evidence gaps.
- Governance promotion can extract durable goal learnings without relying on chat transcripts.

## Key Requirements To Explore Next

- Define the minimum goal metadata fields.
- Decide whether goals live in `feature.yaml`, `work.md`, a dedicated goals artifact, or a combination.
- Determine how goal IDs flow into PRD, UX, architecture, stories, sprint status, dev session, and review artifacts.
- Decide which validations are advisory versus hard gates.
- Define how goal changes are approved and recorded.

## Recommended Next Phase

Proceed to BusinessPlan after PrePlan review. BusinessPlan should produce a PRD and UX/design workflow for how operators and downstream workers view, refine, and validate the goal thread.
