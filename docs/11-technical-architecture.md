# 11 — Technical architecture

## Purpose

This document defines the preferred technical architecture for version 1 of the market atlas.

It answers:
- what stack to use
- why that stack is preferred
- what is in scope for version 1
- how data should flow through the app
- what should remain out of scope until later

This is the implementation baseline Codex should follow.

---

## Architecture goals

The architecture for version 1 must optimize for:

1. **fast iteration**
2. **clear data contracts**
3. **strong local rendering performance**
4. **read-only exploration**
5. **low operational complexity**
6. **easy future extension**

This means the first version should be intentionally small in infrastructure terms, even if the product thinking is rich.

---

## High-level architecture decision

### Preferred approach

Build a **frontend-only web application** that loads a canonical JSON graph dataset, validates it locally, transforms it into a graph runtime model, and renders it in a Kumu-inspired exploration interface.

### What this means in practice
- no backend API in version 1
- no database in version 1
- no authentication in version 1
- no server-managed graph state
- no in-app editing in version 1

This keeps the product honest and drastically reduces build complexity.

---

## Core stack

## 1. App framework

### Recommendation
- **React**
- **TypeScript**
- **Vite**

### Why
- React is the safest choice for a structured interactive UI with panels, controls, and shared state
- TypeScript is non-negotiable for schema-driven work and long-term maintainability
- Vite gives a fast, modern development setup with low ceremony

---

## 2. Graph rendering layer

### Recommendation
- **sigma.js** for rendering
- **graphology** for the graph model and graph operations

