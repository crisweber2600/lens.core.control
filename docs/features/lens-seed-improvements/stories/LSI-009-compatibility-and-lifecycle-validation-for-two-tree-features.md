---
feature: lens-seed-improvements
story_id: LSI-009
doc_type: story
status: done
title: "Compatibility And Lifecycle Validation For Two-Tree Features"
depends_on:
  - LSI-003
  - LSI-004
  - LSI-005
  - LSI-007
updated_at: "2026-05-23T15:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Module Registration And Two-Tree Lifecycle Compatibility"
priority: P0
acceptance_gate: "Lifecycle fixture tests pass for two-tree records, compatibility reads, branch/status separation, target repo readiness, and waiver visibility."
---

# LSI-009: Compatibility And Lifecycle Validation For Two-Tree Features

## Summary

Update lifecycle validation so new two-tree features use stable metadata and waivers, while legacy topology remains readable for context but does not govern new writes.

## Context

The two-tree model replaces path identity with stable metadata. New features under `docs/features/<feature_id>` must validate through source fields such as `stable_id`, `entity_type`, `docs_path`, lifecycle phase, track, publication state, parentage, and target repos. Branch names and folder locations must not become lifecycle authority.

This story depends on LSI-007 for the closeout rule: unresolved `belongs_to: unknown` must block Dev completion unless the pilot `service:lens-workbench` relationship is verified or a complete reviewed exception remains visible.

## Scope

- Validate `stable_id`, `entity_type`, `docs_path`, `target_repos`, `phase`, `track`, `publication_state`, `belongs_to`, and waiver metadata for two-tree features.
- Supersede legacy domain/service write requirements for new two-tree work without removing compatibility reads.
- Ensure branch names and folder locations are not treated as authoritative lifecycle status.
- Make waiver visibility available to lifecycle gates and Dev completion checks.
- Preserve target repo readiness checks before implementation handoff.

## Out Of Scope

- Broad historical feature migration.
- Requiring new two-tree writes to move into legacy domain/service folders.
- Direct governance writes or publication mirror edits.
- Implementing module registration validation already owned by LSI-008.

## Acceptance Criteria

1. Given a new two-tree feature under `docs/features/<feature_id>` with stable metadata, when lifecycle validation runs, then it does not require domain/service folder placement.
2. Given legacy topology artifacts exist, when compatibility discovery runs, then they can be read for context but are labeled as compatibility inputs rather than required write targets.
3. Given branch location differs from recorded phase metadata, when lifecycle validation runs, then recorded lifecycle metadata is used as authority and the branch mismatch is reported separately if needed.
4. Given `target_repos` is missing before Dev handoff, when lifecycle validation runs, then it reports a blocker for implementation readiness.
5. Given `belongs_to: unknown` has a complete temporary waiver, when FinalizePlan validation runs, then the waiver is visible and accepted only through its configured review point.
6. Given Dev completion is requested, when `belongs_to: unknown` remains unresolved and the pilot ledger relationship is not verified, then lifecycle validation reports a blocker.

## Implementation Notes

- Lifecycle validation should consume shared parser, resolver, doctor, projection, and promotion evidence where available instead of reparsing independently.
- New write authority is stable metadata and `docs_path`, not legacy domain/service folder placement.
- Compatibility reads are allowed for context only; label them clearly so they do not become required write targets.
- Branch mismatch reporting should be separate from lifecycle metadata authority.
- The Dev completion closeout note from LSI-007 is mandatory: verify `service:lens-workbench` relationship or preserve a complete reviewed exception.
- Target repo readiness should confirm `TargetProjects/lens-dev/new-codebase/lens.core.src` is present in feature metadata before implementation writes begin.

## Validation

- Lifecycle fixture tests cover new two-tree records, legacy compatibility reads, branch/status separation, target repo readiness, waiver visibility, waiver review-point enforcement, and Dev-completion blockers.
- Tests prove unresolved `belongs_to: unknown` blocks Dev completion unless the pilot `service:lens-workbench` relationship or reviewed exception is visible.
- Tests assert lifecycle validation does not write governance mirrors or generated projection output.

## Dependencies

- Depends on LSI-003, LSI-004, LSI-005, and LSI-007.
- Blocks LSI-010.

## Dev Agent Record

- Added lifecycle validation for two-tree records, compatibility source kind, branch/phase separation warnings, target repo readiness, and waiver visibility.
- Dev-completion checks accept the pilot `service:lens-workbench` ledger relationship or a complete reviewed exception.
- Validation: `uv run python -m pytest -q` passed in `TargetProjects/lens-dev/new-codebase/lens.core.src`.
