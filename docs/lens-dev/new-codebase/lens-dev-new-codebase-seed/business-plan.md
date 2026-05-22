---
feature: lens-dev-new-codebase-seed
doc_type: business-plan
status: draft
track: express
title: "lens-new-seed business plan"
updated_at: 2026-05-22T15:55:00Z
---

# lens-new-seed Business Plan

## Executive Summary

`lens-new-seed` introduces the seed architecture for Auspex as the first product feature under `specdriven/adeptus`, while also replacing Lens control-repo planning branch assumptions with a flat workflow. Auspex provides a thin stakeholder-facing reporting UI over delivery status and artifacts without requiring reporting consumers to have direct GitHub repository access.

MVP1 is read-only. It focuses on project rollup, feature lifecycle visibility, artifact reading, source freshness, and automated refresh. The Lens workflow change is required because the Auspex topology moves source truth toward explicit metadata, durable feature archives, derived projections, and living ledgers rather than branch placement.

## Problem

Delivery evidence exists across governance docs, control docs, and target repositories, but discovery is path-dependent and generally assumes developer access. Non-developer stakeholders need status and artifact visibility without manual report assembly or direct repository permissions.

The current Lens branching model also embeds planning validity into branch topology. That conflicts with the target Auspex model, where artifact identity and validity should come from explicit metadata, stable IDs, and lifecycle state.

## Users

- Product Owners and Scrum Masters who need weekly planning and review visibility.
- Leadership stakeholders who need read-only delivery health rollups.
- Developers who need quick status checks without hunting through repository paths.
- Operations and security stakeholders who need auditable access and refresh behavior.

## Goals

- Provide a read-only UI surface for project, domain, service, and feature status.
- Render core planning and delivery artifacts from `/Docs` and `/TargetProjects`.
- Remove direct GitHub access requirements for reporting consumers.
- Refresh reporting data automatically within a 24-hour MVP1 freshness SLA.
- Establish stable contracts for future pod-of-pods rollup.
- Flatten Lens control branching so planning and implementation validity come from metadata and gates, not `{featureId}-plan` or `{featureId}-dev` branch placement.

## Non-Goals

- Artifact authoring or write-back in Auspex MVP1.
- Replacing Lens lifecycle orchestration.
- Full enterprise cross-pod federation.
- Cross-authority governance mutation.
- Big-bang migration of all legacy Lens features.

## Scope

### In Scope

- Team-pod dashboard with project, domain, service, and feature rollups.
- Feature lifecycle table with phase, owner, status, and timestamps.
- Markdown artifact reader for PRD, architecture, epics, stories, and sprint status.
- Search and filter by domain, service, feature, phase, owner, and status.
- Automated refresh pipeline with visible freshness timestamp and source failure state.
- Viewer-only access abstraction.
- EPLX Kubernetes deployment baseline.
- Lens metadata and branching changes needed for flat workflow adoption:
  - stable IDs and `belongs_to`
  - `docs/features/` archive path for new work
  - derived topology projection rebuild command
  - topology doctor/audit command
  - Salmon upstream-impact support
  - removal of planning-branch validity assumptions

### Out of Scope

- Editing governance artifacts through Auspex.
- Manual-script-only reporting operations.
- Mutating governance outside approved Lens boundaries.
- Requiring all historical features to migrate before MVP1 value is delivered.

## Success Metrics

- Stakeholders can self-serve status and artifacts without developer mediation.
- Manual status-report preparation effort is reduced.
- Viewer users can access reporting without direct GitHub repo permissions.
- Pod deployment is reproducible for more than one team.
- Reporting data meets the 24-hour freshness SLA.
- Lens flat workflow can initialize, plan, commit, and complete feature work without relying on `{featureId}-plan` or `{featureId}-dev` branches.

## Risks

| Risk | Impact | Mitigation |
|---|---|---|
| Dual truth drift between feature archive and landscape ledgers | Stakeholders may see stale conclusions | Make promotion explicit, visible, and audit-backed |
| Derived map drift | Reporting topology may become stale | Rebuild projection from source; never hand-edit it |
| Over-modeling | Teams may create hierarchy too early | Keep domain/program depth optional |
| Salmon signal overload | Upstream alerts may become noise | Default Salmon findings to advisory unless recursive checks find material impact |
| Branch migration disruption | Existing feature flows may break | Introduce flat topology incrementally with compatibility tests |

## Governance Constraints

- Express track is permitted for `lens-dev/new-codebase`.
- Planning must produce `business-plan` and `tech-plan`.
- Dev must produce story artifacts and pass review.
- Any `lens-work` skill or workflow modification must consult BMad Builder guidance and use BMB expectations as the implementation channel.
- Public wrappers must keep installed `lens.core/` boundary paths.

