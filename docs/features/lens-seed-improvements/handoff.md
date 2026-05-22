# Handoff - Lens Seed Improvements

## Recommended Next Workflow

Topology redesign planning before `/preplan`

This work should remain on the full track, but the current `/preplan` conductor is blocked until the domain/service-free topology path is designed or a sanctioned compatibility route exists.

## Carry Forward Context

- Raw request: "lens seed improvments".
- Feature ID: `lens-seed-improvements`.
- Current phase: `preplan`.
- Current status: `intake-blocked`.
- Related work: `nextlens-src-topdownlens`.
- Likely background: TopDownLens seed flow from raw seed to exploration, focused slice, capability mapping, and follow-on seed discovery.
- Operator decision: do not place this work under a legacy domain/service placeholder just to satisfy current PrePlan gates.

## Blocking Evidence

- `lens-init-feature fetch-context --feature-id lens-seed-improvements` failed because the feature is not in governance `feature-index.yaml`.
- `lens-constitution progressive-display` failed because the script requires `--domain` and `--service`.
- This work intentionally tests the proposed two-tree model where feature archives can exist under `docs/features/` without the old domain/service/feature identity construct.

## Inputs Needed By PrePlan

- The seed workflow entry point that needs improvement.
- Examples of current behavior and desired behavior.
- Whether the improvement is CLI behavior, prompt behavior, archive structure, schema, UX copy, or lifecycle routing.
- Ownership decision for `belongs_to` and `target_repos`.
- A sanctioned PrePlan entry path for domain/service-free feature archives.

## Open Questions

- What specific seed action or artifact should improve?
- Is this about NextLens bottom-up seed creation, current Lens workflow commands, or another seed concept?
- Should the typo-normalized feature ID `lens-seed-improvements` be kept?
- Is there enough known scope to switch from full track to express after preplan?
- Should constitution resolution become topology-aware with optional service/domain/program scopes, or should it have a temporary compatibility adapter for feature-only archives?

## Handoff Notes

- Do not rely on hidden chat history; use this archive as the source memory.
- Do not implement during preplan.
- Do not update living ledgers until completion and promotion evidence exists.
