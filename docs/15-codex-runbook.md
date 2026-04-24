# 15 — Codex Runbook

## Purpose

This document tells Codex how to execute the build.

It is not a product brief. It is an execution order.

The key principle is simple:
**do not let implementation outrun ontology**.

Codex should treat the written spec and schema as source of truth and build in controlled increments.

---

## Read order

Before writing code, read these files in this exact order:

1. `docs/00-product-thesis.md`
2. `docs/01-non-goals.md`
3. `docs/02-ontology.md`
4. `schema/market-map.schema.json`
5. `docs/03-schema-explainer.md`
6. `docs/04-field-dictionary.md`
7. `docs/05-view-catalog.md`
8. `docs/06-interaction-spec.md`
9. `docs/07-profile-panel-spec.md`
10. `docs/08-layout-strategy.md`
11. `docs/09-visual-design-spec.md`
12. `docs/11-technical-architecture.md`
13. `docs/12-component-spec.md`
14. `docs/13-acceptance-criteria.md`
15. `docs/14-test-plan.md`
16. `docs/10-seed-data-rules.md`
17. `data/seed.market-map.json`

Do **not** start from UI styling, graph rendering, or component guesswork.

---

## Non-negotiable constraints

Codex must not build:
- an in-app editor
- drag-and-drop authoring
- authentication
- backend services
- CRM/pipeline features
- collaboration features
- speculative dashboards
- generic BI/reporting features

If a feature is not explicitly supported by the written spec, it is out of scope.

---

## Preferred stack

Use:
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

Do not substitute an editor-first graph library unless a hard implementation blocker is documented first.

---

## Build sequence

## Phase 1 — Scaffold the repo

### Goal
Create a clean project skeleton without implementation drift.

### Tasks
- initialize Vite + React + TypeScript
- add Tailwind
- add Sigma.js + graphology
- add Zustand
- add Zod
- add Vitest + RTL + Playwright
- create folder structure from the architecture docs
- wire linting and type checking

### Deliverable
A compiling project with empty shell components and no graph logic yet.

### Stop condition
Pause after scaffold if the project structure does not match `docs/12-component-spec.md`.

---

## Phase 2 — Data contract first

### Goal
Ensure the app can safely load and validate canonical graph data.

### Tasks
- create TypeScript types from the schema model
- create Zod parser mirroring the schema
- create validation helpers
- build data indexing utilities
- implement hard failure states for invalid datasets
- load `seed.market-map.json`

### Deliverable
A validated in-memory model with indexes for nodes, edges, sources, and views.

### Stop condition
Do not move on until the app can:
- load valid seed data
- reject invalid seed data
- surface readable error states

---

## Phase 3 — App shell and UI skeleton

### Goal
Build the non-graph shell before graph rendering.

### Tasks
- create app shell layout
- create top bar
- create left control rail
- create right profile panel shell
- create view switcher shell
- create filter drawer shell
- create legend shell
- create empty state and error state components

### Deliverable
A static interface skeleton with placeholder content.

### Stop condition
Do not wire graph interactions yet.

---

## Phase 4 — Graph runtime and render integration

### Goal
Render the canonical graph and support basic navigation.

### Tasks
- build graphology graph from parsed data
- initialize sigma renderer
- map node/edge attributes into render state
- support zoom and pan
- support hover detection
- support node click selection
- support selection synchronization with the profile panel

### Deliverable
A usable graph canvas with node selection and profile opening.

### Stop condition
Pause if selection works visually but the profile data is incorrect or incomplete.

---

## Phase 5 — Views and filters

### Goal
Make the map act like an atlas instead of a static graph.

### Tasks
- implement view switching from the JSON model
- implement node filters
- implement edge filters
- implement AND filter logic
- implement legend updates by active view
- implement node size switching by selected metric
- implement weak-edge hiding

### Deliverable
Graph behavior changes correctly when view and filter state changes.

### Stop condition
Pause if view logic is partially hard-coded instead of driven by the dataset.

---

## Phase 6 — Focus tools and neighborhood logic

### Goal
Support local exploration without overloading the main map.

### Tasks
- implement first-degree focus
- implement second-degree focus
- implement reset focus
- implement neighborhood highlighting
- ensure focus works with active filters and views

### Deliverable
A focused exploration mode that is readable and stable.

---

## Phase 7 — Profile panel richness

### Goal
Turn the profile panel into a real intelligence surface.

### Tasks
- render summary block
- render market significance block
- render demand and fit block
- render relationship block
- render source list
- handle missing optional fields cleanly

### Deliverable
Selecting a node reveals useful market intelligence, not just raw metadata.

---

## Phase 8 — URL state and persistence

### Goal
Make the atlas shareable and reload-safe.

### Tasks
- encode active view in URL
- encode filters in URL
- encode selected node in URL if practical
- restore state from URL on load
- handle malformed state safely

### Deliverable
Reloading preserves the exploration state.

---

## Phase 9 — Testing and hardening

### Goal
Bring the build up to the acceptance criteria.

### Tasks
- implement unit tests from the test plan
- implement integration tests
- implement end-to-end smoke tests
- fix schema regressions
- fix filter regressions
- fix profile/source mismatches
- verify performance on the seed dataset

### Deliverable
A stable v1 viewer that passes the acceptance criteria.

---

## Phase 10 — Stress test and cleanup

### Goal
Prepare the product for larger datasets.

### Tasks
- verify performance with larger fixture graphs
- tune label visibility
- tune weak-edge thresholds
- reduce re-renders
- document known limitations
- clean component boundaries

### Deliverable
A stable codebase that is ready for data expansion.

---

## Required working habits for Codex

### 1. Stay schema-driven
Do not invent fields in components.
If a field is needed, add it to the contract first.

### 2. Prefer data-driven views
Do not hard-code view behavior that should come from `views[]`.

### 3. Fail loudly on bad data
Silent corruption is worse than a hard error.

### 4. Keep components narrow
Do not collapse graph, controls, selection, and profile logic into one giant component.

### 5. Keep v1 read-only
Do not quietly add editing affordances.

---

## Definition of done

Codex should consider the implementation phase complete only when all of the following are true:
- the seed dataset validates,
- the app loads the seed dataset successfully,
- overview and other defined views work,
- filters and focus tools work,
- profile panel content is correct,
- URL state restoration works,
- tests cover the critical logic,
- and the output matches the acceptance criteria.

---

## Expected repo structure after implementation

```text
market-atlas/
  README.md
  package.json
  tsconfig.json
  vite.config.ts

  docs/
  schema/
  data/

  src/
    app/
    components/
    features/
    graph/
    hooks/
    lib/
    state/
    styles/
    types/

  tests/
    unit/
    integration/
    e2e/
```

---

## First Codex prompt

Use this as the starting prompt:

```text
Build a read-only, Kumu-inspired market atlas for exploring actors in the Stavanger corridor market.

This is not a CRM, not a pipeline tool, and not a map editor.

The app must:
- load a canonical JSON graph dataset
- render an interactive actor network
- support search, filters, focus, and saved views
- show rich node profiles in a right-side panel
- use actor nodes and relationship edges as the primary graph model
- keep demand and relevance mostly as node metadata
- support scale from 50 to 1,500 nodes
- prioritize clarity and exploration over editing

Use:
- React + TypeScript + Vite
- Sigma.js + graphology
- Zustand
- Zod
- Tailwind

Do not add:
- in-app editing
- authentication
- collaboration
- CRM features
- backend services
- speculative product features beyond the written spec

Treat the documents in /docs and /schema as the source of truth.
```
