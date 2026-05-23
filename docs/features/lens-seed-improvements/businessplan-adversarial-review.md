---
feature_id: lens-seed-improvements
doc_type: adversarial-review
phase: businessplan
source: phase-complete
verdict: pass-with-warnings
reviewed_artifacts:
  - prd.md
  - ux-design.md
predecessor_artifacts:
  - brainstorm.md
  - research.md
  - product-brief.md
  - preplan-adversarial-review.md
constitution_levels:
  - org
status: complete
created: "2026-05-23"
updated: "2026-05-23"
---

# BusinessPlan Adversarial Review

## Verdict

**pass-with-warnings**

The BusinessPlan package is complete enough to advance to TechPlan. `prd.md` defines testable product requirements for the two-tree topology, projection authority, topology doctor, and Salmon foundations. `ux-design.md` turns the Salmon rollup problem into a concrete maintainer workbench flow where multiple feature signals become a candidate larger landscape item, pass doctor/projection review, and promote into a ledger without moving feature archive records.

No critical finding blocks the next phase. The warnings below should become architecture decisions, command contracts, or explicit lifecycle policy during TechPlan.

## Review Packet

- `prd.md`: defines product goals, non-goals, personas, functional requirements, non-functional requirements, data requirements, phase-aware doctor behavior, Salmon workflow requirements, compatibility requirements, and acceptance criteria.
- `ux-design.md`: defines the operational UX for Feature A/B/C Salmon signals flowing through the Feature Signal Drawer, Salmon Inbox, Impact Cluster Board, Candidate Larger Item Detail, Promotion Review Panel, Ledger Preview, Projection Explanation Panel, and Doctor Findings Strip.
- PrePlan context: the package preserves the core statement that features are immutable facts, the landscape is an interpretation, and the map is a cache.
- Constitution: org-level only; full track is permitted; review enforcement is enabled; structured gate mode is informational.

## Findings

### High Risk

1. **Projection write ownership is still a TechPlan decision.**
   The PRD and UX correctly state that projection maps are derived caches and that manual edits are invalid. TechPlan must now define the actual command boundary, output path, cache format, and governance write route. Without that, implementation could recreate hand-authored map drift under a new name.

2. **`belongs_to: unknown` policy needs executable lifecycle rules.**
   The PRD proposes warning during intake/PrePlan/BusinessPlan and resolution or waiver by TechPlan/FinalizePlan. TechPlan must translate this into validator behavior and waiver metadata, or phase gates will keep relying on human interpretation.

### Medium Risk

3. **The UX implies several workbench surfaces that may exceed the first implementation slice.**
   The Salmon Inbox, Impact Cluster Board, Candidate Detail, Projection Explanation, Ledger Preview, and Doctor Strip are valuable as a target UX. Architecture must decide the minimal first surface that proves the workflow without building a full dashboard.

4. **Signal clustering confidence needs deterministic rules before automation.**
   The UX describes strong/medium/weak cluster explanations. TechPlan should decide which rules are deterministic MVP checks and which remain manual maintainer judgment.

5. **Promotion review can become a hidden authoring workflow.**
   The UX says promotion creates or updates ledger truth through approved boundaries. Architecture must ensure this does not bypass the governance publication model or create a second manual ledger-writing path.

6. **Salmon materiality remains intentionally open.**
   The PRD defines categories that may block, and the UX shows statuses. The exact threshold for changing advisory signals into blockers still needs a small, testable policy in TechPlan.

### Low Risk

7. **Auspex/reporting context is contained but should stay downstream.**
   Both artifacts keep reporting as a consumer of projection data, not the authoring surface. Future work should preserve that separation.

8. **Terminology is consistent enough for TechPlan but still needs schema names.**
   Terms such as signal, cluster, candidate larger item, ledger entry, projection edge, and waiver need final field names and storage locations.

## Coverage Check

- Logic flaws: no critical contradiction found between PRD and UX. Both preserve permanent feature archives, reorganizable ledgers, and derived projection caches.
- Coverage gaps: command ownership, cache format, waiver schema, first-slice UI boundary, and materiality threshold remain open.
- Complexity and risk: UX scope is intentionally richer than MVP implementation; TechPlan should slice it.
- Cross-feature dependencies: `nextlens-src-topdownlens` remains contextual only; no direct blocker.
- Assumptions and blind spots: both artifacts assume a future projection/doctor substrate can explain relationships locally before promotion.

## Party-Mode Blind-Spot Challenge

Mary (Product): The PRD has clear requirements, but the first release must still feel like less work for maintainers. What is the smallest end-to-end scenario that proves value without a full workbench?

Winston (Architecture): Projection authority is the hinge. If the derived cache has no declared storage format, rebuild command, and write boundary, every downstream surface will guess differently.

Quinn (Quality): The UX uses advisory, material, blocked, waived, and resolved states. Which state transitions are legal, and which require an audit record? This needs tests, not prose.

### Questions To Carry Forward

1. What exact projection cache file or data shape will TechPlan define?
2. Which single UI/workflow slice demonstrates Feature A/B/C Salmon rollup in MVP?
3. What metadata makes a `belongs_to: unknown` waiver valid and machine-readable?
4. Which clustering rules are deterministic, and which are maintainer-curated?
5. Which Salmon state transitions require reviewer, timestamp, rationale, or affected stable IDs?

## Phase Gate Decision

BusinessPlan may advance to TechPlan with warnings. TechPlan should convert the PRD and UX into architecture decisions for projection rebuild, topology doctor, waiver handling, Salmon state transitions, and the smallest implementable Salmon rollup workflow.