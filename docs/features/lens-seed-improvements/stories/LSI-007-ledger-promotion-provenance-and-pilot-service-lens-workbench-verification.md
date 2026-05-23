---
feature: lens-seed-improvements
story_id: LSI-007
doc_type: story
status: ready-for-dev
title: "Ledger Promotion Provenance And Pilot service:lens-workbench Verification"
depends_on:
  - LSI-003
  - LSI-005
  - LSI-006
updated_at: "2026-05-23T00:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Salmon Impact And Ledger Promotion"
priority: P0
acceptance_gate: "Promotion provenance, material Salmon blocking, pilot ledger verification, and Dev-completion waiver closure tests pass."
---

# LSI-007: Ledger Promotion Provenance And Pilot service:lens-workbench Verification

## Summary

Require explicit provenance for ledger promotion and verify or create the pilot `service:lens-workbench` ledger relationship needed to close this feature's temporary topology waiver before Dev completion.

## Context

The temporary `belongs_to: unknown` waiver for `feature:lens-seed-improvements` is accepted only so this feature can prove the pilot ledger model. Dev completion must not treat the waiver as permanent. The implementation must verify that the pilot `service:lens-workbench` ledger exists and links to the feature, or preserve a complete reviewed exception that lifecycle validation can see.

Ledger promotion is explicit and audited. It must not happen as a side effect of projection rebuild or Salmon report generation.

## Scope

- Update ledger promotion behavior to require source feature stable IDs, source paths, signal IDs, target ledger stable ID and path, material Salmon review status, doctor status, projection preview or explain output, owner or steward acceptance, and promotion rationale.
- Add a pilot verification path for `service:lens-workbench` as the service-level ledger for this feature.
- Ensure promotion remains explicit, reviewed, and audited.
- Add Dev-completion validation coverage for closing or preserving the temporary topology waiver.

## Out Of Scope

- Automatic promotion from projection rebuild, Salmon clustering, or doctor output.
- Moving feature archive paths into a service/domain folder.
- Inventing fake legacy domain or service placeholders to satisfy old topology gates.
- Building ledger preview or promotion UI.

## Acceptance Criteria

1. Given a candidate promotion, when required provenance is missing, then promotion is blocked with a finding that names the missing source feature, signal, ledger, doctor, projection, owner, or rationale evidence.
2. Given material or blocked Salmon signals exist for a candidate, when promotion is reviewed, then promotion is disabled until each material issue is resolved or has a complete waiver.
3. Given promotion is accepted, when the ledger entry is created or updated through the approved boundary, then provenance records include `source_feature`, `source_signals`, `promotion_rationale`, reviewer or steward acceptance, and source paths.
4. Given Feature A/B/C fixtures, when promotion preview runs, then it shows the target ledger stable ID, linked features, signal IDs, and projection explain evidence without moving feature archive paths.
5. Given `feature:lens-seed-improvements` still has `belongs_to: unknown`, when Dev completion validation runs, then it fails unless the pilot `service:lens-workbench` ledger exists and is linked to the feature or a complete reviewed exception remains visible.
6. Given the pilot ledger exists, when resolver and doctor run, then `feature:lens-seed-improvements` can be attached or verified against `service:lens-workbench` without inventing a fake legacy domain or service placeholder.

## Implementation Notes

- Preserve the temporary topology waiver closure requirement exactly: Dev completion requires verified `service:lens-workbench` relationship or a complete reviewed exception visible to lifecycle validation.
- Promotion must consume resolver, projection explain, doctor, and Salmon review evidence from prior stories.
- Ledger edits, if supported, must occur only through the approved ledger promotion or topology design apply boundary in the target repo implementation. Do not hand-copy into governance.
- Provenance fields must be first-class validation requirements, not optional narrative text.
- Promotion preview should be useful without mutating feature archive paths or generated projection caches.
- This story creates a closeout dependency for LSI-009 and LSI-010: lifecycle and release validation must fail if waiver closure is not verified.

## Validation

- Promotion provenance tests cover missing source feature, signal, ledger, doctor, projection, owner, and rationale evidence.
- Material Salmon blocking tests prove unresolved material or blocked signals disable promotion unless resolved or fully waived.
- Pilot ledger fixture tests verify `service:lens-workbench` links to `feature:lens-seed-improvements`.
- Dev-completion waiver closure tests fail unresolved `belongs_to: unknown` unless the pilot service ledger relationship or complete reviewed exception is visible.

## Dependencies

- Depends on LSI-003, LSI-005, and LSI-006.
- Blocks LSI-008, LSI-009, and LSI-010.
