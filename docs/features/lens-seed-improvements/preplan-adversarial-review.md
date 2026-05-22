---
feature_id: lens-seed-improvements
doc_type: adversarial-review
phase: preplan
source: phase-complete
verdict: pass-with-warnings
reviewed_artifacts:
  - brainstorm.md
  - research.md
  - product-brief.md
  - product-brief-distillate.md
related_context:
  - nextlens-src-topdownlens
constitution_levels:
  - org
status: complete
created: "2026-05-22"
updated: "2026-05-22"
---

# PrePlan Adversarial Review

## Verdict

**pass-with-warnings**

The PrePlan package is complete enough to advance to BusinessPlan. The core problem, direction, MVP scope, and handoff risks are documented across the brainstorming, technical research, and product brief artifacts. No critical finding blocks the next phase.

The package still carries meaningful warnings: external web research was unavailable, lineage and constitution behavior for `belongs_to: unknown` remains unresolved, and several compatibility decisions are intentionally deferred to later planning phases.

## Review Packet

- `brainstorm.md`: establishes the Two-Tree Model with Derived Map and rejects the old fixed `domain > service > feature` construct as the durable knowledge owner.
- `research.md`: identifies technical implementation surfaces, projection rebuild behavior, topology doctor checks, Salmon workflow semantics, and compatibility risks.
- `product-brief.md`: frames the internal product value, MVP scope, success criteria, and adoption guardrails.
- `product-brief-distillate.md`: preserves overflow context for downstream PRD creation.
- Related context: `nextlens-src-topdownlens` was available as an archived related summary, but did not add material predecessor requirements beyond confirming prior Lens lifecycle work exists.
- Constitution: org-level only; full track is permitted; review enforcement is enabled; planning artifact requirements are informational in the resolved constitution.

## Findings

### High Risk

1. **Lineage policy remains unresolved for `belongs_to: unknown`.**
   The artifacts correctly reject fake domain/service placeholders, but BusinessPlan must define how conductors, constitution resolution, and validators behave when a feature has no service or domain parent. Without that policy, the redesign can still be blocked by the exact legacy gate it is meant to remove.

2. **Projection authority needs sharper boundary language.**
   The package consistently says the governance map is a derived cache, but BusinessPlan must convert that principle into user-visible rules: who can rebuild it, where it lives, what happens on drift, and which operations may write it.

### Medium Risk

3. **Research is locally grounded, not externally verified.**
   The technical research explicitly records that web search was unavailable. That is acceptable for PrePlan, but later phases should avoid treating the report as industry validation or proof that comparable systems work the same way.

4. **MVP scope may still be too broad if treated as one delivery slice.**
   Stable IDs, projection rebuild, doctor checks, draft/published metadata, ledgers, and Salmon are a coherent direction, but not all should be first implementation work. BusinessPlan should distinguish enabling MVP from follow-on capabilities.

5. **Ledger promotion discipline is underspecified.**
   The artifacts identify dual truth drift but do not yet define when feature learnings must promote into a service/domain/program ledger, who owns promotion, or how failure is surfaced.

6. **Legacy compatibility is named but not bounded.**
   Additive reads plus stricter new writes is a good posture, but BusinessPlan must identify which legacy commands, validators, and feature-index assumptions are in or out for the first release.

### Low Risk

7. **Auspex reporting context could distract downstream planning.**
   The artifacts correctly frame Auspex MVP1 as an adjacent consumer, but later authors should keep this feature focused on topology mechanics and knowledge flow rather than building reporting UI.

8. **Terminology needs canonicalization.**
   Terms such as landscape, ledger, projection, governance map, stable ID, and entity type are clear enough for PrePlan but need final definitions before PRD and architecture generation.

## Coverage Check

- Logic flaws: no critical contradiction found; the feature/archive/landscape/projection separation is internally consistent.
- Coverage gaps: lineage policy, projection write boundary, and ledger promotion remain open.
- Complexity and risk: MVP must be sliced carefully to avoid coupling all topology ideas into the first implementation.
- Cross-feature dependencies: related `nextlens-src-topdownlens` context is acknowledged; no direct dependency blocks PrePlan.
- Assumptions and blind spots: current assumptions are visible in the artifacts, especially around local-only research, no historical migration upfront, and advisory-first Salmon.

## Party-Mode Blind-Spot Challenge

Mary (Business Analyst): The story is persuasive, but the buyer of this internal change is the Lens maintainer who has to trust it under pressure. What proof will convince them that the new model reduces operational load instead of adding another metadata ritual?

Winston (Architect): The cache/source boundary is the hinge. If projection writes touch governance, the approved boundary must be unambiguous before architecture starts. Otherwise the first implementation will recreate manual map drift with nicer names.

Quinn (QA): Doctor checks are doing a lot of work in this plan. Which findings are warnings, which are blockers, and at which phase? Without severity rules, every audit result becomes a negotiation.

### Questions To Carry Forward

1. When does `belongs_to: unknown` become unacceptable: before BusinessPlan, before TechPlan, before FinalizePlan, or only before Dev?
2. Where will the derived projection cache live, and which Lens command is the only allowed writer?
3. What is the minimum pilot ledger that proves the model without forcing a full service/domain/program hierarchy?
4. Which doctor findings block phase advancement in the MVP, and which only warn?
5. What materiality threshold turns a Salmon signal from advisory into blocking?

## Phase Gate Decision

PrePlan may advance to BusinessPlan with the warnings above preserved. The next phase should convert the product direction into explicit requirements, especially lineage policy, projection ownership, MVP boundaries, and audit severity rules.