---
feature: lens-seed-improvements
story_id: LSI-006
doc_type: story
status: ready-for-dev
title: "Salmon Signal Schema, State Transitions, And Deterministic Cluster Report"
depends_on:
  - LSI-001
  - LSI-002
  - LSI-003
  - LSI-004
updated_at: "2026-05-23T00:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Salmon Impact And Ledger Promotion"
priority: P1
acceptance_gate: "Salmon materiality, legal transitions, deterministic clustering, advisory weak similarity, and JSON output tests pass."
---

# LSI-006: Salmon Signal Schema, State Transitions, And Deterministic Cluster Report

## Summary

Implement Salmon signal parsing, validation, materiality checks, legal state transitions, and deterministic candidate report generation using the Feature A/B/C fixtures.

## Context

Salmon starts as source metadata and command/report data. The first implementation slice must not build a full Salmon Inbox UI. It must prove advisory and material signal handling with deterministic clustering that can later feed workbench views.

Feature A/B/C fixtures from LSI-001 provide the deterministic cluster case: at least two signals share the same target stable ID and category, so clustering can be tested without relying on text similarity.

## Scope

- Parse `salmon_upstream` entries from source metadata.
- Validate required signal fields, materiality, owner, rationale, affected IDs, review point, timestamps, and target stable IDs.
- Enforce legal state transitions: advisory to material, material to blocked, material to resolved, blocked to waived, blocked to resolved, and waived to resolved.
- Generate deterministic cluster and candidate larger item report data.
- Emit JSON report records for candidate larger items.

## Out Of Scope

- Building Salmon Inbox, Impact Cluster Board, Candidate Detail, Ledger Preview, or other workbench UI panels.
- Auto-promoting candidates into ledgers.
- Treating weak text similarity as sufficient for promotion readiness.
- Creating an always-blocking Salmon policy before advisory and material thresholds are proven.

## Acceptance Criteria

1. Given advisory Salmon signals, when parsed, then they do not block early planning by default.
2. Given a signal meets materiality criteria by contradicting published ledger truth, breaking identity or parent resolution, invalidating lifecycle assumptions, or exposing implementation-critical dependency, security, or API inconsistency, when classified, then it becomes material or blocked according to configured gate impact.
3. Given an illegal state transition, when validation runs, then the transition is rejected with a diagnostic that includes source path, signal ID, current status, requested status, and recommendation.
4. Given Feature A, Feature B, and Feature C fixtures with the same target stable ID and category across at least two features, when clustering runs, then one strong candidate report is produced deterministically.
5. Given signals share only weak text similarity or adjacency, when clustering runs, then the report may include an advisory suggestion but must not auto-promote or mark the candidate ready.
6. Given Salmon output is requested as JSON, when the report runs, then candidate records include candidate ID, title, source signals, source features, target ledger stable ID, cluster confidence, highest status, and promotion readiness.

## Implementation Notes

- Consume source entities and relationship edges from the shared parser/resolver instead of scanning independently.
- Materiality is evidence-backed only. It is material when it contradicts published ledger truth, breaks stable identity or parent resolution, invalidates lifecycle-critical assumptions, exposes implementation-critical dependency/security/API inconsistency, or blocks approved promotion.
- Strong clustering is same target stable ID plus same category across two or more features. Medium and weak cases may be reported, but weak text similarity must stay advisory.
- Candidate records are report outputs until explicitly promoted by LSI-007 behavior. They are not ledger truth.
- Diagnostics should preserve source path, signal ID, status, requested transition, and recommendation.

## Validation

- Unit tests cover signal schema validation, required fields, materiality classification, legal transitions, and illegal transition diagnostics.
- Report tests cover Feature A/B/C deterministic clustering, weak similarity advisory behavior, and JSON candidate output.
- Tests assert Salmon report generation does not promote ledgers or write projection source truth.

## Dependencies

- Depends on LSI-001, LSI-002, LSI-003, and LSI-004.
- Blocks LSI-007, LSI-008, and LSI-010.
