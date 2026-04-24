# 06 — Interaction spec

## Purpose

This document defines how the user interacts with the market atlas.

The graph itself is not enough.
A Kumu-inspired product works because exploration is shaped by a combination of:
- search
- filters
- focus behavior
- views
- legends
- profile inspection
- controlled state changes

Kumu’s controls model is a useful conceptual reference here because it treats things like filters, focus, clustering, legends, and view toggles as explicit interaction structures rather than hidden UI side effects. ([Kumu Controls Guide](https://docs.kumu.io/guides/controls))

This document tells Codex what the app should do when a user:
- opens the app
- switches a view
- searches
- clicks a node
- hovers a relationship
- applies filters
- focuses a neighborhood
- resets the map

---

## Product interaction philosophy

### 1. Read-only but highly navigable

The app is not editable in version 1.
But it should still feel active, exploratory, and responsive.

### 2. Reveal, do not overwhelm

The interface should progressively reveal context.
It should never try to show all labels, all profiles, and all relationships at once.

### 3. Focus is a first-class interaction

The user should be able to move from:
- broad market overview
- to one actor
- to one actor’s neighborhood
- to one filtered subset

without losing orientation.

### 4. Every major state should be reversible

The user must always be able to:
- clear filters
- exit focus
- reset camera
- close profile
- return to default view state

---

## Primary UI regions

Version 1 should be organized into five main UI regions.

### 1. Top bar
Contains:
- app title
- current view label
- search input
- reset button
- optional view description toggle

### 2. Left control rail or drawer
Contains:
- view switcher
- filters
- metric selector
- edge visibility control
- optional legend toggle

### 3. Center graph canvas
Contains:
- the network graph
- zoom/pan behavior
- hover and selection response
- optional inline label rendering

### 4. Right profile panel
Contains:
- node profile or edge profile
- evidence list
- relationships summary
- market interpretation

### 5. Bottom utility region (optional)
Contains:
- legend
- graph state summary
- result count
- status hints

---

## Startup behavior

When the app loads:

1. validate the JSON dataset against schema
2. load `meta.default_view_id` if present
3. if no default view is present, load `overview`
4. apply the selected view’s default filters and style
5. set camera using the view’s assigned layout preset if available
6. show graph without selecting any node
7. keep profile panel closed by default

If the dataset is invalid:
- show a clear validation error state
- do not silently patch or guess missing structure

---

## Search behavior

## Purpose
Search is the fastest entry point into a dense graph.

## Search scope
Search should operate across:
- `label`
- `aliases`
- `sector`
- `subsector`
- `location.municipality`
- `profile.summary`
- `profile.why_it_matters`
- `profile.relevant_to_olavstoppen`

## Matching behavior
- case-insensitive
- partial match
- prioritize exact `label` matches first
- then alias matches
- then profile text matches

## Search result behavior
When the user selects a result:
- center the graph on that node
- highlight the node
- open the profile panel
- apply temporary focus to first-degree neighbors

## Empty search state
If no results are found:
- keep current graph state intact
- show “No matches found” in the search dropdown
- do not reset filters automatically

---

## Node hover behavior

On node hover:
- visually emphasize hovered node
- visually emphasize first-degree connected edges
- visually emphasize first-degree neighboring nodes
- de-emphasize non-neighborhood elements lightly, not completely
- show a small tooltip with label and key classification fields

The tooltip should include:
- label
- buyer type
- sector
- municipality

Do not open the full profile panel on hover.

---

## Node click behavior

On node click:
- set node as selected
- open right-side profile panel
- keep neighborhood highlighting active
- center node if it is off-screen or partially hidden
- preserve current filters and current view

If a node is already selected and the user clicks it again:
- keep it selected
- do not toggle it off unless the UI explicitly supports deselect-on-second-click

Recommended version 1 behavior:
- second click does nothing special
- deselection happens via explicit close/reset action

---

## Edge hover behavior

On edge hover:
- highlight the edge
- emphasize its source and target nodes
- show small tooltip with:
  - edge type
  - optional label
  - strength

Do not open the full right-side profile panel on hover.

---

## Edge click behavior

Version 1 should support edge click, but edge inspection should remain lighter than node inspection.

On edge click:
- open a compact edge profile in the right panel
- show:
  - source label
  - target label
  - edge type
  - strength
  - confidence
  - notes
  - source refs

This is useful but secondary.
The app is node-led.

---

## Profile panel behavior

## Open states
The right-side panel can display:
- node profile
- edge profile
- no selection

## Open actions
The panel opens when:
- a node is clicked
- an edge is clicked
- a search result is selected

## Close actions
The panel closes when:
- the close button is clicked
- the user hits `Escape`
- the global reset command is used

Closing the panel should:
- remove active node/edge selection
- preserve filters and view state
- preserve camera unless reset was explicitly requested

---

## Focus behavior

Focus is one of the most important interactions in the app.

## Focus actions
The app should support:
- first-degree focus
- second-degree focus
- clear focus

## Focus definition
A focus action means:
- keep the selected node visually primary
- highlight neighbors within N hops
- de-emphasize unrelated nodes and edges
- optionally hide unrelated edges below a threshold

## Default behavior
- node click applies first-degree focus by default
- certain views may set `focus_hops = 2`

## Focus controls
The UI should include:
- “1 hop” focus option
- “2 hops” focus option
- “Clear focus” action

## Important rule
Focus should not mutate filters.
It is a temporary navigation state, not a filter change.

---

## Filter behavior

## Filtering model
All filters use **AND logic** by default.

That means:
- selecting `buyer_type = public`
- and `municipality = Stavanger`
- and `attributes.ai_relevance = high`

returns only nodes satisfying all three conditions.

This is the correct default because it is predictable and analytically sharp.

## Filter UI expectations
Filters should be grouped by category:
- actor filters
- geography filters
- relevance / demand filters
- commercial overlays
- relationship display filters

## Filter application behavior
When a filter is applied:
- visible nodes update immediately
- visible edges update accordingly
- current selection remains if still valid
- if current selection becomes hidden, close the profile panel and clear selection

## Filter persistence
Filters should persist while the user remains in the current view.
Switching to another view should apply that view’s defaults, unless the app later supports “preserve filters across views” as an advanced option.

Recommended version 1 behavior:
- switching views resets to the selected view’s saved defaults

## Filter summary
The UI should always show:
- number of active filters
- result count after filtering
- quick “clear all” action

---

## View switching behavior

A view switch should feel like changing lens, not loading a different app.

On view switch:
- apply the new view’s saved filters
- apply the new view’s style rules
- apply the associated layout preset if defined
- clear active focus
- clear active selection unless explicitly preserved later
- close the profile panel
- update the URL state if enabled

The user should always know:
- which view is active
- what that view is for

Recommended behavior:
- show the view name prominently
- optionally provide a short description or info icon

---

## Legend behavior

The legend should explain the currently active encoding.

If a view changes:
- node color encoding changes -> legend updates
- node size encoding changes -> legend updates if relevant
- edge visibility policy changes -> legend or helper text updates if needed

The legend should be:
- concise
- collapsible
- context-aware

Do not show a static legend disconnected from the active view.

---

## Metric selector behavior

Version 1 should allow a small number of metric-driven size or emphasis changes.

Examples:
- neutral size
- commercial attractiveness
- short-term opportunity
- access difficulty
- degree (if computed)

Changing the metric should:
- update node sizing
- preserve filters
- preserve selection where possible
- not change the underlying view definition unless the user saves it later in a future version

---

## Edge visibility control

Dense graphs become unreadable if every edge is always equally visible.

The UI should support:
- all edges
- strong edges only
- focused edges only

### Definitions
- **all**: show all visible edges
- **strong only**: hide edges below configured threshold
- **focused only**: only show edges touching the selected/focused neighborhood

Recommended default per view:
- overview -> `strong_only`
- dense analytical views -> `focused_only`
- sparse views -> `all`

---

## Camera behavior

The camera should support:
- zoom with mouse wheel / trackpad pinch
- pan by drag
- fit-to-visible on reset
- smooth transition when centering on a selected node

### Reset camera action
Reset camera should:
- preserve filters
- preserve current view
- clear temporary focus if user selected “full reset”
- otherwise simply reframe visible graph elements

Recommended version 1 actions:
- `Reset view` = resets camera + clears focus + clears selection
- `Recenter` = only fits the current visible graph

---

## Keyboard behavior

Version 1 keyboard support should be minimal but deliberate.

### Required shortcuts
- `Escape` -> close profile panel or clear focus
- `/` -> move cursor to search
- `0` -> reset camera / fit graph

Optional later:
- arrow navigation through search results
- keyboard cycling through views

---

## URL state behavior

Version 1 should preserve a shareable exploration state as much as reasonably possible without backend complexity.

Recommended URL state:
- current view id
- selected node id (optional)
- active filters (serialized)
- metric mode (optional)

This allows:
- bookmarking
- returning to a lens
- sharing a specific actor-centered state later

If this proves too heavy for initial implementation, preserve at least:
- current view id
- selected node id

---

## Result count behavior

The UI should expose a live sense of scope.

At minimum it should show:
- visible node count
- visible edge count

This matters because filtering a graph without feedback is cognitively disorienting.

---

## Empty states

The app needs at least four explicit empty states.

### 1. No dataset loaded
Show:
- “No dataset loaded”
- brief instruction or fallback

### 2. Dataset invalid
Show:
- validation failure summary
- link to inspect errors
- do not attempt partial rendering silently

### 3. No results after filters
Show:
- “No actors match the current filters”
- button to clear filters
- preserve current view context

### 4. Nothing selected
The right panel should show:
- concise instruction such as “Select an actor to inspect its profile”

---

## Error states

The app should fail clearly for:
- invalid schema
- missing referenced node IDs in edges
- missing referenced source IDs
- invalid layout references
- invalid default view reference

Recommended behavior:
- surface a human-readable error summary
- do not silently ignore referential integrity problems

---

## Performance behavior

The interaction model should anticipate large graphs.

### Required performance-friendly defaults
- label density control
- edge de-emphasis by default
- hover effects limited to local neighborhood
- no expensive full-panel recomputations on every mouse move
- search results virtualized if large

### Important principle
The app should never assume that 1,500 visible nodes can all be fully labeled simultaneously.

---

## Interaction anti-patterns to avoid

Do not implement these in version 1:
- auto-opening multiple panels
- modal-heavy interactions for simple actions
- hidden filter state with no summary
- hover-triggered persistent panels
- drag-editing nodes and edges
- forced camera jumps on every small interaction
- OR/AND filter ambiguity
- search that secretly changes filters

---

## Acceptance conditions for interaction design

The interaction system is good enough when a user can do this without explanation:

1. open the atlas
2. switch to a relevant view
3. search for an actor
4. inspect its profile
5. understand its immediate neighborhood
6. filter to a meaningful subset
7. reset and continue exploring

If any of those steps feels ambiguous, the interaction model is not done.

---

## Bottom line

The graph canvas is not the product by itself.
The product is the combination of:
- graph,
- controls,
- focus,
- profiles,
- and saved views.

This interaction spec defines how those pieces should work together so the app feels like a deliberate market atlas rather than a raw graph demo.
