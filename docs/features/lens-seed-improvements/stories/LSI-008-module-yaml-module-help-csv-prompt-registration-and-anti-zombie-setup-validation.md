---
feature: lens-seed-improvements
story_id: LSI-008
doc_type: story
status: ready-for-dev
title: "module.yaml, module-help.csv, Prompt Registration, And Anti-Zombie Setup Validation"
depends_on:
  - LSI-005
  - LSI-006
  - LSI-007
updated_at: "2026-05-23T00:00:00Z"
target_repo: TargetProjects/lens-dev/new-codebase/lens.core.src
epic: "Module Registration And Two-Tree Lifecycle Compatibility"
priority: P0
acceptance_gate: "Module Builder validation passes for module.yaml, module-help.csv, orphan references, duplicate menu codes, setup idempotency, and progressive disclosure."
---

# LSI-008: module.yaml, module-help.csv, Prompt Registration, And Anti-Zombie Setup Validation

## Summary

Keep the Lens module discoverable and installable by updating module registration assets and anti-zombie setup behavior alongside changed command surfaces.

## Context

Module Builder rules are a release gate, not documentation polish. Any changed Lens commands for doctor, projection, Salmon, promotion, topology, map audit, or reporting consumers must be reflected in module registration assets, help rows, prompt references, setup behavior, and validation checks.

The multi-skill Lens module must use setup-skill registration and anti-zombie merge behavior: remove previous Lens rows before inserting current rows.

## Scope

- Update `module.yaml`, `module-help.csv`, prompt stubs, setup assets, and relevant SKILL references for changed Lens commands.
- Ensure setup registration for the multi-skill Lens module removes stale Lens rows before inserting current rows.
- Keep SKILL frontmatter triggerable and concise.
- Move detailed behavior into scripts, assets, references, or prompt stages when SKILL bodies grow too procedural.
- Add tests or validation checks for orphan help entries, duplicate menu codes, missing files, broken references, inaccurate descriptions, missing capabilities, and oversized SKILL bodies.
- Preserve Module Builder validation as a release gate that LSI-010 must run or document.

## Out Of Scope

- Implementing parser, resolver, doctor, projection, Salmon, promotion, or lifecycle core behavior already owned by earlier stories.
- Treating module registration updates as a substitute for command contract tests.
- Adding broad UI or dashboard capabilities.
- Writing generated setup mirrors outside the approved target repo implementation surface.

## Acceptance Criteria

1. Given changed Lens commands for doctor, projection, Salmon, promotion, topology, map audit, or reporting consumers, when module assets are inspected, then `module.yaml` and `module-help.csv` list current capabilities, args, outputs, and dependency order.
2. Given setup registration runs twice, when module help or prompt entries are merged, then old Lens rows are removed before current rows are inserted and no duplicate Lens entries remain.
3. Given a help entry references a skill or prompt that does not exist, when module validation runs, then it fails with an orphan reference diagnostic.
4. Given two help rows share a duplicate menu code, when validation runs, then it fails with both row identifiers.
5. Given SKILL files grow procedural details that belong in scripts, assets, references, or prompts, when validation runs, then it warns or fails according to the module validation policy.
6. Given command output contracts changed, when help is rendered, then it describes check, write, explain, JSON, Salmon report, promotion review, and validation outputs accurately.

## Implementation Notes

- Apply BMAD Module Builder rules from architecture: plan capabilities first, register through `module.yaml` and `module-help.csv`, use setup-skill registration, and validate before release.
- Help entries should include current capability, arguments, phase/dependency order, outputs, and relevant gate behavior.
- Anti-zombie setup behavior should be idempotent. A second setup run must not duplicate Lens rows or leave stale prompt/help entries.
- SKILL frontmatter is always in context, so keep it concise and move detail to progressively disclosed scripts, assets, references, or prompt stages.
- This story must leave a validation path that LSI-010 can run or include in a VM-style transcript.

## Validation

- Module asset validation tests cover `module.yaml`, `module-help.csv`, orphan references, duplicate menu codes, missing files, broken references, inaccurate descriptions, missing capabilities, weak entries, and progressive disclosure risk.
- Setup anti-zombie idempotency tests run setup twice and assert stale Lens rows are removed and duplicate rows are absent.
- Help rendering tests verify check, write, explain, JSON, Salmon report, promotion review, and validation outputs are described accurately.
- Validation results are treated as a release gate and carried into LSI-010.

## Dependencies

- Depends on LSI-005, LSI-006, and LSI-007.
- Blocks LSI-010.
