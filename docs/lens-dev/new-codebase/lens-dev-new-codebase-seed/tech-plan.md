---
feature: lens-dev-new-codebase-seed
doc_type: tech-plan
status: draft
track: express
title: "lens-new-seed technical plan"
updated_at: 2026-05-22T15:55:00Z
---

# lens-new-seed Technical Plan

## Architecture Direction

`lens-new-seed` has two coupled architecture slices:

1. Auspex MVP1 reporting seed: a read-only reporting layer over Lens source artifacts.
2. Lens flat workflow seed: a branch-topology overhaul that makes lifecycle validity metadata-driven instead of planning-branch-driven.

The flat workflow is the immediate enabling change in `lens.core.src`. It must preserve governance immutability, target-repo write boundaries, publication gates, and existing feature identity while replacing hard assumptions that every feature owns `{featureId}`, `{featureId}-plan`, and `{featureId}-dev` branches.

## Current Branching Problem

The source tree still advertises and enforces a `3-branch` control topology in several places:

- `bmadconfig.yaml` / `lens_config.py`
- `lens-git-orchestration` branch creation, commit, merge, and PR contracts
- `lens-new-feature`, `lens-expressplan`, `lens-finalizeplan`, `lens-dev`, and `lens-complete` conductor instructions
- regression tests expecting `{featureId}-plan` and `{featureId}-dev`

Planning branch placement is not a durable truth source for Auspex. The new truth source must be:

- `feature.yaml`
- document frontmatter and required metadata
- lifecycle validators
- governance publication history
- target repo state for implementation work

## Flat Workflow Contract

Flat control workflow means:

- One active control branch per feature: `{featureId}`.
- Planning docs, dev-cycle docs, and closeout docs are committed to `{featureId}`.
- No required `{featureId}-plan` or `{featureId}-dev` control branches for new flat features.
- Governance remains main-only and is updated through existing approved publication operations.
- Target repo implementation branching remains repo-scoped and can still use `direct-default`, `feature-id`, or `feature-id-username`.
- Existing three-branch features must remain readable and complete-able during migration.

## Implementation Surfaces

| Surface | Required change |
|---|---|
| `scripts/lens_config.py` | Allow `control_topology: flat` and expose topology helpers instead of rejecting non-`3-branch`. |
| `bmadconfig.yaml` | Change default topology to `flat`. |
| `lens-git-orchestration/scripts/git-orchestration-ops.py` | Make branch operations topology-aware. `create-feature-branches` should create only `{featureId}` in flat mode and return plan/dev aliases as the base branch for compatibility. |
| `commit-artifacts` | Accept flat feature branch as the valid docs branch and stop requiring plan/dev branches when topology is flat. |
| `merge-plan` / planning PR text | In flat mode, no plan merge is required; planning commits already live on the feature branch. |
| `lens-new-feature` docs/runtime contract | Report flat branch creation and omit deferred plan PR instructions for flat mode. |
| `lens-expressplan`, `lens-finalizeplan`, `lens-dev`, `lens-complete` | Replace branch-placement validity language with metadata/artifact validation language. |
| Tests | Add flat topology regression coverage while preserving compatibility tests for existing three-branch behavior. |

## Auspex Reporting Seed Contracts

MVP1 reporting should consume stable, read-only projection data. The seed implementation should define contracts before UI build-out:

- Feature records include stable IDs and parent `belongs_to`.
- Domain, service, and program entities have metadata homes and optional ledgers.
- A rebuild command derives topology projection from source metadata.
- A doctor command audits orphaned features, parent-child mismatches, empty ledgers, disconnected ledgers, and completed features not promoted to living ledgers.
- Salmon support records upstream-impact signals and recursive consistency findings.
- Reporting snapshots include freshness, source failure details, phase distribution, active feature counts, risks, and blockers.

The durable seed contract is captured in `auspex-contracts.md`; implementation work should keep code-facing schema names aligned with that document.

## Testing Strategy

Use focused tests around the changed branch contracts:

- `lens_config` accepts `flat` and `3-branch`.
- `create-feature-branches` in flat mode creates/pushes only `{featureId}` and reports aliases compatibly.
- `commit-artifacts` permits `{featureId}` for flat features.
- legacy `3-branch` tests continue to pass.
- conductor contract tests no longer require branch placement for artifact validity.

Given/When/Then validation for the implementation:

- Given a feature with `control_topology: flat`, when feature branches are created, then only `{featureId}` is required and plan/dev branch outputs resolve to that branch.
- Given express planning artifacts with required metadata, when artifact commit validation runs, then validity is accepted on `{featureId}` without requiring `{featureId}-plan`.
- Given a legacy `3-branch` config, when existing orchestration tests run, then old branch behavior remains intact.

## Rollout

1. Land topology helpers and config support.
2. Make `create-feature-branches` and `commit-artifacts` topology-aware.
3. Update conductor contracts and tests.
4. Add migration/audit guidance for existing features.
5. Use Auspex follow-up stories for stable IDs, derived maps, doctor, Salmon, and MVP1 reporting UI contracts.
