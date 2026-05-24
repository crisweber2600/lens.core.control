---
feature: goal-based-development
doc_type: brainstorm
status: approved
phase: preplan
goal: "Explore how Lens can make development explicitly goal-driven from intake through completion evidence."
key_decisions:
  - "Treat goals as durable lifecycle context, not chat-only intent."
  - "Allow goal refinement through explicit lifecycle gates instead of silent mutation."
  - "Trace goals into stories, validation evidence, completion review, and reporting."
open_questions:
  - "Which artifact owns the canonical goal model?"
  - "Which lifecycle commands must enforce goal traceability first?"
  - "What minimum evidence proves a goal was satisfied?"
depends_on: []
blocks: []
updated_at: '2026-05-24T12:08:12Z'
---

# Brainstorm - Goal Based Development

## Session Frame

The seed idea is "goal based development": a Lens workflow capability where work starts from explicit goals, carries those goals through planning and execution, and judges completion against agreed evidence instead of hidden chat memory or vague task closure.

## Analyst Framing

Goal based development should solve the current gap between a user's raw intent and the later implementation/review evidence. The workflow needs a durable goal thread that is visible in files, refined intentionally, and useful to planners, developers, reviewers, and stakeholders.

## Divergent Ideas

### Goal Capture

- Add a "goal contract" section to intake work archives.
- Represent goals as a small YAML structure with intent, user value, success signal, owner, and current confidence.
- Preserve raw goal wording separately from refined goals.
- Capture anti-goals so scope control is explicit.
- Track goal assumptions and unresolved goal questions.
- Require every lifecycle phase to state whether it refined, preserved, split, or retired each goal.
- Link goals to source signals such as user quotes, support tickets, repo findings, or strategy notes.
- Support multiple goal types: product outcome, operational outcome, technical outcome, learning outcome, compliance outcome.
- Distinguish durable goals from temporary implementation tactics.
- Give each goal a stable ID for traceability.

### Planning Flow

- PrePlan should turn raw goals into a product brief goal model.
- BusinessPlan should translate goals into user-facing requirements and acceptance criteria.
- TechPlan should map goals to architecture decisions and non-functional constraints.
- FinalizePlan should connect goals to epics, stories, readiness criteria, and sprint tracking.
- Dev should carry goal context into each story execution.
- Completion should compare evidence to the original and refined goal set.
- Adversarial reviews should ask which goals are under-evidenced or contradicted.
- `/next` should route based on the next unsatisfied goal traceability need.
- Batch intake should collect goal answers explicitly.
- Reporting snapshots should surface goal progress and remaining uncertainty.

### Developer Experience

- Story files should include a Lens work-unit section with goal IDs and evidence expectations.
- Dev sessions should record which goal a change advances.
- Validation logs should map tests, manual checks, and review notes to goals.
- Code review prompts should include goal mismatch detection.
- Developers should be warned when a story has no linked goal.
- Completion notes should identify goals satisfied, partially satisfied, deferred, or invalidated.
- Goal drift should be visible as an explicit decision.
- Goal conflicts should block planning until resolved or accepted.
- Goals should be readable without opening hidden chat transcripts.
- Dev handoffs should prioritize "why this matters" before "what to change."

### Governance And Topology

- Goal state should live in the feature archive, not governance mirrors, until published through approved routes.
- Living ledgers should receive promoted goal knowledge only after completion.
- Goal metadata should not bypass existing lifecycle phase gates.
- Goal traceability can remain draft-friendly while parentage and target repos are unknown.
- Org-level constitution rules should be included as constraints on goal satisfaction evidence.
- Salmon impact should trigger when downstream goal evidence changes upstream assumptions.
- Map audit should eventually verify goal links if they become projection-relevant.
- Goal-based reporting should not treat draft goals as published commitments.
- Generated projections should remain derived from authored goal records.
- Feature archives should remain inspectable and diffable.

### Edge Cases

- A user gives a vague goal that is actually a solution proposal.
- A goal conflicts with another active feature.
- A goal becomes invalid during research.
- A technical constraint makes the original goal impossible.
- A story satisfies implementation tasks but not the user goal.
- Multiple stories partially satisfy one goal.
- One story serves several goals with different evidence needs.
- A reviewer discovers the wrong goal was optimized.
- Goals are too many and need prioritization.
- A goal is intentionally deferred to a follow-up feature.

## Candidate Models

### Model A: Goal Contract Artifact

Create a dedicated `goals.md` or `goal-contract.md` in the feature archive. This is explicit and easy to review, but introduces another required artifact and lifecycle contract surface.

### Model B: Goal Metadata Embedded In Existing Artifacts

Add structured goal sections to intake, product brief, stories, dev session, and review artifacts. This is lower ceremony, but risks fragmentation without stable goal IDs.

### Model C: Hybrid Goal Thread

Use the intake archive as the root goal record, then require each lifecycle artifact to reference goal IDs and evidence. This keeps one source of truth while preserving phase-specific context.

## Recommended Direction

Use the hybrid goal thread. Start with goal metadata in the feature archive and product brief, then trace goal IDs through stories, validation evidence, and completion review. Avoid a new required artifact until discovery proves it is necessary.

## Initial Success Hypothesis

Goal based development succeeds when a future worker can open a feature archive, understand the approved goals, see how each plan/story maps to them, and verify completion evidence without relying on chat history.
