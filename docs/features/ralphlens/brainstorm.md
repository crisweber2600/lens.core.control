---
feature: ralphlens
feature_id: ralphlens
doc_type: brainstorm
status: draft
title: "Workflow-first brainstorm for RalphLens"
selected_approach: workflow-first
techniques_used:
  - Process Mapping
  - Failure Mode Analysis
  - Constraint-Driven Design
session_topic: "Enable Ralph loop execution in Lens dev lifecycle"
session_goals:
  - "Allow lens-dev to run a Ralph loop inspired by bmalph."
  - "Preserve Lens phase gates, governance boundaries, and write-scope safety."
  - "Define a minimal, auditable workflow that can iterate safely."
source_reference:
  - "https://github.com/LarsCowe/bmalph"
updated_at: "2026-05-26"
---

# Brainstorm — Workflow-First Route

## 1) Problem Statement

`lens-dev` needs a repeatable Ralph loop capability so implementation can iterate in controlled cycles while still honoring Lens lifecycle controls.

## 2) Desired Workflow Outcome

A developer can trigger a bounded Ralph loop during dev execution, producing auditable progress without bypassing required contracts (constitution gates, artifact rules, and write boundaries).

## 3) Draft Ralph Loop Flow (Lens-Compatible)

1. **Loop Intent Capture**  
   Operator declares loop goal, scope, and stop condition.
2. **Pre-loop Contract Check**  
   Validate feature context, phase eligibility, write scope, and constitution hard gates.
3. **Iteration Plan Slice**  
   Pick one bounded objective (single story step or narrow bugfix slice).
4. **Implement + Validate**  
   Execute implementation step in target repo only, then run required checks.
5. **Evidence Update**  
   Record iteration evidence in feature docs (status, risks, decisions, outputs).
6. **Loop Decision Gate**  
   Decide: continue next slice, pause for blocker, or exit loop.
7. **Exit Contract**  
   On loop completion, ensure artifacts and lifecycle state remain consistent.

## 4) Required Guardrails

- No direct governance writes.
- No release-clone writes.
- Target-repo implementation only for code mutations.
- Pre-loop and per-iteration constitution checks must pass before execution.
- Each loop iteration must be explicitly bounded (objective + done condition).
- Loop cannot auto-advance lifecycle phase.

## 5) Integration Points

- **Feature Context**: resolve from `feature.yaml` authority.
- **Lifecycle State**: read-only gating before each iteration; updates only through approved lifecycle operations.
- **Validation**: run existing phase and artifact validators as guardrails.
- **Evidence**: append structured loop outcomes into feature docs to prevent context loss.

## 6) Candidate Command Surface

- `lens-dev --ralph-loop` as an execution mode
- optional controls:
  - `--goal "..."`
  - `--max-iterations N`
  - `--stop-on-risk high`
  - `--evidence-doc docs/features/ralphlens/work.md`

(Exact command shape should be finalized during architecture/techplan alignment.)

## 7) Failure Modes and Mitigations

- **Mode drift** (loop becomes open-ended)  
  Mitigation: mandatory max-iteration or explicit stop condition.
- **Gate bypass**  
  Mitigation: force pre-loop and per-iteration checks with hard stop on fail.
- **Artifact lag** (work done but not documented)  
  Mitigation: evidence write required before next iteration.
- **Cross-repo confusion**  
  Mitigation: show resolved write scope at loop start and reject out-of-scope writes.

## 8) MVP Recommendation

Implement a minimal Ralph loop mode in `lens-dev` with:

1. feature-context resolution,
2. pre-loop hard-gate validation,
3. single-objective iteration cycle,
4. per-iteration evidence capture,
5. bounded continuation decision,
6. safe exit contract.

## 9) Inputs for Next PrePlan Artifacts

Carry these decisions into research and product brief:

- Ralph loop is a **bounded dev-iteration mechanism**, not a lifecycle-phase override.
- Loop safety depends on **contract checks at start and each iteration**.
- Evidence capture is first-class to avoid feature-pocket context loss.
- Command UX should prioritize explicit scope and stop conditions.
