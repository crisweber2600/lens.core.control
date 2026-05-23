---
feature: lens-dev-new-codebase-auspex
doc_type: expressplan-adversarial-review
status: responses-recorded
review_format: abc-choice-v1
phase: expressplan
source: phase-complete
verdict: pass-with-warnings
updated_at: "2026-05-23T00:00:00Z"
---

# ExpressPlan Adversarial Review

**Feature:** `lens-dev-new-codebase-auspex`  
**Phase:** expressplan  
**Source:** phase-complete  
**Verdict:** `pass-with-warnings`

## Artifacts Reviewed

- `business-plan.md` ✓
- `tech-plan.md` ✓
- `sprint-plan.md` ✓

Supplementary context:

- Feature description: Auspex preferred Lens workflow for Two-Tree topology, Salmon, derived maps, ledgers, audits, and MVP1 reporting snapshots.
- Resolved constitution for `lens-dev / new-codebase`: express permitted; gate mode informational; planning requires `business-plan` and `tech-plan`; stories and review are enforced; lens-work skill/workflow changes must consult BMad Builder docs and use BMB implementation channel.

## Summary

The ExpressPlan packet is coherent and reviewable. It keeps Auspex framed as a read-only visibility plane over source truth, narrows MVP1 to static reporting snapshots, and carries the key constitutional constraints into the technical and sprint plans. No critical blocker prevents FinalizePlan. The remaining issues are scope-shaping risks that FinalizePlan must convert into explicit story acceptance criteria: exact output location, adapter contracts for existing commands, report freshness semantics, and BMB evidence.

## Findings

### Critical

No critical findings.

### High

| # | Dimension | Finding | Recommendation |
|---|---|---|---|
| H1 | Source-of-truth boundary | The plans correctly state Auspex is read-only, but they do not yet name the final default report root. Without a concrete safe root, an implementer could choose a governance-adjacent path and weaken the boundary. | FinalizePlan should create a story acceptance criterion requiring an explicit generated-output root and tests that reject governance feature-doc paths before mutation. |
| H2 | Cross-feature dependency | Auspex depends on existing projection rebuild, map audit, ledger promotion, Salmon impact, and reporting snapshot surfaces whose data contracts may differ. The plan describes tolerant adapters but not a minimum input contract per source. | FinalizePlan should split adapter work and require fixture contracts for each source: minimum fields, missing-source behavior, and source-reference preservation. |

### Medium / Low

| # | Dimension | Finding | Recommendation |
|---|---|---|---|
| M1 | Coverage gap | Freshness is central to Auspex, but the plan does not yet define whether freshness is timestamp-, hash-, or rebuild-manifest-based. | Add a story task to inspect existing Derived Map output and choose a concrete freshness rule before implementation. |
| M2 | Complexity and risk | The workflow lists ledger promotion before reporting, but reporting must not perform promotion itself. This distinction may be lost in implementation if the command tries to be a one-shot orchestrator. | In stories, separate "consume ledger promotion context" from "invoke ledger promotion"; default report generation should read existing evidence unless an explicit workflow mode runs upstream commands. |
| M3 | Governance/process | The BMB requirement is included, but the sprint plan does not specify the evidence artifact showing BMB was used. | Require implementation notes or story checkboxes documenting BMad Builder reference consultation and BMB channel use for any skill/workflow change. |
| L1 | Rollout | Optional `dashboard.html` is mentioned. Even as optional, it may distract MVP1 into UI work. | Keep dashboard generation out of first-pass acceptance criteria unless all static JSON/Markdown contracts are complete. |

## Party-Mode Blind-Spot Challenge

Mary (Product Manager): The business value is "make Lens state visible." If MVP1 only proves that a report can be generated from fixtures, stakeholders may still not know when to run it. The workflow story needs a clear trigger: after projection rebuild, before review, after Salmon, or on demand.

Winston (Architect): The source boundary is sound, but orchestration mode is ambiguous. A pure snapshot reader and a workflow runner are different tools. If one command both invokes upstream commands and emits a report, tests must prove the upstream writes are intentional and the report writer remains read-only.

Quinn (QA): The read-only test must compare source hashes across governance, ledgers, derived inputs, and Salmon fixtures. It is not enough to assert "no feature.yaml write"; path traversal and default-output behavior need negative tests.

## Blind-Spot Challenge Questions

1. What exact generated-output root is safe for Auspex snapshots, and should it be configurable per workspace?
2. Is MVP1 a snapshot-only command, a workflow runner that invokes upstream commands, or two modes with different write expectations?
3. Which existing Derived Map artifact supplies freshness data today: timestamp, content hash, manifest, or none?
4. What is the minimum stable Salmon signal shape that Auspex can consume without coupling to future Salmon internals?
5. What evidence will a reviewer inspect to confirm BMB and BMad Builder documentation were used for lens-work edits?

## Responses / Required FinalizePlan Handling

1. **Safe output root** — Treat this as a FinalizePlan acceptance criterion. Stories should select a generated-output root outside governance feature docs and test rejection of unsafe roots.
2. **Snapshot versus workflow runner** — Start MVP1 as snapshot generation over existing evidence. If a workflow runner is included, it must be a separate mode with explicit upstream command invocation and postflight handling.
3. **Freshness source** — Carry as an implementation discovery task in the Derived Map adapter story, then encode the chosen rule in tests.
4. **Salmon shape** — Use a tolerant minimum contract: status counts, affected IDs, source path, and raw reference preservation. Do not require richer Salmon internals for MVP1.
5. **BMB evidence** — Require story-level implementation notes or checklist items proving BMad Builder reference consultation and BMB channel use for any skill/workflow edits.

## Verdict

**`pass-with-warnings`** — The ExpressPlan artifacts are concrete enough for FinalizePlan. The warnings are implementation-shaping risks, not phase blockers, provided FinalizePlan turns them into story acceptance criteria and does not proceed as if output routing, source adapter contracts, and orchestration mode are already settled.
