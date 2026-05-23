---
feature_id: lens-seed-improvements
doc_type: review-report
phase: finalizeplan
source: phase-complete
verdict: pass-with-warnings
reviewed_artifacts:
  - product-brief.md
  - research.md
  - brainstorm.md
  - prd.md
  - ux-design.md
  - architecture.md
predecessor_reviews:
  - preplan-adversarial-review.md
  - businessplan-adversarial-review.md
  - techplan-adversarial-review.md
constitution_levels:
  - org
status: complete
created: "2026-05-23"
updated: "2026-05-23"
---

# FinalizePlan Review

## Verdict

**pass-with-warnings**

The planning set is ready to generate the downstream execution bundle. PrePlan, BusinessPlan, and TechPlan artifacts are present and coherent. The package defines the two-tree authority model, stable IDs, parent references, derived projection cache, topology doctor, advisory Salmon flow, ledger promotion boundaries, and BMAD Module Builder validation requirements for `lens.core.src`.

No fail-level issue remains after the pre-review metadata fixes below. The remaining warnings must be preserved as epics, stories, acceptance criteria, or explicit deferrals in the downstream bundle.

## Pre-Review Fixes Applied

1. **Registered the implementation target repo.**
   `feature.yaml` now includes `TargetProjects/lens-dev/new-codebase/lens.core.src` as the implementation target for Dev handoff.

2. **Recorded the `belongs_to: unknown` topology waiver.**
   `feature.yaml` now records an accepted `topology_waiver` for `feature:lens-seed-improvements`. The waiver preserves the architecture decision that this feature must not invent a legacy domain/service placeholder before the pilot ledger exists. Dev stories must create or verify the pilot service ledger path before Dev completion.

3. **Kept feature archive permanence intact.**
   The feature remains under `docs/features/lens-seed-improvements`; no topology movement or governance hand-copy was performed.

## Review Packet

- PrePlan: `brainstorm.md`, `research.md`, `product-brief.md`, and `preplan-adversarial-review.md` establish the two-tree model and local technical research limits.
- BusinessPlan: `prd.md`, `ux-design.md`, and `businessplan-adversarial-review.md` define product, UX, and Salmon rollup requirements.
- TechPlan: `architecture.md` and `techplan-adversarial-review.md` convert the requirements into target implementation surfaces and story-generation constraints.
- Feature metadata: `feature.yaml` now has target repo metadata and an accepted topology waiver for the remaining unknown parent state.
- Constitution: org-level only; `full` track permitted; review and dev stories enforced; gate mode informational.

## Findings To Carry Into The Bundle

### High Priority

1. **Shared parser/resolver must be the first implementation foundation.**
   The execution bundle must not start by editing SKILL prose. The first epic should establish tested source inventory, stable ID validation, parent resolution, waiver validation, and shared diagnostic structures used by doctor, projection, map audit, Salmon, and promotion workflows.

2. **The topology waiver is temporary and must be story-visible.**
   The waiver allows FinalizePlan to proceed, but stories must require creation or verification of the pilot `service:lens-workbench` ledger before Dev completion or closeout.

3. **Module Builder validation is a release gate.**
   The bundle must include work for `module.yaml`, `module-help.csv`, prompt/skill registration, anti-zombie setup behavior, progressive disclosure checks, and a VM-style validation step for `lens.core.src`.

### Medium Priority

4. **Projection output paths must be pinned in stories.**
   The architecture defines generated map outputs and governance boundaries, but stories must state the exact local default paths and how `--check`, `--write`, `--explain`, `--json`, and `--include-drafts` behave.

5. **Salmon clustering must remain deterministic.**
   The Feature A/B/C rollup should be represented as test fixtures and report output. Weak text similarity may be reported as advisory only and must not drive auto-promotion.

6. **UX workbench panels are later-slice consumers.**
   The first bundle should produce command/report data that could support the Salmon Inbox, Impact Cluster Board, Candidate Detail, Ledger Preview, and Projection Explanation Panel later.

### Low Priority

7. **Historical migration remains later scope.**
   New two-tree writes and compatibility reads are in scope. Broad migration of historical features is not.

8. **Reporting remains downstream.**
   Reporting snapshots may consume projection outputs but must not author topology truth.

## Party-Mode Blind-Spot Challenge

Winston (Architecture): If the bundle does not isolate the shared engine first, each Lens skill will keep implementing its own idea of metadata truth. Make the parser/resolver story small, tested, and reused.

Quinn (Quality): The hardest bug class will be phase severity drift. Require fixtures for BusinessPlan, TechPlan, FinalizePlan, Dev, and published projection states so `belongs_to: unknown`, waivers, and material Salmon cannot change meaning silently.

Paige (Technical Writing): Module Builder rules are part of the architecture, not documentation garnish. The story bundle needs registration and validation acceptance criteria wherever command surfaces change.

## Questions For Dev Handoff

1. Which story creates the shared inventory/resolver module and golden fixtures?
2. Which story creates or verifies the `service:lens-workbench` pilot ledger and clears the temporary topology waiver?
3. Which story owns the projection output path contract and check/write/explain modes?
4. Which story proves the Feature A/B/C Salmon rollup fixture?
5. Which story runs VM-style module validation for `lens.core.src` before release?

## Intentional Deferrals

- Full workbench UI for Salmon Inbox, Impact Cluster Board, Candidate Detail, and Ledger Preview is deferred until command/report data exists.
- Broad historical feature migration is deferred.
- Always-blocking Salmon policy is deferred until advisory and material thresholds are proven with fixtures.
- Mature program/domain/service hierarchy is deferred beyond the pilot service ledger.

## Phase Gate Decision

FinalizePlan may continue to downstream bundle generation with warnings. The bundle must preserve the applied metadata fixes and convert all high-priority findings into implementation-ready epics, stories, readiness checks, sprint status, and story files.