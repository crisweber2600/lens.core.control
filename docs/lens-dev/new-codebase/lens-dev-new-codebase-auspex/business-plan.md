---
feature: lens-dev-new-codebase-auspex
doc_type: business-plan
status: draft
track: express
updated_at: "2026-05-23T00:00:00Z"
depends_on: []
blocks: []
key_decisions:
  - "Auspex is a read-only visibility plane, not a source of truth."
  - "MVP1 consumes the Derived Map, ledgers, audit outputs, and Salmon signals before introducing richer UI."
  - "ExpressPlan scope ends at business-plan.md, tech-plan.md, sprint-plan.md, and expressplan-adversarial-review.md."
open_questions: []
---

# Business Plan — Auspex Preferred Lens Workflow

## Problem

Lens now has the pieces for a Two-Tree operating model: a permanent Feature Archive, living landscape ledgers, a Derived Map projection, Salmon upstream-impact handling, map audits, and reporting snapshot concepts. The pieces are not yet expressed as a preferred end-to-end workflow that a maintainer can run consistently.

Without Auspex, stakeholders and agents must infer status by reading scattered feature folders, ledgers, and command outputs. That reintroduces the exact failure modes the Two-Tree topology is meant to solve: stale local memory, unclear source-of-truth boundaries, missing Salmon visibility, and reporting surfaces that accidentally become authoritative.

Auspex provides the preferred workflow and MVP1 reporting snapshot for seeing Lens work as a coherent system without making the report the system of record.

## Target Users

| User | Need |
|---|---|
| Lens maintainers | A repeatable workflow that rebuilds projections, audits map health, promotes ledger knowledge, identifies Salmon signals, and emits a concise report. |
| Feature authors | A clear way to see whether their feature is represented in the current topology and whether upstream impacts remain unresolved. |
| Reviewers and governance owners | Confidence that status reports trace back to feature.yaml, ledgers, derived maps, and audit evidence rather than hand-authored dashboard claims. |
| Future agents | Machine-readable status snapshots that can drive next-step selection, risk triage, and implementation handoff context. |

## Business Goals

1. **Make the preferred Lens workflow explicit** — define the run order and handoff expectations across setup, projection rebuild, map audit, ledger promotion, Salmon impact, and reporting snapshot.
2. **Keep source truth protected** — reports must read feature metadata, ledgers, and derived maps; they must not author governance state or replace lifecycle gates.
3. **Surface actionable status** — MVP1 should show active features, lifecycle phase, freshness, open decisions, blockers, Salmon signals, audit findings, and next recommended Lens command.
4. **Support Two-Tree adoption** — make Feature Archive versus Landscape versus Derived Map responsibilities visible to humans and agents.
5. **Prepare for all-epic lens-dev execution** — give `/finalizeplan` enough direction to produce stories that `lens-dev` can implement without scope ambiguity.

## Constitution and Governance Context

The resolved constitution for `lens-dev / new-codebase` permits `express` and `expressplan` and uses `gate_mode: informational`. Planning requires `business-plan` and `tech-plan`; dev requires stories. Review is enforced and stories are enforced before implementation.

Applicable prose constraints carried into implementation planning:

- Domain/service work must keep review enforced and preserve story artifacts before dev.
- Any `lens.core.src` change that creates or edits `lens-work` skills, agents, workflows, module metadata, or release prompts must use the BMad Builder implementation channel and consult the BMad Builder reference index.
- Public command wrappers and prompt handoffs must use the installed `lens.core/` wrapper path at the public boundary.
- Any coding story must include and execute a Given/When/Then validation scenario before closure.
- Auspex reporting must stay read-only and must not write directly to governance mirrors or feature metadata.

## Scope

### In Scope

- Define the preferred command workflow for Auspex:
  1. verify setup and feature context;
  2. rebuild the Derived Map;
  3. run map audit;
  4. promote approved ledger updates when available;
  5. evaluate Salmon upstream-impact signals;
  6. emit an MVP1 reporting snapshot.
- Introduce or complete the `lens-reporting-snapshot` / Auspex-facing reporting behavior in `TargetProjects\lens-dev\new-codebase\lens.core.src`.
- Specify a read-only report contract that can include `status.json`, `stakeholder-summary.md`, `read-only-report.md`, and optional dashboard-ready data.
- Preserve traceability from every reported status item back to source files and stable IDs.
- Add validation coverage for projection freshness, audit inclusion, Salmon inclusion, and read-only governance behavior.

### Out of Scope

- Building a full interactive web dashboard or long-running server.
- Making Auspex the source of truth for lifecycle, topology, ledger, or Salmon state.
- Direct edits to governance feature docs or generated governance mirrors.
- Creating FinalizePlan bundle artifacts during ExpressPlan.
- Replacing existing lifecycle gates, map audit, ledger promotion, or Salmon commands.
- Adding unrelated API endpoints; if any future `/v1/` routes are introduced, OpenAPI documentation becomes mandatory before merge.

## MVP1 Reporting Snapshot

MVP1 is a generated, read-only snapshot rooted in derived and authored truth. It should answer:

- What feature, service, domain, or program is currently active?
- Which lifecycle phase is each active feature in?
- Are required planning, review, story, and implementation artifacts present?
- Is the Derived Map fresh relative to its source files?
- What map-audit findings are open?
- What Salmon signals are open, accepted, resolved, or blocked?
- What ledgers were read, and which promoted facts are relevant?
- What command should a maintainer run next?

## Success Criteria

- A maintainer can run the preferred Auspex workflow from the workspace root without guessing command order.
- The generated report explicitly states it is read-only and names the source files it read.
- Derived-map freshness and map-audit findings are visible in the snapshot.
- Salmon signals are summarized with source references and next actions.
- Ledger context appears as consumed context, not as silently rewritten truth.
- Tests prove reporting generation does not mutate governance feature records or control planning docs outside the report output location.
- FinalizePlan can turn this plan into epics and stories without asking for PRD-, UX-, or architecture-named inputs.

## Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Reporting becomes treated as authoritative truth | High | Add explicit report metadata: `source: derived/read-only`, source file references, and no governance writes. |
| MVP1 grows into a dashboard build | Medium | Limit MVP1 to static machine-readable and markdown artifacts. Defer serve/UI behavior. |
| Salmon output shape is not stable enough | Medium | Use a narrow adapter that records raw source references and tolerant status fields. |
| Ledger promotion and reporting are conflated | High | Workflow sequencing may read ledger promotion results, but snapshot generation must not promote. |
| Agents skip BMB for lens-work skill edits | High | Stories must require BMB channel and BMad Builder docs consultation before implementation. |

## Handoff to FinalizePlan

FinalizePlan should consume only the express-track approved inputs: `business-plan.md`, `tech-plan.md`, and `sprint-plan.md`, plus `expressplan-adversarial-review.md` as review context after it exists. It should not require PRD, UX, or architecture documents for this feature.
