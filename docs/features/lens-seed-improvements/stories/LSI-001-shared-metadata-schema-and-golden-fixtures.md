---
feature: lens-seed-improvements
story_id: LSI-001
doc_type: story
status: ready-for-dev
title: "Shared Metadata Schema And Golden Fixtures"
depends_on: []
updated_at: "2026-05-23T00:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Shared Metadata And Relationship Foundation"
priority: P0
acceptance_gate: "Schema fixtures and deterministic serialization tests pass."
---

# LSI-001: Shared Metadata Schema And Golden Fixtures

## Summary

Define the canonical two-tree metadata contract and golden fixtures used by parser, resolver, doctor, projection, Salmon, promotion, lifecycle, and release validation stories.

## Context

This is the foundation story for the Lens Seed Improvements sprint. The architecture moves Lens from path-shaped planning to identity-shaped planning with stable IDs, source feature archives, landscape ledgers, and generated projection caches. Downstream stories must consume the same schema and fixtures instead of inventing local metadata interpretations.

Implementation targets `TargetProjects/lens-dev/new-codebase/lens.core.src`. The expected schema surface is `skills/lens-setup/assets/metadata-schema.md`, with fixtures and tests placed according to existing target repo conventions.

The temporary `belongs_to: unknown` waiver for `feature:lens-seed-improvements` must be represented as source metadata so later stories can validate waiver completeness and Dev-completion closure.

## Scope

- Extend the Lens metadata schema to cover source entities, waivers, Salmon signals, projection diagnostics, and candidate larger item reports.
- Add deterministic golden fixtures for one current feature, one pilot service ledger, Feature A/B/C Salmon rollup, duplicate IDs, invalid stable ID prefixes, broken parents, cycles, missing required fields, and waiver completeness.
- Keep fixture data small, stable, and suitable for unit tests and CLI contract tests.
- Include `feature:lens-seed-improvements` with `belongs_to: unknown` and a complete accepted temporary `topology_waiver`.
- Include a pilot `service:lens-workbench` ledger fixture for later waiver-closure stories.

## Out Of Scope

- Implementing parser, resolver, doctor, projection, Salmon, promotion, lifecycle, or release validation behavior.
- Building workbench UI, Salmon Inbox UI, or broad historical migration.
- Writing directly to governance feature folders or hand-editing generated projection output as source truth.

## Acceptance Criteria

1. Given a new two-tree feature fixture, when schema validation runs, then `stable_id`, `entity_type`, `feature_id`, `title`, `belongs_to`, `docs_path`, `status`, `phase`, `track`, `publication_state`, and `updated_at` are validated as source metadata.
2. Given a ledger fixture, when schema validation runs, then `stable_id`, `entity_type`, `title`, `belongs_to`, `ledger_path` or source path, linked `features`, `publication_state`, and optional owner or steward fields are validated.
3. Given `feature:lens-seed-improvements`, when fixture metadata is loaded, then it includes `belongs_to: unknown` and a complete temporary `topology_waiver` with owner, rationale, affected stable IDs, review point, impacted gate, status, and accepted timestamp.
4. Given Feature A, Feature B, and Feature C fixtures, when Salmon fixtures are loaded, then at least two signals share the same target stable ID and category so deterministic clustering can be tested without text similarity.
5. Given invalid fixture cases, when tests run, then duplicate stable IDs, invalid prefixes, missing required fields, broken parents, cycles, and incomplete waivers are all represented by named fixtures.
6. Given the fixture set is serialized twice, when tests compare normalized output, then record order and content are stable.

## Implementation Notes

- Prefer a single shared schema and fixture vocabulary that later stories can import rather than copying schema fragments into each command.
- Stable ID prefixes must match entity type: `feature:`, `service:`, `domain:`, and `program:` for MVP records.
- Feature records require `feature_id`, `track`, `phase`, and `docs_path`; target repos are optional during planning but required before Dev handoff.
- The waiver fixture should include code, owner, rationale, affected stable IDs, review point, impacted gate, status, and accepted timestamp so waiver validation can test both complete and incomplete cases.
- The Feature A/B/C fixtures should create deterministic Salmon clustering through shared target stable ID and category, not through text similarity.
- Do not use fake legacy domain or service placeholders to satisfy parentage. The pilot service ledger fixture is the intended closure path.

## Validation

- Unit tests validate schema fields for feature, service, domain, and program records.
- Unit tests cover waiver completeness and named invalid fixture cases.
- Fixture serialization tests prove stable normalized order and content.
- Tests must be runnable inside `TargetProjects/lens-dev/new-codebase/lens.core.src` without writing governance or projection source truth.

## Dependencies

- No story dependencies.
- Blocks LSI-002, LSI-003, LSI-006, and LSI-010.
