# Journey - Goal Based Development

## Intake

- Capture the raw request as a durable work archive.
- Preserve the current uncertainty about ownership, target repositories, relationships, and exact workflow scope.
- Route the feature to the full lifecycle track at `preplan`.

## Discovery

- Use `/preplan` to clarify the user problem, affected roles, current workflow gaps, and the intended goal model.
- Produce or update discovery artifacts such as brainstorm, research, product brief, and adversarial review as required by the lifecycle contract.

## Business Planning

- Use `/businessplan` if PrePlan confirms a product or workflow change.
- Define the user-facing behavior, UX expectations, acceptance criteria, and success measures for goal capture and goal traceability.

## Technical Planning

- Use `/techplan` after business planning to identify artifact schemas, lifecycle command changes, validation hooks, and target repositories.
- Ensure any goal metadata or files have clear source-of-truth boundaries and do not duplicate living ledgers or generated projections.

## FinalizePlan

- Use `/finalizePlan` to create epics, stories, implementation-readiness evidence, sprint status, and story files.
- Stories should cite this feature archive under a Lens work-unit section and preserve the goal context they implement.

## Development

- Use `/dev` only after implementation readiness confirms target repositories and story scope.
- Track validation evidence against explicit goals and approved refinements.

## Review And Completion

- Record review status, changed files, validation evidence, and completion notes in this archive.
- Run `lens-preflight`, `lens-map-audit`, `lens-ledger-promotion`, `lens-salmon-impact` if upstream assumptions changed, and `lens-reporting-snapshot` when the work is complete.
