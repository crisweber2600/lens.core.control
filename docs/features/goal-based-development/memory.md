# Memory - Goal Based Development

## Raw Intent

- User request on 2026-05-24: "goal based development".

## Decisions

- Created a new durable work archive because no existing feature archive matched the request.
- Normalized the feature ID to `goal-based-development`.
- Selected the full lifecycle track because the request is broad, underspecified, and not yet implementation-ready.
- Left `belongs_to`, relationships, and `target_repos` unresolved rather than guessing ownership.
- Recorded `lens_preflight_status: passed` because Lens preflight was run successfully immediately before intake in this session.

## Related Context

- Existing archive scan found only `docs/features/lens-seed-improvements/`; no direct `goal based development` matches were found in `docs/`.
- This work may later relate to seed improvements, work intake, lifecycle routing, story creation, or development validation, but those relationships require discovery before being recorded as stable links.

## Assumptions

- The request concerns Lens/NextLens development workflow behavior, not a generic essay or unrelated implementation task.
- The work should begin with planning because the desired capability and target surface are not yet defined.

## Open Loops

- Define what goal based development means for the intended users.
- Identify where goals should be captured and how they should flow through PrePlan, planning, stories, Dev, review, completion, audit, and reporting.
- Confirm whether goals should become a new artifact, a metadata field, a story convention, a lifecycle gate, or a combination.
- Confirm the owning Lens area and target repositories.
- Confirm whether this remains full-track or can be converted to express after scope is clarified.

## Discarded Options

- Did not create implementation stories during intake.
- Did not attach this work to `lens-seed-improvements` without evidence.
- Did not assign a target repository without ownership discovery.
