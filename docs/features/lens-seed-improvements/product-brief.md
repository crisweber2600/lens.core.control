---
title: "Product Brief: Lens Seed Improvements"
status: "complete"
created: "2026-05-22"
updated: "2026-05-22"
inputs: ["brainstorm.md", "research.md", "feature.yaml"]
feature_id: "lens-seed-improvements"
doc_type: "product-brief"
research_limitation: "External web research was unavailable; brief is grounded in local Lens artifacts and user-provided brainstorming notes."
---

# Product Brief: Lens Seed Improvements

## Executive Summary

Lens needs a topology model that matches how durable planning knowledge actually grows. The current fixed `domain > service > feature` construct gives each feature a clean work pocket, but it also traps reusable service, domain, and program truth inside those pockets. Over time, maintainers and downstream planning agents lose sibling context, predecessor decisions, and cross-feature implications unless they already know where to look.

Lens Seed Improvements proposes the Two-Tree Model with Derived Map. Features become immutable delivery facts stored permanently under `docs/features/`. Service, domain, and program knowledge becomes reorganizable living landscape ledgers. The governance map becomes a derived cache rebuilt from frontmatter rather than a hand-authored source of truth. Stable IDs and explicit `belongs_to` parent references replace path-as-identity, while explicit draft and published metadata replaces planning branch dependence.

The MVP should be incremental: establish stable IDs and parent refs, make new work use `docs/features/`, create one pilot ledger, add a projection rebuild command, add a lightweight topology doctor, and introduce Salmon later as a first-class upstream-impact signal once the base model is stable.

## The Problem

Lens currently over-identifies folders, paths, and branches with knowledge authority. A feature path is treated like identity. A planning branch is treated like status. A fixed domain/service hierarchy is treated like the natural structure for every kind of work.

That model breaks down for organic, multi-feature, team-scale planning. Completed features contain durable learnings that belong to services, domains, or programs, but those learnings remain buried in feature-local artifacts. New agents can miss predecessor context. Maintainers face drift between governance indexes, feature records, and living project knowledge. The current feature metadata for this effort shows the failure clearly: the work is blocked by the legacy domain/service/feature gate even though the purpose of the work is to remove that assumption.

The cost is not cosmetic folder churn. It is reduced trust in planning artifacts, weaker reuse of prior work, and unnecessary pressure to preserve old topology because moving files would appear to change identity.

## The Solution

Adopt the Two-Tree Model with Derived Map.

- The feature tree stores immutable delivery facts under `docs/features/`. Feature folders are permanent records and do not move when the landscape changes.
- The landscape tree stores living service, domain, and program ledgers. These ledgers are allowed to reorganize as understanding matures.
- The governance map is derived from source frontmatter and rebuilt by tooling. It is a cache, not the authority.

In this model, a feature can start with a stable ID and an unresolved or simple parent reference, then later attach to a service, domain, or program without moving its historical record. Maintainers gain a stable archive for auditability and a flexible landscape for human understanding. Downstream planning agents gain a queryable map that can explain where every relationship came from.

## What Makes This Different

The design separates three ideas Lens currently blends together: delivery evidence, living product knowledge, and routing/projection data. Features are facts. The landscape is interpretation. The map is a cache.

That separation avoids two common traps. It does not centralize all truth into a manually maintained governance document, which would create another drift point. It also does not jump straight to a pure graph model, which would be powerful but too abstract and heavy for the current lifecycle. The proposal keeps authoring local and auditable while allowing topology to evolve through explicit metadata.

Auspex-style stakeholder reporting can become a downstream consumer of the derived projection, but it is not the product. The primary product is a more trustworthy Lens planning topology for maintainers, conductors, and downstream BMAD agents.

## Who This Serves

Primary users are Lens maintainers and lifecycle conductors who need reliable feature state, reusable knowledge, and governance views without hand-maintaining parallel maps.

Secondary users are downstream planning and implementation agents that need predecessor context, sibling relationships, ownership clues, and phase readiness without guessing from paths or branches.

Stakeholders and reporting surfaces benefit indirectly because derived maps can provide clearer status, ownership, and impact views.

## MVP Scope

The first release should prove the topology shift without requiring a full historical migration.

- Add stable IDs and explicit `belongs_to` parent references for new feature and ledger records.
- Keep new feature work permanently under `docs/features/`.
- Create one pilot living ledger, likely at the service level, to test promotion of durable learnings.
- Add a deterministic projection rebuild command that scans frontmatter and rebuilds or checks the derived map.
- Add a lightweight topology doctor for duplicate IDs, broken parent refs, orphaned records, moved feature paths, and projection drift.
- Replace planning branch status assumptions with explicit draft and published metadata for new two-tree flows.
- Defer Salmon blocking behavior, broad domain/program ledger expansion, and historical migration until the base model proves useful.

## Success Criteria

Lens Seed Improvements is working when new work can proceed without a fake legacy domain/service placeholder, and maintainers can answer topology questions from metadata rather than memory.

Success signals include: every new feature has a stable ID and permanent `docs/features/` record; parent references resolve or are explicitly marked unresolved; the projection rebuild is deterministic and explainable; topology doctor findings are actionable; one pilot ledger captures durable learnings from multiple features; downstream agents can find predecessor and sibling context through the derived map; branch state no longer acts as planning truth for new flows.

## Risks and Guardrails

Dual truth drift is the main risk. Guardrail: treat feature and ledger frontmatter as source inputs and the governance map as a rebuildable cache that tooling can check.

Legacy validator lock-in may keep blocking the new model. Guardrail: add compatibility deliberately, with read support for legacy structures and stricter write rules for new two-tree work.

Over-modeling could bury the MVP under program, domain, service, graph, and Salmon semantics all at once. Guardrail: start with stable IDs, parent refs, one pilot ledger, projection rebuild, and doctor checks.

Salmon can become noisy if every upstream note blocks work. Guardrail: make Salmon advisory by default and block only material discovered impacts that contradict published truth or lifecycle-critical assumptions.

Historical migration could slow adoption. Guardrail: adopt the new model for new work first and migrate high-value legacy knowledge selectively.

## Vision

If successful, Lens becomes a system where planning knowledge is both auditable and alive. Feature records remain stable delivery evidence. Landscape ledgers become the maintained memory of services, domains, and programs. Derived maps give humans, agents, validators, and reporting tools a consistent view without turning paths into identity.

The long-term vision is not a new folder convention. It is a planning substrate where Lens can reorganize what it knows without losing what happened, surface upstream impacts before they surprise downstream work, and let future agents reason from explicit metadata instead of inherited topology assumptions.