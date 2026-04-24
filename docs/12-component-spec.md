# 12 — Component spec

## Purpose

This document defines the frontend component architecture for version 1 of the market atlas.

Its job is to tell Codex:
- what components exist
- what each component owns
- what each component should not own
- how data and actions should flow through the UI

This is not a pixel-perfect design spec.
It is a structural UI contract.

---

## Component design principles

### 1. Separate graph logic from UI chrome

The graph canvas should not own filter state, view definitions, or profile rendering.

### 2. Keep components narrow in responsibility

Each major surface should do one thing well.

### 3. Shared state should live in the store, not in prop spaghetti

The UI will have enough cross-cutting interaction state that global app state should be explicit.

### 4. Components should follow the product ontology

If the ontology says the app is actor-centric and read-only, the component tree should reflect that.

---

## Top-level component tree

Recommended high-level structure:

```text
App
└── AppShell
    ├── TopBar
    │   ├── AppTitle
    │   ├── ViewTitle
    │   ├── SearchBar
    │   └── ResetActions
    ├── LeftControlPanel
    │   ├── ViewSwitcher
    │   ├── FilterPanel
    │   ├── MetricSelector
    │   ├── EdgeVisibilityControl
    │   └── LegendToggle
    ├── GraphWorkspace
    │   ├── GraphCanvas
    │   ├── GraphOverlayStatus
    │   └── EmptyOrErrorOverlay
    ├── RightProfilePanel
    │   ├── EmptyProfileState
    │   ├── NodeProfileView
    │   └── EdgeProfileView
    └── BottomUtilityBar (optional)
        ├── LegendPanel
        ├── ResultCount
        └── StatusSummary
```

---

## Component catalog

# 1. `App`

## Purpose
Root application bootstrap.

## Responsibilities
- initialize app
- load dataset
- validate dataset
- mount app shell
- decide between valid app / error state

## Must not own
- graph rendering logic
- filter UI logic
- profile rendering details

---

# 2. `AppShell`

## Purpose
Primary layout container.

## Responsibilities
- arrange main regions
- coordinate shell-level responsive behavior
- pass stable structural props to major surfaces

## Must not own
- heavy business logic
- dataset parsing
- graph algorithms

---

# 3. `TopBar`

## Purpose
Persistent top interaction bar.

## Responsibilities
- show app title
- show active view label
- host search
- host reset/recenter actions
- optionally show short view description access

## Child components
- `AppTitle`
- `ViewTitle`
- `SearchBar`
- `ResetActions`

## Must not own
- search indexing logic
- filter state
- graph selection logic

---

# 4. `SearchBar`

## Purpose
Primary actor lookup control.

## Responsibilities
- capture search input
- display ranked results
- allow keyboard and mouse selection
- dispatch selection/focus actions

## Inputs
- indexed search data
- current search query
- search result list

## Outputs
- select node action
- focus node action
- update query action

## Must not own
- profile rendering
- graph layout changes directly

---

# 5. `ResetActions`

## Purpose
Group of global reset/recenter actions.

## Responsibilities
- full reset
- recenter graph
- clear focus
- clear selection

## Must not own
- filter panel UI
- view switching

---

# 6. `LeftControlPanel`

## Purpose
Primary control surface for exploration.

## Responsibilities
- view switching
- filter controls
- metric selection
- edge visibility controls
- legend toggle

## Child components
- `ViewSwitcher`
- `FilterPanel`
- `MetricSelector`
- `EdgeVisibilityControl`
- `LegendToggle`

## Must not own
- graph rendering
- profile panel content

---

# 7. `ViewSwitcher`

## Purpose
Switch between saved views.

## Responsibilities
- display available views
- indicate active view
- trigger view change action
- optionally expose view description

## Inputs
- list of views
- active view id

## Outputs
- set active view action

## Must not own
- implementation of view logic

---

# 8. `FilterPanel`

## Purpose
Display and manage active filters.

## Responsibilities
- render filter groups
- show active filter summary
- allow clear all
- update store with filter changes

## Recommended subgroups
- actor filters
- geography filters
- relevance / demand filters
- commercial overlay filters
- relationship filters

## Must not own
- filtering algorithms themselves

---

# 9. `MetricSelector`

## Purpose
Switch node sizing/emphasis metric.

## Responsibilities
- present allowed metrics
- update active metric mode

## Must not own
- compute metrics directly

---

# 10. `EdgeVisibilityControl`

## Purpose
Manage density of visible edges.

## Responsibilities
- switch between `all`, `strong_only`, `focused_only`
- reflect current edge visibility state

## Must not own
- edge filtering implementation logic

---

# 11. `LegendToggle`

## Purpose
Show/hide legend panel.

## Responsibilities
- toggle legend visibility

## Must not own
- legend content generation

---

# 12. `GraphWorkspace`

## Purpose
Wrapper around the canvas and canvas-adjacent overlays.

## Responsibilities
- host graph canvas
- host overlay counts/status
- host empty/error overlays when appropriate

## Child components
- `GraphCanvas`
- `GraphOverlayStatus`
- `EmptyOrErrorOverlay`

## Must not own
- profile details
- control logic

---

# 13. `GraphCanvas`

## Purpose
Render the interactive graph.

## Responsibilities
- mount sigma instance
- render nodes and edges
- listen for hover/click events
- handle camera behavior
- apply derived style state

## Inputs
- graph runtime model
- resolved visible nodes/edges
- style rules
- selection state
- focus state
- active layout mode

