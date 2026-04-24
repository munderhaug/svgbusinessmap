# INDEX

## Purpose
This file explains what each file is for and whether it is authoritative.

## Authority levels
- **Normative**: Codex should treat this as instruction or contract.
- **Reference**: Helpful context, but not the controlling source.
- **Test data**: Input files for validation or rendering tests.

## Root files
| File | Role | Authority |
|---|---|---|
| `AGENTS.md` | Codex execution rules and reading order | Normative |
| `README.md` | Project summary and starting instructions | Normative |
| `INDEX.md` | File index and authority guide | Normative |
| `DECISIONS.md` | Locked product and technical decisions | Normative |

## Docs
| File | Role | Authority |
|---|---|---|
| `docs/00-product-thesis.md` | Product definition and purpose | Normative |
| `docs/01-non-goals.md` | Scope guardrails | Normative |
| `docs/02-ontology.md` | Domain model and graph ontology | Normative |
| `docs/03-schema-explainer.md` | Human explanation of the schema | Normative |
| `docs/04-field-dictionary.md` | Field meanings, types, display intent | Normative |
| `docs/05-view-catalog.md` | Saved view definitions | Normative |
| `docs/06-interaction-spec.md` | Interaction and controls behavior | Normative |
| `docs/07-profile-panel-spec.md` | Profile panel information architecture | Normative |
| `docs/08-layout-strategy.md` | Layout principles and rendering strategy | Normative |
| `docs/09-visual-design-spec.md` | Visual encoding system | Normative |
| `docs/10-seed-data-rules.md` | Seed-data construction rules | Normative |
| `docs/11-technical-architecture.md` | Runtime stack and architecture | Normative |
| `docs/12-component-spec.md` | Component responsibilities | Normative |
| `docs/13-acceptance-criteria.md` | Definition of done | Normative |
| `docs/14-test-plan.md` | Test coverage expectations | Normative |
| `docs/15-codex-runbook.md` | Build sequencing and execution guidance | Normative |
| `docs/16-seed-provenance.md` | Provenance and caveats for the seed dataset | Normative |

## Schema
| File | Role | Authority |
|---|---|---|
| `schema/market-map.schema.json` | Canonical machine-readable contract | Normative |

## Data
| File | Role | Authority |
|---|---|---|
| `data/seed.market-map.json` | Main seed dataset | Test data |
| `data/seed.market-map.min.json` | Smaller seed dataset | Test data |
| `data/fixtures/minimal-valid.json` | Minimal valid fixture | Test data |
| `data/fixtures/invalid-schema.json` | Intentionally invalid fixture | Test data |
| `data/fixtures/sparse-graph.json` | Sparse graph fixture | Test data |
| `data/fixtures/dense-graph.json` | Dense graph fixture | Test data |

## Prompts
| File | Role | Authority |
|---|---|---|
| `prompts/01-initial-build.md` | First Codex build prompt | Reference |
| `prompts/02-graph-runtime.md` | Graph runtime implementation prompt | Reference |
| `prompts/03-filters-and-views.md` | Filters and views implementation prompt | Reference |
| `prompts/04-profile-panel.md` | Profile panel implementation prompt | Reference |
| `prompts/05-polish-and-tests.md` | Final polish and test hardening prompt | Reference |
