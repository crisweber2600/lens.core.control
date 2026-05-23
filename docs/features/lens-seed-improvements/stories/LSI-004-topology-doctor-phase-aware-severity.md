---
feature: lens-seed-improvements
story_id: LSI-004
doc_type: story
status: done
title: "Topology Doctor Phase-Aware Severity"
depends_on:
  - LSI-003
updated_at: "2026-05-23T15:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Topology Doctor And Projection Rebuild"
priority: P0
acceptance_gate: "Doctor fixture and CLI tests prove phase-aware severity and read-only behavior."
---

# LSI-004: Topology Doctor Phase-Aware Severity

## Summary

Make `lens-doctor` consume the shared inventory and resolver and emit phase-aware findings without mutating source files.

## Context

Projection write ownership and phase-aware topology severity are core controls in the two-tree model. Doctor is the read-only triage workflow consumed by projection rebuild, map audit, Salmon impact, lifecycle checks, and future CI gates.

This story must prove that `belongs_to: unknown`, waiver completeness, duplicate IDs, broken parents, projection drift, and Salmon materiality are classified consistently across lifecycle phases.

## Scope

- Wire doctor checks to shared parser and resolver output.
- Emit stable finding codes and severity values: `info`, `warning`, and `blocker`.
- Classify severity by lifecycle phase, publication state, and waiver completeness.
- Support machine-readable JSON and compact Markdown summaries.
- Preserve read-only behavior for local runs, CI, and lifecycle gate reports.

## Out Of Scope

- Rebuilding or writing projection files.
- Creating or editing ledgers or feature archives.
- Implementing Salmon clustering beyond consuming materiality-related diagnostics from shared contracts.
- Building dashboard or workbench UI.

## Acceptance Criteria

1. Given `belongs_to: unknown` during BusinessPlan, when doctor runs, then the finding is a warning and does not report blocked status.
2. Given `belongs_to: unknown` during TechPlan with an accepted resolution plan or waiver, when doctor runs, then the finding is a warning with recommendation to resolve or verify before Dev completion.
3. Given `belongs_to: unknown` during FinalizePlan without a complete waiver, when doctor runs, then the finding is a blocker.
4. Given Dev or Complete phase with unresolved `belongs_to: unknown`, when no verified pilot ledger relationship or reviewed exception exists, then doctor reports a blocker.
5. Given duplicate stable IDs, invalid stable ID prefixes, broken resolved parents, parent type mismatches, moved feature paths, projection drift, or material unchecked Salmon, when doctor runs, then finding codes and severities match the architecture severity policy.
6. Given doctor runs in any mode, then it does not write feature archives, ledgers, projection caches, governance metadata, or config files.

## Implementation Notes

- Doctor input must be the shared parser/resolver output from LSI-002 and LSI-003.
- Use architecture finding codes where applicable: `duplicate_stable_id`, `missing_required_field`, `invalid_stable_id_prefix`, `broken_belongs_to`, `parent_type_mismatch`, `parent_cycle`, `docs_path_mismatch`, `projection_drift`, `unchecked_salmon`, `invalid_waiver`, and `unpromoted_completed_feature`.
- Status output should distinguish pass, pass with advisories, and blocked.
- JSON output should include code, severity, phase relevance, stable ID, source path, message, recommendation, and evidence.
- Read-only behavior is an acceptance requirement; tests should assert no source or generated files are modified.

## Validation

- Phase fixture tests cover BusinessPlan, TechPlan, FinalizePlan, Dev, Complete, and published projection states.
- CLI contract tests cover Markdown and JSON doctor output.
- Tests prove complete waiver, incomplete waiver, verified pilot ledger relationship, reviewed exception, and unresolved unknown parent states.
- Mutation guard tests assert doctor does not write archives, ledgers, projection caches, governance metadata, or config.

## Dependencies

- Depends on LSI-003.
- Blocks LSI-005, LSI-006, LSI-009, and LSI-010.

## Dev Agent Record

- Added read-only `lens-doctor` CLI wrapper and shared doctor checks for severity, waivers, lifecycle, Lens context, links, and Salmon diagnostics.
- Preserved existing doctor test compatibility while adding two-tree fixture coverage.
- Validation: `uv run python -m pytest -q` passed in `TargetProjects/lens-dev/new-codebase/lens.core.src`.
