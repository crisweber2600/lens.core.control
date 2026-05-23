---
feature: lens-seed-improvements
story_id: LSI-002
doc_type: story
status: done
title: "Shared Inventory And Frontmatter Parser"
depends_on:
  - LSI-001
updated_at: "2026-05-23T15:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Shared Metadata And Relationship Foundation"
priority: P0
acceptance_gate: "Parser unit tests cover Markdown frontmatter, standalone YAML, ledgers, duplicates, missing fields, and compatibility reads."
---

# LSI-002: Shared Inventory And Frontmatter Parser

## Summary

Implement one reusable inventory and frontmatter parser for feature archives, standalone `feature.yaml`, and landscape ledger records.

## Context

The FinalizePlan review identified parser/resolver drift as a high implementation risk. Doctor, projection, Salmon, promotion, lifecycle, and release validation must not each parse Lens metadata differently. This story creates the shared parser foundation using the schema and fixtures from LSI-001.

The parser must support new two-tree metadata while keeping legacy topology files readable for compatibility context. Compatibility reads must not become the write pattern for new records.

## Scope

- Create or extend a shared parser module under the Lens command script area in `lens.core.src`.
- Scan configured feature archive paths and landscape ledger roots.
- Parse Markdown YAML frontmatter and standalone YAML metadata files.
- Normalize source paths relative to the project root.
- Emit source entities, duplicate stable ID diagnostics, missing field diagnostics, and a source path index.
- Preserve compatibility read mode for legacy topology inputs without treating them as authoritative new write targets.

## Out Of Scope

- Resolving parent, membership, related, dependency, or Salmon edges.
- Classifying phase-aware severity or projection readiness.
- Updating SKILL prose or module registration assets before the shared parser exists.
- Writing governance maps, ledgers, feature archives, or generated projection output.

## Acceptance Criteria

1. Given a Markdown feature record with YAML frontmatter, when inventory scanning runs, then the parser emits one normalized entity keyed by `stable_id` with a project-relative source path.
2. Given a standalone `feature.yaml`, when inventory scanning runs, then the parser emits the same normalized entity shape as Markdown frontmatter.
3. Given a service ledger fixture, when the landscape root is scanned, then the parser emits a ledger entity without requiring the ledger to live under `docs/features/`.
4. Given two source records with the same `stable_id`, when parsing completes, then the inventory emits a duplicate stable ID diagnostic that includes both source paths.
5. Given a source record with missing required fields, when parsing completes, then diagnostics include finding code, severity candidate, source path, stable ID if available, and recommendation.
6. Given legacy topology files are present, when compatibility read mode is enabled, then they may be indexed for context without becoming the write pattern for new two-tree records.

## Implementation Notes

- Build on LSI-001 fixtures and schema definitions; do not introduce a separate metadata contract.
- Parser output should be deterministic and easy for resolver, doctor, projection, Salmon, promotion, lifecycle, and release validation to consume.
- Diagnostics should include code, severity candidate, source path, stable ID when available, message, recommendation, and evidence where useful.
- Normalize paths to project-relative strings so CLI output and tests remain stable across local machines.
- The parser should treat `belongs_to: unknown` as valid source input. Waiver validity belongs to LSI-003.
- Prefer standard library YAML/frontmatter handling already used in the target repo where available; avoid adding dependencies unless the repo pattern requires it.

## Validation

- Parser unit tests use LSI-001 golden fixtures.
- Tests cover Markdown frontmatter, standalone `feature.yaml`, ledger records outside `docs/features/`, duplicate stable IDs, missing fields, path normalization, and compatibility read mode.
- Tests assert no parser operation mutates source archives, ledgers, projection caches, governance metadata, or config.

## Dependencies

- Depends on LSI-001.
- Blocks LSI-003, LSI-006, and LSI-010.

## Dev Agent Record

- Implemented shared nested YAML/frontmatter inventory parsing in `skills/lens-setup/scripts/lens_seed_core.py`.
- Parser normalizes source paths, duplicate diagnostics, missing fields, ledgers, and compatibility source kind.
- Validation: `uv run python -m pytest -q` passed in `TargetProjects/lens-dev/new-codebase/lens.core.src`.
