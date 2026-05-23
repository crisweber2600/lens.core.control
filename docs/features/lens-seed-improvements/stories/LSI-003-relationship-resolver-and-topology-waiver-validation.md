---
feature: lens-seed-improvements
story_id: LSI-003
doc_type: story
status: done
title: "Relationship Resolver And Topology Waiver Validation"
depends_on:
  - LSI-001
  - LSI-002
updated_at: "2026-05-23T15:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Shared Metadata And Relationship Foundation"
priority: P0
acceptance_gate: "Resolver tests cover parent resolution, unknown parents, waiver completeness, cycles, and relationship edges."
---

# LSI-003: Relationship Resolver And Topology Waiver Validation

## Summary

Resolve two-tree relationships and validate `belongs_to: unknown` waiver metadata through a shared resolver used by doctor, projection, Salmon, promotion, and lifecycle checks.

## Context

LSI-001 defines source metadata and fixtures. LSI-002 emits normalized source entities and diagnostics. This story turns those source records into parent, child, membership, related, dependency, and Salmon reference edges with consistent waiver validation.

The architecture treats `belongs_to: unknown` as a planned topology debt state, not a parser failure. It must remain visible so later phase gates can classify the state correctly.

## Scope

- Resolve `belongs_to` parent edges, child edges, feature-to-ledger membership, `related_to`, `depends_on`, and Salmon reference edges.
- Validate allowed parent types for feature, service, domain, and program entities.
- Detect broken parents, wrong parent types, parent cycles, unresolved references, and waiver completeness.
- Preserve `belongs_to: unknown` and its waiver metadata for downstream phase classification.
- Emit relationship and diagnostic payloads traceable to source paths and stable IDs.

## Out Of Scope

- Deciding phase-aware severity for doctor or lifecycle gates.
- Writing projection maps or ledger records.
- Promoting feature facts into landscape ledgers.
- Replacing legacy topology compatibility reads with legacy write requirements.

## Acceptance Criteria

1. Given a feature whose `belongs_to` references `service:lens-workbench`, when resolver runs, then parent, child, and membership edges are emitted with source evidence.
2. Given `belongs_to: unknown`, when resolver runs, then it emits an unknown-parent finding and preserves waiver metadata for phase classification by doctor and lifecycle checks.
3. Given a complete `topology_waiver`, when waiver validation runs, then the waiver is accepted and includes code, owner, rationale, affected stable IDs, review point, impacted gate, and status.
4. Given an incomplete waiver, when validation runs, then an `invalid_waiver` diagnostic identifies missing fields and the affected stable IDs.
5. Given a parent cycle across ledger records, when resolver runs, then a `parent_cycle` diagnostic identifies the cycle path without infinite recursion.
6. Given `related_to`, `depends_on`, or Salmon target references, when resolver runs, then relationship edges are emitted separately from parent and membership edges.

## Implementation Notes

- Resolver input is the shared parser output from LSI-002; do not re-parse source files locally.
- Parent rules follow the architecture table: features may reference service, domain, program, or unknown; services should parent to domain when known; domains should parent to program when known; programs need no parent.
- Keep `unknown` as a known debt state with diagnostics and waiver data, not an exception path.
- Relationship edge output should separate parent, child, membership, related, dependency, and Salmon edges so projection and explain modes can report them cleanly.
- Cycle detection must be deterministic and bounded.
- Diagnostics should use stable codes such as `broken_belongs_to`, `parent_type_mismatch`, `parent_cycle`, `invalid_waiver`, and `unresolved_reference` where aligned with architecture.

## Validation

- Resolver unit tests cover known parent resolution, unknown parent preservation, complete waiver acceptance, incomplete waiver diagnostics, parent cycle detection, and non-parent relationship edges.
- Tests use golden fixtures from LSI-001 and parser output from LSI-002.
- Tests verify no resolver operation writes source archives, ledgers, projection caches, governance metadata, or config.

## Dependencies

- Depends on LSI-001 and LSI-002.
- Blocks LSI-004, LSI-005, LSI-006, LSI-007, LSI-009, and LSI-010.

## Dev Agent Record

- Implemented relationship edge generation, parent validation, cycle detection, and topology waiver completeness checks.
- Pilot ledger fixture links `service:lens-workbench` to `feature:lens-seed-improvements`.
- Validation: `uv run python -m pytest -q` passed in `TargetProjects/lens-dev/new-codebase/lens.core.src`.
