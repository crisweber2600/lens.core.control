---
feature_id: lens-seed-improvements
doc_type: adversarial-review
phase: techplan
source: phase-complete
verdict: pass-with-warnings
reviewed_artifacts:
  - architecture.md
predecessor_artifacts:
  - prd.md
  - ux-design.md
  - businessplan-adversarial-review.md
constitution_levels:
  - org
status: complete
created: "2026-05-23"
updated: "2026-05-23"
---

# TechPlan Adversarial Review

## Verdict

**pass-with-warnings**

The TechPlan package is sufficient to advance to FinalizePlan. `architecture.md` converts the BusinessPlan warnings into executable architecture decisions for stable IDs, `belongs_to` handling, projection rebuild ownership, topology doctor severity, Salmon signal materiality, ledger promotion boundaries, and BMAD Module Builder-driven module validation for `lens.core.src`.

No critical finding blocks phase completion. The warnings below must be carried into FinalizePlan and Dev story generation so the architecture becomes implementable work rather than a large undifferentiated module refactor.

## Review Packet

- `architecture.md`: defines the two-tree authority model, metadata contracts, projection rebuild, topology doctor, Salmon schema and transitions, ledger promotion rules, BMAD Module Builder constraints, implementation surfaces, and validation strategy.
- `prd.md`: supplies stable ID, projection, doctor, Salmon, and compatibility requirements.
- `ux-design.md`: supplies the Salmon rollup workflow, status vocabulary, review panels, and acceptance criteria.
- `businessplan-adversarial-review.md`: records the BusinessPlan warnings and the operator response to use the BMAD Module Builder reference to enhance `lens.core.src`.
- Constitution: org-level only; full track permitted; gate mode informational; review and story enforcement enabled.

## Findings

### High Risk

1. **Implementation slice still needs story-level containment.**
   The architecture correctly rejects a full UI and chooses a command/report/metadata-first slice, but it still spans schema, parser, resolver, doctor, projection, Salmon, module registration, and tests. FinalizePlan must split this into small stories with clear acceptance tests, or Dev may turn into a broad platform rewrite.

2. **`belongs_to: unknown` remains unresolved for the feature itself.**
   The architecture records an accepted TechPlan resolution plan and requires resolution or waiver before FinalizePlan. This is adequate for TechPlan, but FinalizePlan must enforce the waiver or pilot-ledger attachment before downstream implementation readiness is declared.

### Medium Risk

3. **Projection output location is architecturally bounded but not fully environment-final.**
   The architecture defines generated outputs under the configured reporting/projection path and prohibits hand-authored cache edits. Dev stories should pin the exact default output paths and how governance publication, local reports, and release packaging consume them.

4. **Module Builder validation is required but not yet wired to a local command.**
   The architecture imports the right BMAD Module Builder constraints: `module.yaml`, `module-help.csv`, setup skill registration, anti-zombie merge behavior, progressive disclosure, and Validate Module checks. FinalizePlan should create explicit validation tasks rather than assuming a human will remember to run VM-style checks.

5. **Salmon clustering can drift into heuristic behavior if tests are weak.**
   Strong, medium, and weak clustering labels are defined. Dev must test that only strong and medium rule matches influence candidate reports, while weak similarity remains advisory and cannot auto-promote.

### Low Risk

6. **Target source structure may need cleanup as part of implementation.**
   The architecture references `skills/lens-projection-rebuild/scripts/lens_projection.py` as the shared engine surface. If the current target repo has partial or missing scripts, Dev should create the engine explicitly and avoid scattering parsing logic across skills.

7. **UX accessibility requirements are deferred to later UI work.**
   This is acceptable for the command/report MVP. FinalizePlan should preserve UX acceptance criteria as later-slice coverage rather than dropping them.

## Coverage Check

- Logic flaws: no contradiction found between PRD, UX, and architecture. The architecture preserves feature archive permanence, authored ledgers, and derived projection cache boundaries.
- Coverage gaps: exact default projection output path, FinalizePlan waiver enforcement, and local Module Builder validation command remain to be made story-level concrete.
- Complexity and risk: architecture is correctly sliced away from dashboard/UI build, but Dev story boundaries need discipline.
- Cross-feature dependencies: `nextlens-src-topdownlens` remains context only; no direct blocker.
- Assumptions and blind spots: architecture assumes a shared stdlib projection engine can be introduced without destabilizing existing skill install surfaces.

## Party-Mode Blind-Spot Challenge

Winston (Architecture): The shared parser/resolver is the real core. If Dev starts by editing SKILL text instead of creating tested shared mechanics, every command will continue to disagree about source truth.

Quinn (Quality): The waiver and severity model must be tested across phase boundaries. One golden fixture with `belongs_to: unknown` is not enough; include BusinessPlan, TechPlan, FinalizePlan, and published projection states.

Paige (Technical Writing): The Module Builder constraints need to appear in the generated stories and validation checklist. Otherwise module registration and progressive disclosure will be treated as release polish instead of architecture requirements.

### Questions To Carry Forward

1. What exact story owns the first shared parser/resolver implementation?
2. Where will the default generated projection cache live in local development versus governance publication?
3. Will `lens-seed-improvements` resolve to `service:lens-workbench` before FinalizePlan, or will it carry an explicit topology waiver?
4. Which command proves Module Builder validation locally for `lens.core.src` during Dev?
5. Which Salmon cluster fixture demonstrates Feature A/B/C rolling up into one candidate larger item?

## Phase Gate Decision

TechPlan may advance to FinalizePlan with warnings. FinalizePlan should turn `architecture.md` and this review into bounded epics and stories, enforce the `belongs_to: unknown` resolution or waiver requirement, and make module validation a first-class implementation readiness gate.