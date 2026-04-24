# AGENTS.md

## Project role
Build a **read-only, Kumu-inspired market atlas** for exploring actors in the Stavanger corridor market.

This project is **not**:
- a CRM
- a sales pipeline tool
- a graph editor
- a collaboration platform
- a backend-heavy data platform

## Source of truth
Treat the following as authoritative, in this order:
1. `README.md`
2. `INDEX.md`
3. `docs/00-product-thesis.md`
4. `docs/01-non-goals.md`
5. `docs/02-ontology.md`
6. `schema/market-map.schema.json`
7. `docs/03-schema-explainer.md`
8. `docs/04-field-dictionary.md`
9. `docs/05-view-catalog.md`
10. `docs/06-interaction-spec.md`
11. `docs/07-profile-panel-spec.md`
12. `docs/08-layout-strategy.md`
13. `docs/09-visual-design-spec.md`
14. `docs/11-technical-architecture.md`
15. `docs/12-component-spec.md`
16. `docs/13-acceptance-criteria.md`
17. `docs/14-test-plan.md`
18. `docs/15-codex-runbook.md`
19. `docs/16-seed-provenance.md`
20. `DECISIONS.md`

The files in `prompts/` are convenience prompts, not the source of truth.
The files in `data/fixtures/` are test inputs, not production data.

## Core build constraints
- Build **viewer first**, not editor first.
- Use **actor nodes** and **relationship edges** as the primary graph model.
- Keep **demand, relevance, and opportunity** as node metadata in v1.
- Preserve the **canonical JSON schema** as the runtime source of truth.
- Do not invent extra ontology categories or UI features unless explicitly required by the docs.
- Prefer clarity over cleverness.
- Prefer modular, testable components over a monolithic page.

## Expected stack
Use the stack specified in `docs/11-technical-architecture.md` unless an implementation blocker requires a documented deviation:
- React
- TypeScript
- Vite
- Sigma.js
- graphology
- Zustand
- Zod
- Tailwind CSS
- Vitest
- React Testing Library
- Playwright

## Build order
1. Scaffold the app shell and toolchain.
2. Implement schema-safe JSON loading and normalization.
3. Render the graph from the canonical dataset.
4. Implement views, filters, search, and focus.
5. Implement the right-side profile panel.
6. Implement polish, URL state, and tests.

## What to avoid
- Do not build in-app authoring.
- Do not add authentication.
- Do not add a database or API unless explicitly asked later.
- Do not add CRM or outreach workflow concepts.
- Do not turn every metadata field into a graph node.
- Do not treat the seed dataset as fully production-clean.

## Delivery standard
A phase is complete only when it meets the acceptance criteria in `docs/13-acceptance-criteria.md` and its relevant tests from `docs/14-test-plan.md`.
