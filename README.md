# Market Atlas Codex Handoff

This repository is a **clean handoff package** for building a read-only, Kumu-inspired market atlas for the Stavanger corridor market.

The product is intended to help a human user explore:
- who the important actors are
- how they relate to each other
- what kind of demand or relevance each actor may have
- how to navigate the market through multiple saved views, filters, and profiles

The product is **not** a CRM, a sales pipeline tool, or a graph editor.

## What is in this repo
- `docs/` — normative product, UX, data-model, and testing specifications
- `schema/` — canonical JSON schema for the market-map data model
- `data/` — seed datasets and fixtures for implementation and testing
- `prompts/` — phase-specific prompts that can be pasted into Codex
- `AGENTS.md` — execution rules and reading order for Codex
- `INDEX.md` — master file index and authority levels
- `DECISIONS.md` — locked product and technical decisions

## Start here
Read these in order:
1. `AGENTS.md`
2. `README.md`
3. `INDEX.md`
4. `docs/00-product-thesis.md`
5. `docs/01-non-goals.md`
6. `docs/02-ontology.md`
7. `schema/market-map.schema.json`
8. `docs/13-acceptance-criteria.md`
9. `docs/15-codex-runbook.md`

## What to build first
Use `prompts/01-initial-build.md` as the first executable build prompt after reading the docs.

## Source of truth
The **canonical JSON model** is the source of truth. The UI must adapt to the data model, not the other way around.
