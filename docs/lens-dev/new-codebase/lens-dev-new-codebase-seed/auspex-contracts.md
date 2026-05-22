---
feature: lens-dev-new-codebase-seed
doc_type: contract
status: draft
track: express
title: "Auspex seed contracts"
updated_at: 2026-05-22T00:00:00Z
---

# Auspex Seed Contracts

Auspex MVP1 consumes read-only Lens reporting data. This seed does not build the UI; it defines the minimum source contracts the UI and refresh pipeline can rely on.

## Topology Source

Authored source truth lives in repo artifacts, not generated maps.

- Feature archive: `docs/features/**` for new feature records and durable work history.
- Living landscape: service, domain, and program ledgers under docs-managed landscape paths.
- Derived map: generated output rebuilt from authored metadata and never hand-edited.

Minimum governed metadata:

```yaml
stable_id: feature:<slug>
entity_type: feature
title: <display title>
status: active
publication_state: published
belongs_to: service:<slug>
updated_at: 2026-05-22T00:00:00Z
feature_id: <lens feature id>
track: express
phase: expressplan
docs_path: docs/<domain>/<service>/<feature>
target_repos: []
promotion_status: pending
```

## Derived Map

The rebuild command produces a JSON projection and a human-readable Markdown mirror. The projection must include:

- `module: lens`
- `report_type: governance_map`
- `generated_at`
- `source_model: authored_metadata`
- `doctor.status`, `doctor.blocking_count`, and `doctor.advisory_count`
- `entities[]` with `stable_id`, `entity_type`, `title`, `status`, `publication_state`, `belongs_to`, `updated_at`, and `path`

The rebuild command must stop on blocking doctor findings unless an explicit preview/force flag is supplied.

## Doctor Findings

Doctor/audit output is non-mutating JSON. Blocking findings prevent projection rebuilds and stakeholder-ready reporting.

Minimum finding shape:

```json
{
  "severity": "blocking",
  "code": "missing_parent_entity",
  "stable_id": "feature:example",
  "entity_type": "feature",
  "path": "docs/features/example/feature.md",
  "problem": "Parent service:example was not found in authored sources.",
  "recommended_fix": "Create the parent ledger or correct belongs_to."
}
```

Required checks:

- missing required fields
- duplicate stable IDs
- invalid stable ID prefix for entity type
- missing or mismatched `belongs_to`
- parent cycles
- broken local links
- completed feature not promoted to a living ledger
- Lens lifecycle context mismatch when Lens metadata is present

## Salmon Signals

Salmon records upstream-impact evidence discovered downstream. MVP1 reporting treats `blocking` signals as promotion/reporting blockers and lower severities as advisory.

Minimum signal fields:

- `id`
- `severity`
- `status`
- `source`
- `upstream_targets`
- `finding`
- `evidence_refs`
- `recommended_action`

Default severity routing:

| Severity | Reporting treatment |
|---|---|
| low | advisory |
| medium | advisory |
| high | advisory requiring triage |
| blocking | blocker until resolved or superseded |

## Reporting Snapshot

Auspex consumes a read-only reporting snapshot shaped for dashboard, artifact reader, search, freshness, and access display.

Minimum top-level fields:

- `module: lens`
- `report_type: reporting_snapshot`
- `created_at`
- `scope`
- `overall_status`
- `summary_cards`
- `blocking`
- `advisory`
- `features`
- `ledgers`
- `projection`
- `salmon_impacts`
- `artifact_index`
- `facets`
- `search_fields`
- `freshness`
- `refresh`
- `access`

MVP1 freshness contract:

- `refresh.mode` is one of `manual`, `scheduled`, or `ci`.
- `refresh.generated_from` lists source roots or artifacts.
- `freshness.generated_at` records snapshot generation time.
- `freshness.max_age_hours` defaults to `24`.
- stale or failed sources are visible in `blocking` or `advisory` findings.

MVP1 access contract:

- `access.model` records viewer-only access for reporting consumers.
- No snapshot field grants write-back capability.
- Source paths in `artifact_index` provide traceability without requiring consumers to have direct GitHub repository access.
