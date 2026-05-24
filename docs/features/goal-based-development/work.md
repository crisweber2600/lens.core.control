---
stable_id: feature:goal-based-development
entity_type: feature
title: Goal Based Development
status: intake
publication_state: draft
work_id: goal-based-development
feature_id: goal-based-development
created_at: '2026-05-24T03:52:45Z'
updated_at: '2026-05-24T03:52:45Z'
related_to: []
extends: []
replaces: []
belongs_to: unknown
lifecycle_stage: preplan
lens_feature_id: ""
lens_track: ""
lens_phase: ""
lens_docs_path: ""
lens_constitution_root: ""
lens_feature_yaml_path: ""
lens_constitution_status: unknown
lens_preflight_status: passed
promotion_status: not_started
salmon_upstream: false
links: []
---

# Goal Based Development

## Goal

Turn the raw request "goal based development" into a durable Lens work unit for discovering and designing a development workflow that starts from explicit goals, preserves goal context through planning and implementation, and uses those goals to guide validation and completion.

## Scope

- Clarify what "goal based development" should mean in the Lens/NextLens workflow.
- Identify where goals are captured, refined, linked to plans/stories, and checked during development.
- Determine whether the feature affects planning artifacts, lifecycle commands, developer handoff, validation evidence, reporting, or implementation tooling.
- Prepare the work for `/preplan` so discovery can define user-visible outcomes before implementation scope is set.

## Non-Goals

- Do not implement lifecycle, planning, or code changes during intake.
- Do not edit living ledgers, generated projections, sprint status, story files, or implementation code.
- Do not attach this work to a target repository or service until ownership is discovered.

## Success Criteria

- A clear goal model exists for how development work should be initiated, traced, and completed.
- Planning and implementation artifacts can reference goals without relying on hidden chat history.
- Story creation and development handoff preserve goal intent, acceptance criteria, and validation evidence.
- Completion can be judged against the original goals and any approved refinements.
- Ownership, lifecycle track, and target repositories are confirmed before development begins.

## Current Understanding

The request is broad and likely describes a workflow capability rather than an immediately implementation-ready change. The work should begin in the full track at PrePlan so the problem, users, artifacts, and lifecycle touchpoints can be discovered before technical design.

## Risks And Open Questions

- The term may refer to a product workflow, a development methodology, a CLI command behavior, or documentation conventions.
- Ownership is unknown; candidate areas include Lens lifecycle orchestration, NextLens planning artifacts, story generation, dev-session tracking, and reporting.
- The target repository is unknown and should not be guessed during intake.
- The feature may overlap with existing seed or work-intake concepts, but no direct matching archive or documentation was found.

## Completion Evidence

- Planning artifacts approved.
- Implementation stories completed, if implementation is required.
- Validation evidence recorded against explicit goals.
- Ledger promotion and reporting handled after completion, if applicable.
