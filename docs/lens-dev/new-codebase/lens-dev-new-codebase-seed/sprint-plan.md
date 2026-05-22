---
feature: lens-dev-new-codebase-seed
doc_type: sprint-plan
status: draft
track: express
title: "lens-new-seed sprint plan"
updated_at: 2026-05-22T15:55:00Z
---

# lens-new-seed Sprint Plan

## Sprint Goal

Make the Lens branching workflow flat for new work while preserving compatibility for existing three-branch features, and establish the Auspex seed contracts that depend on metadata-based artifact validity.

## Work Slices

### Slice 1 - Topology Configuration

- Add explicit support for `control_topology: flat`.
- Keep `3-branch` as a supported legacy value.
- Update configuration tests to prove both values load.

Acceptance:
- `lens_config` accepts `flat`.
- default Lens Work config uses `flat`.
- invalid topology values still fail.

### Slice 2 - Flat Feature Branch Creation

- Update `create-feature-branches` to branch by topology.
- In flat mode, create/push only `{featureId}`.
- Return `plan_branch` and `dev_branch` as compatibility aliases pointing at `{featureId}`.
- Preserve existing `3-branch` behavior.

Acceptance:
- flat-mode branch creation test creates one branch.
- legacy branch creation tests continue to pass.

### Slice 3 - Artifact Commit Rules

- Make `commit-artifacts` topology-aware.
- In flat mode, allow commits from `{featureId}`.
- Stop treating missing `{featureId}-plan` or `{featureId}-dev` as a blocker when topology is flat.

Acceptance:
- flat-mode artifact commits work on `{featureId}`.
- wrong-branch protection remains for legacy three-branch features.

### Slice 4 - Conductor Contract Updates

- Update `lens-new-feature`, `lens-expressplan`, `lens-finalizeplan`, `lens-dev`, and `lens-complete` language so artifact validity depends on metadata, lifecycle validators, and review gates rather than branch placement.
- Keep governance main-only language intact.

Acceptance:
- contract tests reflect flat workflow.
- public wrapper paths still use installed `lens.core/` boundary paths.

### Slice 5 - Auspex Seed Follow-up Contracts

- Capture follow-up stories for stable IDs, `belongs_to`, `docs/features/`, projection rebuild, doctor/audit, Salmon, and reporting snapshots.
- Do not implement the reporting UI until flat workflow contracts are stable.

Acceptance:
- finalize planning can produce implementation stories with metadata-first artifact validity.

## Dependencies

- Governance constitution allows express planning.
- BMad Builder guidance has been consulted for `lens-work` workflow changes.
- Source writes remain under `TargetProjects/lens-dev/new-codebase/lens.core.src`.

## Risks

- Existing lifecycle code may still assume `{featureId}-plan` in hidden paths.
- Completing partially migrated features can fail if compatibility is removed too early.
- Test expectations are broad and may require multiple focused updates.

## Definition of Done

- Flat branch workflow is implemented in source and covered by focused tests.
- Existing three-branch compatibility tests remain green or are deliberately retained under legacy topology.
- ExpressPlan artifacts are committed to the Lens control feature branch.
- Required postflight passes.

