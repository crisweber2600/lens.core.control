---
feature: lens-seed-improvements
story_id: LSI-010
doc_type: story
status: done
title: "Release Validation Or VM-Style Module Validation For lens.core.src"
depends_on:
  - LSI-001
  - LSI-002
  - LSI-003
  - LSI-004
  - LSI-005
  - LSI-006
  - LSI-007
  - LSI-008
  - LSI-009
updated_at: "2026-05-23T15:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Release Validation For lens.core.src"
priority: P0
acceptance_gate: "Local release validation or VM-style transcript covers parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, module validation, projection drift, and waiver closure."
---

# LSI-010: Release Validation Or VM-Style Module Validation For lens.core.src

## Summary

Provide a local release validation flow for the Lens module source tree that verifies command contracts, module structure, tests, and temporary waiver closure before Dev completion.

## Context

This story closes the sprint by proving the implemented Lens module source tree is release-ready. It must run or document validation for parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, module assets, projection drift, and topology waiver closure.

Module Builder validation remains a release gate from LSI-008. The temporary `belongs_to: unknown` waiver remains a closeout gate from LSI-007 and LSI-009.

## Scope

- Add or document a local validation command or VM-style validation procedure for `lens.core.src`.
- Run parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, and module asset validations together.
- Validate that release artifacts stay inside `lens.core.src` and generated projection output is produced only by approved commands.
- Fail validation when the pilot `service:lens-workbench` relationship or reviewed waiver exception is not present.
- Produce a release validation report or transcript naming commands run, fixtures covered, paths checked, module assets checked, and intentional deferrals.

## Out Of Scope

- Implementing earlier story behavior that validation should consume.
- Publishing to governance or hand-copying release artifacts.
- Broad historical migration or full workbench UI validation.
- Treating skipped checks as passing without explicit unavailable or deferred status.

## Acceptance Criteria

1. Given a clean `lens.core.src` checkout, when release validation runs, then parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, and module validation checks are all invoked or explicitly reported as unavailable.
2. Given module assets are incomplete, when validation runs, then missing module files, orphan help entries, duplicate menu codes, broken references, inaccurate descriptions, missing capabilities, and weak entries fail or warn according to policy.
3. Given projection output is stale, when validation runs in check mode, then it reports drift and does not write generated cache files.
4. Given a SKILL body contains implementation detail that should live in scripts, assets, references, or prompt stages, when module validation runs, then the result identifies progressive disclosure risk.
5. Given `feature:lens-seed-improvements` still has `belongs_to: unknown`, when release validation runs, then validation fails unless `service:lens-workbench` is verified as the pilot ledger relationship or a complete reviewed exception remains active.
6. Given validation completes successfully, when Dev handoff is prepared, then the report names the commands run, fixtures covered, output paths checked, module assets checked, and any intentional deferrals such as full UI, broad historical migration, or always-blocking Salmon policy.

## Implementation Notes

- The validation flow can be a local command or a VM-style transcript, but it must be repeatable and explicit.
- Treat Module Builder validation as a release gate: `module.yaml`, `module-help.csv`, prompt references, setup anti-zombie behavior, and progressive disclosure checks must pass or report policy-defined warnings.
- Treat topology waiver closure as a release gate: validation fails if `feature:lens-seed-improvements` remains `belongs_to: unknown` without verified `service:lens-workbench` linkage or a complete reviewed exception.
- Projection validation must use check mode for drift and must not write caches while checking.
- The report should state commands run, fixtures covered, output paths checked, module assets checked, and intentional deferrals.
- Keep release artifacts and implementation edits inside `TargetProjects/lens-dev/new-codebase/lens.core.src`.

## Validation

- Run or document the local release validation command or VM-style procedure for `lens.core.src`.
- Include parser, resolver, doctor, projection, Salmon, promotion, lifecycle, setup, module validation, projection drift, and waiver closure checks.
- Include Module Builder validation results and setup anti-zombie idempotency evidence.
- Include failure behavior for unresolved `belongs_to: unknown` without verified `service:lens-workbench` relationship or reviewed exception.

## Dependencies

- Depends on LSI-001, LSI-002, LSI-003, LSI-004, LSI-005, LSI-006, LSI-007, LSI-008, and LSI-009.
- This is the release and Dev-closeout validation story for the sprint.

## Dev Agent Record

- Added repeatable release validation in `skills/lens-setup/scripts/release-validate.py`.
- Release validation covers doctor, projection check, Salmon, promotion, module asset validation, projection drift, and waiver closure using the release fixture set.
- Validation: `uv run python skills/lens-setup/scripts/release-validate.py . --feature-archive-path skills/lens-setup/assets/fixtures/seed_release --landscape-root skills/lens-setup/assets/fixtures/seed_release --reporting-output-path _bmad-output/lens --include-drafts` passed in `TargetProjects/lens-dev/new-codebase/lens.core.src`.