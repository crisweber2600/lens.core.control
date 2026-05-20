---
feature: nextlens-src-bottomup
doc_type: adversarial-review
status: responses-recorded
phase: techplan
source: phase-complete
verdict: pass-with-warnings
critical_count: 0
high_count: 0
medium_count: 3
low_count: 2
carry_forward_blockers: []
reviewed_artifacts:
  - docs/nextlens/src/nextlens-src-bottomup/architecture.md
depends_on:
  - docs/nextlens/src/nextlens-src-bottomup/businessplan-adversarial-review.md
  - docs/nextlens/src/nextlens-src-bottomup/prd.md
  - docs/nextlens/src/nextlens-src-bottomup/ux-design.md
updated_at: 2026-05-20T00:00:00Z
---

# Adversarial Review: nextlens-src-bottomup / techplan

**Reviewed:** 2026-05-20T00:00:00Z  
**Source:** phase-complete  
**Overall Rating:** pass-with-warnings

## Summary

The TechPlan is fit to proceed. The architecture correctly pivots the solution away from current Lens governance/control/release constructs and toward a brand-new standalone BMad module, using the BMad Builder guide as the module packaging authority. The plan preserves the product’s core restraint promise: one bottom-up packet, explicit safety gates, separate packet validity and BMAD readiness, and machine-verifiable non-effects. Remaining risks are implementation-level: exact schema dependency choice, shared script packaging shape, marketplace metadata, and confirmation-token details still need to be resolved during FinalizePlan stories rather than blocking TechPlan completion.

## Findings

### Critical

| # | Dimension | Finding | Recommendation |
|---|-----------|---------|----------------|
| — | — | No critical findings. | Proceed to FinalizePlan. |

### High

| # | Dimension | Finding | Recommendation |
|---|-----------|---------|----------------|
| — | — | No high-severity findings. | Proceed; preserve the non-Lens boundary in epics and stories. |

### Medium / Low

| # | Severity | Dimension | Finding | Recommendation |
|---|----------|-----------|---------|----------------|
| M1 | Medium | Coverage Gaps | The architecture defers the choice between handwritten Python validation and `jsonschema`. This is acceptable, but implementation stories need a concrete decision before schema tests are authored. | FinalizePlan should create a story or acceptance criterion that locks the schema validation mechanism before packet validator implementation begins. |
| M2 | Medium | Complexity and Risk | Shared helper packaging is unresolved: duplication per skill is preferred for MVP, but a `bul-common` internal helper could emerge. Incorrect packaging could break BMad module validation or installation. | FinalizePlan should sequence setup/structure validation before shared helper refactoring and keep helpers local until module validation proves an internal shared shape. |
| M3 | Medium | Assumptions and Blind Spots | The architecture says the module must not use Lens constructs, but downstream story authors may still be tempted to reuse current NextLens top-down runtime modules because they already exist. | Stories must include explicit negative acceptance criteria: no dependency on `feature.yaml`, Lens governance repos, Lens branch topology, Lens constitution, or current NextLens top-down runtime modules. |
| L1 | Low | Logic Flaws | The module code `bul` is practical but may be less obvious than the human phrase “Bottom-Up LENS.” | Preserve `Bottom-Up LENS` as the user-facing name and `bul` only as the technical module code. |
| L2 | Low | Coverage Gaps | Marketplace metadata and license are acknowledged but not chosen. | FinalizePlan should include a setup/scaffold story task to fill owner, repository, homepage, license, and keywords. |

## Accepted Risks

- The exact schema validation implementation can be decided during implementation planning as long as the first validator story locks it before coding dependent scripts.
- The MVP intentionally ships without BMAD execution, read-only reporting UI, adjacency detection, repeated pressure detection, Salmon routing, promotion, Landscape updates, or Graph projection.
- The MVP intentionally uses command/prompt-native UX rather than a browser UI.
- The architecture accepts BMad module packaging as the install/distribution mechanism and does not attempt to make the module a standalone CLI package.

## Party-Mode Challenge

Winston (Architect): The pivot is correct, but implementation will fail if the first scaffold story leaves room for Lens imports. Put the non-Lens boundary directly into every early story’s acceptance criteria, not only in the architecture prose.

Paige (Technical Writer): The plan needs one crisp README narrative: “This is not Lens governance; it is a BMad module that creates a safe packet.” Without that, users will map old Lens terms onto the new module and expect governance side effects.

Quinn (QA Engineer): Non-effects are only trustworthy if the tests try to catch lies. Include a false-receipt fixture and a forbidden changed-file fixture early. Do not wait until the verifier is “mostly done.”

## Gaps You May Not Have Considered

1. How will evals assert that Lens skills did not trigger or get used during standalone module flows?
2. Should the first scaffold story include a Module Builder validation run as an explicit acceptance criterion?
3. How will the module behave if `_bmad/config.yaml` is absent and the user invokes `bul-validate-packet` directly on a file path?
4. What exact output contract will future BMAD handoff consume if `bmadReady.status=fail` but `packetValid.status=pass`?
5. Should “Bottom-Up LENS” be documented as a product name while avoiding any implication that Lens governance is installed?

## Open Questions Surfaced

- Should MVP schema validation be handwritten Python or JSON Schema-backed?
- Should the final confirmation token be exactly `CREATE PACKET`, or should the module use a generated phrase containing the selected feature ID?
- Should helper code remain duplicated per skill until after the first module validation pass?
- What license and repository metadata should the distribution manifest use?
- Which eval should explicitly prove that unrelated Lens lifecycle prompts do not trigger this module?