## Outputs
- node hover action
- node select action
- edge hover action
- edge select action
- camera events if needed

## Must not own
- schema validation
- search indexing
- profile rendering
- filter UI state

This is the single most important separation in the whole app.

---

# 14. `GraphOverlayStatus`

## Purpose
Small overlay with graph-state summary.

## Responsibilities
- show visible node count
- show visible edge count
- optionally show focus/filter summaries

## Must not own
- graph calculations beyond simple display inputs

---

# 15. `EmptyOrErrorOverlay`

## Purpose
Render non-happy-path states above or in place of the graph.

## Responsibilities
- no dataset state
- invalid dataset state
- zero results state

## Must not own
- dataset parsing
- retry logic beyond simple callbacks

---

# 16. `RightProfilePanel`

## Purpose
Display the currently selected node or edge profile.

## Responsibilities
- choose which profile state to render
- handle close action
- render empty state if nothing selected

## Child components
- `EmptyProfileState`
- `NodeProfileView`
- `EdgeProfileView`

## Must not own
- selection logic itself
- graph camera logic

---

# 17. `EmptyProfileState`

## Purpose
Guide the user when nothing is selected.

## Responsibilities
- show concise instructional placeholder

---

# 18. `NodeProfileView`

## Purpose
Render a full actor profile.

## Responsibilities
- header
- snapshot
- summary
- market interpretation
- relationship context
- evidence
- notes

## Suggested internal subcomponents
- `ProfileHeader`
- `ProfileSnapshot`
- `ProfileSummary`
- `ProfileMarketInterpretation`
- `ProfileRelationshipContext`
- `ProfileEvidence`
- `ProfileNotes`

## Must not own
- selection state
- graph traversal logic beyond using prepared data

---

# 19. `EdgeProfileView`

## Purpose
Render a compact relationship profile.

## Responsibilities
- source/target relation header
- relationship summary
- evidence
- notes

## Must not own
- graph rendering
- graph selection state

---

# 20. `LegendPanel`

## Purpose
Explain the active visual encoding.

## Responsibilities
- show node color meaning
- show size meaning if relevant
- show edge visibility cues if needed
- reflect active view styling

## Must not own
- encoding logic generation itself

---

# 21. `BottomUtilityBar` (optional)

## Purpose
Secondary utility surface.

## Responsibilities
- house legend or result summaries when appropriate
- stay visually quiet

## Important note
This should not become a dashboard strip packed with KPIs.

---

## Feature-level modules

Beyond visible components, the app should have feature modules that encapsulate behavior.

## `views/`
Responsibilities:
- active view selection
- view metadata lookup
- applying view defaults

## `filters/`
Responsibilities:
- filter state
- filter serialization
- filter application helpers

## `selection/`
Responsibilities:
- selected node/edge state
- selection transitions
- clear selection actions

## `profile/`
Responsibilities:
- transform canonical data into profile-ready display structures
- relationship grouping for display

## `search/`
Responsibilities:
- search indexing
- ranking
- query matching

## `legend/`
Responsibilities:
- derive active legend from active view + style state

---

## Store shape recommendation

Recommended global state domains:

- `dataset`
- `graphRuntime`
- `activeViewId`
- `activeFilters`
- `selectedNodeId`
- `selectedEdgeId`
- `focusState`
- `metricMode`
- `edgeVisibilityMode`
- `legendOpen`
- `profilePanelOpen`
- `uiStatus`

### Important rule
Keep canonical data and mutable UI state clearly separate.

---

## Data ownership map

| Concern | Owner |
|---|---|
| Canonical dataset | parser / data layer |
| Graph runtime object | graph layer |
| Visible subset | derived view/filter layer |
| Selection state | global store |
| Search index | search feature |
| Profile transformation | profile feature |
| Legend content | legend feature |
| Actual graph rendering | `GraphCanvas` |
| Profile UI | `RightProfilePanel` subtree |

This table is important because without it, too much logic tends to get shoved into the graph canvas component.

---

## Prop discipline rules

### Good prop candidates
- current view label
- current counts
- current legend content
- current selected profile data
- current control values

### Bad prop candidates
- entire raw dataset passed deep through the tree
- raw graphology graph passed everywhere
- layout logic passed into unrelated UI surfaces

Use selectors/hooks instead of wide prop drilling.

---

## Component anti-patterns to avoid

### 1. Monolithic `GraphPage` component
A single giant component that mixes rendering, filters, layout, profile logic, and search.

### 2. Profile panel directly querying raw graph in many places
This creates brittle coupling and re-render problems.

### 3. Filter components implementing filtering rules themselves
UI should dispatch intent; filtering logic belongs in feature/graph layers.

### 4. Graph canvas owning app state
The renderer should consume derived state, not become the product brain.

### 5. View switcher hardcoding view-specific behavior
Views should come from data and feature logic.

---

## Acceptance criteria for component architecture

The component architecture is good enough when:

1. the graph renderer can be replaced without rewriting the whole app
2. the profile panel can evolve without contaminating canvas logic
3. filters and views are data-driven rather than hardcoded into components
4. state changes are easy to reason about
5. new views can be added without major refactors

---

## Bottom line

A Kumu-inspired product can still fail badly if the component architecture is sloppy.

This spec exists so Codex builds:
- a graph renderer,
- a control system,
- and a profile-driven atlas

as coordinated parts of one product — rather than as one tangled React page.
