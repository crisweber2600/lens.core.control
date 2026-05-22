---
stable_id: feature:lens-seed-improvements
entity_type: feature
title: Lens Seed Improvements
status: intake-blocked
publication_state: draft
work_id: lens-seed-improvements
feature_id: lens-seed-improvements
created_at: '2026-05-22T00:00:00Z'
updated_at: '2026-05-22T12:00:00Z'
related_to:
  - nextlens-src-topdownlens
lifecycle_stage: preplan
---

# Lens Seed Improvements

## Goal

Turn the raw request "lens seed improvments" into a durable Lens work unit for improving the Lens seed flow.

## Scope

- Clarify what seed-related workflow needs to improve.
- Preserve the existing TopDownLens seed-flow context as related background.
- Identify whether this belongs to NextLens source work, Lens core workflow work, or another service area.
- Prepare the work for `/preplan` so requirements can be discovered before implementation scope is set.

## Non-Goals

- Do not implement seed command changes during intake.
- Do not edit living ledgers, generated projections, sprint status, story files, or implementation code.
- Do not assume the final target repository until preplan confirms ownership.

## Success Criteria

- The desired seed improvements are described as user-visible outcomes.
- Ownership, target repository, and lifecycle track are confirmed.
- Related TopDownLens seed concepts are either adopted, refined, or explicitly rejected.
- The next workflow has enough context to produce planning artifacts without hidden chat history.

## Current Understanding

The closest existing context is the TopDownLens seed concept: small raw seeds are explored, focused into implementation slices, then promoted into capabilities and follow-on seeds. This intake keeps that context available without treating it as final scope.

## PrePlan Blocker

The current PrePlan conductor cannot proceed cleanly for this work unit because the feature is intentionally domain/service-free:

- `lens-init-feature fetch-context` expects `lens-seed-improvements` to exist in the governance `feature-index.yaml`.
- `lens-constitution progressive-display` requires a legacy `domain` and `service` scope.
- The user selected the domain/service-free route, so this blocker is part of the redesign evidence rather than a reason to invent a legacy home.

## Risks And Open Questions

- The request may refer to a specific command, schema, or UX issue that has not been stated yet.
- The owning area is unknown: candidate areas include NextLens source, Lens core workflow commands, or planning documentation.
- Target repositories are unknown and should not be guessed during intake.
- The typo in the raw request was normalized in the feature ID as `improvements`; the original wording is preserved in `memory.md`.
- Existing PrePlan tooling still assumes the old domain/service/feature construct and blocks domain/service-free feature archives.

## Completion Evidence

- Planning artifacts approved.
- Implementation stories completed, if implementation is required.
- Validation evidence recorded.
- Ledger promotion and reporting handled after completion, if applicable.