### Why sigma.js
Sigma.js is explicitly designed for visualizing graphs of thousands of nodes and edges using WebGL, and it is built to work in symbiosis with graphology. Its own docs position it well for graph exploration, hover neighborhoods, and graph rendering at a scale that fits this product better than editor-first libraries. See the [Sigma.js homepage](https://www.sigmajs.org/) and [Sigma docs](https://www.sigmajs.org/docs/).

### Why graphology
Graphology provides a robust graph object plus graph algorithms and utilities, and sigma.js itself treats graphology as its data backend. That makes the pair a natural fit for a graph viewer where the runtime graph and the render graph should stay aligned. See [Graphology](https://graphology.github.io/).

### Why not an editor-first graph library
The product is not a node editor in version 1. Using an editor-first library would bias the implementation toward drag/edit workflows the product explicitly rejects.

---

## 3. Schema validation layer

### Recommendation
- **Zod** for runtime parsing in the app
- canonical JSON Schema file as the contract artifact

### Why
- JSON Schema is the transport contract and handoff artifact
- Zod is convenient inside the app for runtime parsing, user-facing errors, and TypeScript type inference

### Rule
The app must not assume the dataset is valid.
It must validate before render.

---

## 4. State management

### Recommendation
- **Zustand**

### Why
The app has enough interaction state to need structure, but not enough global complexity to justify something heavier.

Typical state domains:
- active view
- active filters
- selected node/edge
- focus state
- metric mode
- legend state
- camera/presentation state

Zustand is appropriate for this scale and avoids unnecessary ceremony.

---

## 5. Styling system

### Recommendation
- **Tailwind CSS** for layout and UI scaffolding
- CSS variables / tokens for semantic color system

### Why
- fast implementation speed
- strong consistency for panels, controls, spacing
- easy token-driven styling
- low friction for Codex execution

### Important rule
Do not hard-code semantic colors or state styles inside components.
Use tokens.

---

## 6. Testing stack

### Recommendation
- **Vitest** for unit tests
- **React Testing Library** for UI/component behavior
- **Playwright** for end-to-end interaction testing

### Why
This combination is a strong default for a frontend app with a non-trivial interaction model.

---

## 7. Build and package model

### Version 1 recommendation
- static frontend app
- local JSON dataset file committed or loaded from local/public directory
- no SSR required
- no backend deployment dependency

This keeps hosting simple and lets the app be previewed anywhere a static site can run.

---

## Runtime data flow

The runtime should follow this flow:

1. load canonical JSON dataset
2. validate dataset
3. normalize and index dataset
4. build graphology graph instance
5. apply selected view rules
6. derive visible subset and style data
7. render through sigma.js
8. open profile/controls around the rendered graph

### Important principle
The JSON dataset is the source of truth.
The runtime graph is a computed representation of that source.
The UI state sits on top of both.

---

## Recommended application layers

## Layer 1 — Canonical data layer

This is the raw dataset as defined by:
- `market-map.schema.json`
- seed or production graph JSON file

The app should not mutate this object directly.

## Layer 2 — Parsed model layer

A validated, typed representation of the canonical data.

Typical responsibilities:
- schema validation
- type-safe access
- reference integrity checks
- normalized indexes by node ID, edge ID, source ID, view ID

## Layer 3 — Graph runtime layer

A graphology graph built from the parsed data.

Typical responsibilities:
- storing runtime graph state
- neighborhood queries
- degree counts or other graph metrics
- feeding render attributes to sigma

## Layer 4 — View application layer

A derived state layer that takes:
- current view
- filters
- focus state
- metric mode

and produces:
- visible nodes
- visible edges
- style props
- legend content

## Layer 5 — UI layer

React components for:
- shell
- controls
- graph canvas
- profile panel
- legends
- states and messages

---

## File system architecture

Recommended structure:

```text
src/
  app/
    App.tsx
    AppShell.tsx
    routes.ts

  components/
    TopBar.tsx
    LeftControlPanel.tsx
    RightProfilePanel.tsx
    LegendPanel.tsx
    SearchBar.tsx
    EmptyState.tsx
    ErrorState.tsx

  graph/
    buildGraph.ts
    applyView.ts
    applyFocus.ts
    applyFilters.ts
    layout.ts
    metrics.ts
    styling.ts

  features/
    views/
    filters/
    selection/
    profile/
    search/
    legend/

  lib/
    schema.ts
    parseDataset.ts
    integrityChecks.ts
    fieldAccess.ts

  state/
    useAppStore.ts
    selectors.ts

  styles/
    tokens.css
    globals.css

  types/
    schema.ts
    ui.ts
    graph.ts
```

This keeps the graph logic separate from the React component layer.

---

## Data loading strategy

### Version 1 recommendation
- load a JSON file from a known local/public path
- optionally allow a user-selected local file later

### Required behaviors
- schema validation on load
- referential integrity check after validation
- readable error state if invalid

### Required integrity checks
- all edges reference valid node IDs
- all source refs referenced by nodes/edges exist
- `default_view_id` must reference a real view if present
- `layout_preset_id` in views must reference a real preset if present

---

## Graph runtime strategy

### Graph construction
Use graphology as the runtime graph store.

Recommended node attributes in runtime graph:
- original canonical data
- resolved style fields
- visibility state
- selection state
- focus distance if relevant

Recommended edge attributes in runtime graph:
- original canonical data
- visibility state
- resolved styling state

### Why this matters
The renderer should not be responsible for ontology semantics.
It should render already-resolved graph state.

---

## Rendering strategy

### Sigma responsibilities
- render nodes and edges
- handle camera
- expose interaction events
- support layer-based rendering behavior

Sigma’s documentation is clear that it uses WebGL for graph rendering and separates graph data/model concerns from rendering concerns, which is exactly what we want. It also exposes specialized layers for labels, hover states, and interaction handling, which matches the atlas interaction model well. See [Sigma renderers](https://www.sigmajs.org/docs/advanced/renderers/) and [Sigma layers](https://www.sigmajs.org/docs/advanced/layers/).

### Recommended renderer behavior
- nodes and edges rendered from graphology data
- labels selectively rendered
- hover and selection drawn with elevated clarity
- graph canvas treated as one subsystem, not the center of all business logic

---

## View application strategy

Each saved view should be treated as a declarative input to a view engine.

### View engine responsibilities
- apply node filters
- apply edge filters
- resolve style rules
- apply layout mode
- set interaction defaults
- update legend state

### Important rule
A view should not directly mutate canonical data.
It should only produce derived runtime state.

---

## Search strategy

### Recommendation
Build a lightweight local search index in memory from parsed node data.

Search fields:
- label
- aliases
- sector
- subsector
- municipality
- summary fields
- relevance framing text

### Implementation note
A simple in-memory ranked search is enough for version 1.
Do not introduce a dedicated search backend.

---

## Layout strategy in code

### Recommendation
Support three layout pathways:
- force layout
- preset layout
- geo layout

### Implementation idea
`layout.ts` should expose something like:
- `applyForceLayout(graph, options)`
- `applyPresetLayout(graph, preset)`
- `applyGeoLayout(graph, options)`

The selected view decides which pathway to invoke.

### Important rule
Layout changes should be event-driven, not incidental.
Do not recompute layout on every hover or panel change.

---

## Metrics strategy

### Version 1 recommendation
Support a small number of metrics only:
- degree
- optional weighted degree later
- commercial_attractiveness from data
- short_term_opportunity from data
- access_difficulty from data

Kumu’s metrics approach is useful as an inspiration here: metrics should be overlays that help interpret a network, not a replacement for the primary model. Degree is a practical first metric because it helps identify local connectors/hubs. See [Kumu metrics](https://docs.kumu.io/guides/metrics).

### Important rule
Do not build a general-purpose graph analytics workbench in version 1.

---

## Error handling architecture

The app should distinguish between:

### 1. Load errors
Example:
- JSON file missing

### 2. Schema errors
Example:
- field missing or wrong type

### 3. Integrity errors
Example:
- edge references missing node

### 4. Runtime UI errors
Example:
- invalid view state after filter change

Each should surface differently, and none should fail silently.

---

## Performance strategy

### Version 1 target
Comfortable interaction with roughly 50–1,500 nodes and corresponding edges, assuming reasonable view filtering.

### Required techniques
- selective labels
- hidden weak edges by default in dense views
- derived-state memoization where useful
- avoid full graph rebuild on small UI changes
- avoid re-layout on hover or panel events
- use sigma.js WebGL rendering rather than SVG/canvas-only graph rendering for the main graph surface

Sigma.js explicitly positions itself for graph visualization at the scale of thousands of nodes and edges, which is one of the main reasons it is the preferred renderer here. See the [Sigma.js homepage](https://www.sigmajs.org/).

---

## Non-goals in architecture

Do not add these in version 1:
- backend database
- real-time collaboration
- authentication/authorization
- autosave editing workflows
- cloud graph persistence
- server-side graph preprocessing pipeline as a requirement
- in-app graph authoring engine

These can all wait.

---

## Codex execution guidance

Codex should build this app in phases.

### Phase 1
- scaffold app
- wire schema validation
- load seed dataset
- render minimal shell

### Phase 2
- build graph runtime
- render nodes and edges
- support selection and profile open

### Phase 3
- add views, filters, focus, and legends

### Phase 4
- add styling polish and performance tuning

### Phase 5
- test with larger datasets

This phased approach fits Codex’s strength as a coding agent working through scoped tasks rather than improvising a whole product in one undisciplined pass. OpenAI’s published Codex materials describe it as suited to parallelized, scoped software work across isolated environments, which is exactly how this build should be approached. See [Introducing the Codex app](https://openai.com/index/introducing-the-codex-app/) and [Introducing Codex](https://openai.com/index/introducing-codex/).

---

## Bottom line

The preferred architecture is deliberately simple:

- React + TypeScript + Vite
- sigma.js + graphology
- Zod + JSON Schema
- Zustand
- Tailwind
- static local data source

That gives you a Kumu-inspired market atlas with enough architectural discipline to scale, without turning version 1 into infrastructure theater.
