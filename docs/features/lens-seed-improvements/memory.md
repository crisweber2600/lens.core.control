# Memory - Lens Seed Improvements

## Raw Intent

- User request on 2026-05-22: "lens seed improvments".

## Decisions

- Created a new durable work archive because no existing `feature.yaml` archive matched the request.
- Normalized the feature ID to `lens-seed-improvements`; preserved the original spelling in raw intent.
- Selected the full lifecycle track because the request is not yet implementation-ready and needs discovery.
- Left `belongs_to` and `target_repos` unresolved rather than guessing ownership.
- User selected option 2 after PrePlan blocked: keep `lens-seed-improvements` as a domain/service-free two-tree feature archive and treat the PrePlan blocker as redesign evidence.

## Related Context

- `docs/nextlens/src/rawNotes/TopDown.md` describes a seed workflow where raw seeds are explored, focused into slices, mapped to capabilities, and used to discover follow-on seeds.
- `docs/nextlens/src/nextlens-src-topdownlens/business-plan.md` records TopDownLens as supporting bottom-up seed creation and self-hosted evolution.
- `docs/nextlens/src/nextlens-src-topdownlens/finalizeplan-review.md` carries TopDownLens delivery constraints and dogfooding concerns that may matter if seed improvements land there.

## Assumptions

- The request concerns Lens or NextLens seed workflow behavior, not unrelated data seeding.
- The work should begin with planning because the desired improvement is underspecified.

## Open Loops

- Confirm the exact user-facing seed problem to solve.
- Confirm whether the owning area is NextLens source, Lens core workflow, or another Lens service.
- Confirm whether this should remain full-track or be converted to express after scope is clarified.
- Identify target repositories after ownership is known.
- Redesign or adapt PrePlan context and constitution gates so domain/service-free feature archives can enter planning without fabricating legacy hierarchy.

## Discarded Options

- Did not create implementation stories during intake.
- Did not attach the work to a specific target repo without evidence.
- Did not route PrePlan through `domain=nextlens`, `service=src`; that would hide the structural blocker this feature is meant to expose.
