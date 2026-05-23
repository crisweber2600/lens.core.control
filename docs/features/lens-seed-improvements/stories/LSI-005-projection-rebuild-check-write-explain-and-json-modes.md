---
feature: lens-seed-improvements
story_id: LSI-005
doc_type: story
status: ready-for-dev
title: "Projection Rebuild Check, Write, Explain, And JSON Modes"
depends_on:
  - LSI-003
  - LSI-004
updated_at: "2026-05-23T00:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Topology Doctor And Projection Rebuild"
priority: P0
acceptance_gate: "Projection CLI contract tests pass for check, write, explain, JSON, include-drafts, drift, and explicit output paths."
---

# LSI-005: Projection Rebuild Check, Write, Explain, And JSON Modes

## Summary

Implement projection rebuild as the only owner of generated `governance-map.json` and `governance-map.md` files, with deterministic check, write, explain, draft-inclusive, and JSON behavior.

## Context

Projection files are generated caches, not source truth. Feature archives and landscape ledgers remain authored source records. This story makes projection rebuild the executable owner of generated map writes and prevents hand-authored projection output from becoming authority.

The architecture points to `skills/lens-projection-rebuild/scripts/lens_projection.py` as the expected implementation surface if absent, with user-facing workflow text kept in the Lens projection skill.

## Scope

- Implement or complete the projection engine in `lens.core.src` using shared inventory, resolver, and doctor components.
- Pin generated filenames to `governance-map.json` and `governance-map.md` under the provided `--reporting-output-path`.
- Require explicit output path arguments for local development and never write directly to governance as a fallback.
- Provide `--check`, `--write`, `--explain <stable_id>`, `--json`, and `--include-drafts` modes.
- Include derived marker, source paths, entities, edges, diagnostics, and draft-inclusive state in machine-readable output.

## Out Of Scope

- Hand-editing generated projection output.
- Writing directly to governance feature folders or governance map paths outside an approved output argument.
- Ledger promotion or topology apply operations.
- Workbench UI for projection explanation panels.

## Acceptance Criteria

1. Given stable source metadata and an explicit reporting output path, when projection rebuild runs twice, then generated JSON and Markdown content are deterministic except for an intentionally controlled generation timestamp field.
2. Given `--check`, when generated output differs from committed cache files, then the command reports projection drift and does not write files.
3. Given `--write` and no doctor blockers, when the command runs, then it writes only `governance-map.json` and `governance-map.md` under the configured reporting output path.
4. Given `--write` and doctor blockers, when no explicit force option is accepted, then the command refuses to write and reports blockers.
5. Given `--explain feature:lens-seed-improvements`, when the command runs, then output shows source path, source fields, parent edge or unknown parent state, child edges, ledger membership, related edges, Salmon edges, and diagnostics.
6. Given `--json`, when any projection mode runs, then machine-readable output includes module, report type, derived marker, source paths, entities, edges, diagnostics, and include-drafts state.
7. Given `--include-drafts`, when projection output is generated, then draft entities are included and the output clearly marks draft-inclusive status.

## Implementation Notes

- Projection must consume parser/resolver/doctor output from LSI-002 through LSI-004.
- Use the architecture command shape and keep output paths explicit. Do not silently fall back to governance paths.
- Projection JSON should include module, report type, derived marker, generated timestamp, source paths, entities, parent/child/membership/related/Salmon edges, diagnostics, and include-drafts state.
- Explain output must be source-traceable and useful for `feature:lens-seed-improvements` and the pilot `service:lens-workbench` fixture.
- `--write` must refuse to write when doctor blockers exist unless an explicit accepted force behavior is implemented and tested.
- Keep generated output deterministic through stable sorting and normalized paths.

## Validation

- CLI contract tests cover `--check`, `--write`, `--explain`, `--json`, `--include-drafts`, drift, blocker refusal, deterministic output, and explicit output paths.
- Tests assert write mode only writes `governance-map.json` and `governance-map.md` under the requested reporting output path.
- Tests assert check and explain modes do not mutate source files or generated caches.

## Dependencies

- Depends on LSI-003 and LSI-004.
- Blocks LSI-007, LSI-008, LSI-009, and LSI-010.
