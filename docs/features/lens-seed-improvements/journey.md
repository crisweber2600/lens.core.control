# Journey - Lens Seed Improvements

## Intake

- Capture the raw seed-improvement request as a durable feature archive.
- Preserve likely related TopDownLens seed-flow context.
- Route to preplan for discovery and scope clarification.

## Discovery

- Clarify the concrete seed workflow pain points.
- Identify users, entry points, expected outputs, failure modes, and examples.
- Decide whether existing TopDownLens seed notes are the intended source model.

## Product And UX Planning

- Produce product requirements only after the seed workflow outcome is clear.
- Define any CLI, prompt, archive, schema, or reporting behavior from user journeys rather than assumptions.

## Architecture Planning

- Identify the owning module and target repositories.
- Define schema, command, prompt, and projection impacts if seed behavior changes persistent artifacts.
- Record compatibility constraints for existing Lens features.

## Finalize And Dev Handoff

- Convert approved planning into epics, stories, readiness evidence, and sprint status.
- Keep story scope bounded to seed-flow improvements that have explicit acceptance evidence.

## Development And Review

- Implement only after the owning target repo and stories are known.
- Validate with focused command, schema, and regression tests for the touched seed workflow.

## Completion

- Record changed files, validation evidence, review outcome, and completion notes.
- Run map audit, ledger promotion, Salmon impact review, and reporting snapshot only if completion evidence requires them.
