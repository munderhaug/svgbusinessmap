# 08 — Layout strategy

## Purpose

This document defines how the graph should be laid out in version 1.

This matters because layout is not a cosmetic detail.
Layout determines:
- whether clusters are readable
- whether labels collide
- whether central actors are visible
- whether the map feels navigable or chaotic

If layout is left to the graph library with no strategy, the atlas will become unreadable long before the data model is finished.

---

## Design principle

The product should adopt a **relationship-first layout strategy** with optional alternate lenses.

That means:
- the default layout emphasizes structure and connectivity
- geography is a secondary layout mode
- stored layouts are allowed
- layout is not editable in-app in version 1

This is consistent with the most relevant Kumu patterns to borrow:
- stakeholder/network-style maps use force-directed layouts
- large-network templates optimize for readability and performance
- geo is a distinct template/lens rather than the default worldview. ([Kumu Templates](https://docs.kumu.io/guides/templates), [Kumu Force-directed Layout](https://docs.kumu.io/guides/layouts/force-directed), [Kumu Geo Template](https://docs.kumu.io/guides/templates/geo))

---

## Core layout decisions

### 1. Default = force-directed network layout

The default layout should be a force-directed network layout.

Why:
- it naturally reveals clusters and hubs
- it supports exploratory network reading
- it matches the actor/relationship ontology
- it is closer to the visual behavior people expect from Kumu stakeholder/network maps

The exact engine does not need to mimic Kumu, but the layout philosophy should be similar: connected actors attract, dense neighborhoods form clusters, and pinned positions can override the layout later where useful. ([Kumu Force-directed Layout](https://docs.kumu.io/guides/layouts/force-directed))

### 2. Geography is a separate lens, not the default layout

A geographic layout should exist as a separate view or layout preset.

Why:
- geography matters in this corridor
- but geography is not the primary truth of the market
- relationship structure is usually more informative than physical placement for the core atlas experience

Kumu treats geo as a dedicated template rather than a universal default, which is the right principle to borrow here. ([Kumu Geo Template](https://docs.kumu.io/guides/templates/geo))

### 3. Layout presets should be data-driven

Layout decisions should live in `layout_presets` data where appropriate.

This allows:
- a stable overview layout
- a geographic layout
- future curated/pinned layouts

without tying layout logic entirely to frontend code.

### 4. No in-app manual layout editing in v1

The app is a viewer.

It may support reading stored coordinates and respecting pinned positions, but it should not include drag-to-reposition editing tools in version 1.

---

## Layout modes for version 1

Version 1 should support three layout modes.

## A. `force`

### Purpose
Default exploratory network layout.

### Best for
- overview
- private buyers
- public and hybrid
- growth and product-led
- ecosystem hubs
- opportunity overlay

### Behavior
- connected nodes pull together
- nodes repel to reduce overlap
- dense relationship neighborhoods form visible clusters
- weakly connected actors sit more peripherally

### When to use
Use as the default for most saved views.

---

## B. `preset`

### Purpose
Use stored coordinates for curated readability.

### Best for
- refined overview layout
- presentation-quality stable arrangement
- future hand-tuned market storytelling view

### Behavior
- positions are read from `layout_presets[].positions`
- camera framing may use stored viewport
- pinned nodes remain stable

### When to use
Use when you want deterministic visual structure rather than re-running the layout.

---

## C. `geo`

### Purpose
Place actors by geographic coordinates or mapped geography fields.

### Best for
- geographic corridor view
- municipality concentration analysis
- place-based exploration

### Behavior
- nodes use latitude/longitude if available
- edges become secondary visual context
- overlapping actors may require spread/jitter rules

### When to use
Use only when the question is geographically oriented.

---

## Layout-by-view recommendations

| View | Recommended layout | Why |
|---|---|---|
| `overview` | `force` initially, later optional `preset` | Best for structural reading; later can be stabilized |
| `private_buyers` | `force` | Private clusters should emerge organically |
| `public_and_hybrid` | `force` | Relationship and procurement structure matter more than place |
| `growth_and_product_led` | `force` | Good for spotting connected growth clusters |
| `design_relevant` | `force` | Shows where design-relevant actors cluster structurally |
| `ai_relevant` | `force` | Same principle as above |
| `software_and_product` | `force` | Reveals product/software neighborhoods |
| `geographic_corridor` | `geo` | Place is the main logic here |
| `ecosystem_hubs` | `force` | Connectivity is the point of the view |
| `opportunity_overlay` | `force` or `preset` | Structural context with commercial overlay |

---

## Force-layout strategy

Because the app is intended to support roughly 200–1,500 nodes, the force layout should be tuned for readability rather than perfect simulation elegance.

## Core force objectives
- reduce overlap
- keep connected neighborhoods visually coherent
- avoid excessive edge crossing where possible
- keep hubs legible
- preserve enough whitespace for hover and selection

## Practical rules

### 1. Use a staged layout workflow
Recommended order:
1. create graph
2. apply filters for current view
3. run or apply layout for visible graph
4. render

This matters because Kumu’s metrics and view behavior explicitly operate on the currently visible map state rather than on hidden items, and the same principle is sensible here: layout should reflect what the user is actually seeing. ([Kumu Metrics](https://docs.kumu.io/guides/metrics))

### 2. Respect pinned positions if present
If a preset provides pinned positions:
- keep those nodes stable
- allow the rest of the graph to settle around them

### 3. Avoid full re-layout on every small interaction
Do not re-run layout when:
- hovering a node
- opening a profile
- changing a metric-only size mode

Only re-run or reapply layout when:
- switching views
- applying major filters
- loading a new dataset
- explicitly resetting layout

### 4. Keep transitions smooth
When layout changes:
- animate lightly if performance allows
- avoid violent jumps
- preserve orientation where possible

---

## Preset-layout strategy

Preset layouts should be supported even before manual in-app editing exists.

Why:
- some views will eventually need curated clarity
- stable screenshots and demos matter
- a fixed “market overview” layout can be easier to learn

## Preset rules
- stored positions must map to valid node IDs
- nodes without positions may either fall back to force layout or remain hidden until placed, depending on implementation choice
- the app should not silently ignore broken preset references

## Recommended behavior
If a preset layout is incomplete:
- apply stored positions to known nodes
- use force layout for the remainder
- flag incomplete preset state in development mode if helpful

---

## Geo-layout strategy

Geo is helpful, but it introduces its own problems.

### Known risks
- actors in the same municipality may overlap heavily
- exact same coordinates can stack perfectly
- edges can become messy and visually long
- geography can imply importance where none exists

Kumu’s geo template documentation notes similar practical issues: stacked elements, location-field quality, and a constrained visual style. That is another reason geo should be treated as a specific lens rather than as the main canvas mode. ([Kumu Geo Template](https://docs.kumu.io/guides/templates/geo))

## Geo rules for version 1
- use geo only in the dedicated geographic view
- allow node overlap reduction or subtle jitter
- reduce edge prominence aggressively
- keep labels sparse by default
- allow municipality and zone filters to do most of the work

---

## Label strategy by layout mode

Labels are a layout problem, not just a typography problem.

## Default label rules

### Force layout
- default label mode: `focused`
- show labels for selected node and local neighborhood
- optionally show labels for top-ranked nodes when zoomed out

### Preset layout
- label mode may be slightly more permissive if spacing has been curated

### Geo layout
- show labels only for high-priority or selected nodes by default
- avoid municipality-wide label clutter

---

## Edge density rules by layout mode

### Force layout
- default to `strong_only` or `focused_only` in denser views

### Preset layout
- same as force, unless the preset was created for a sparse presentation subset

### Geo layout
- default to `focused_only` or highly de-emphasized edges

The edge rule is simple:

> If edges stop helping spatial interpretation, they must visually step back.

---

## Large-graph readability rules

For larger datasets, the app must not attempt to show everything equally.

## Required strategies
- thresholded labels
- weak-edge suppression
- local hover highlighting
- result counts after filtering
- focus tools
- optional clustering/grouping later

Kumu’s template guidance is revealing here: stakeholder maps use force-directed layout, while its big-data/SNA-oriented template trades some decoration richness for scale and performance. That is exactly the kind of product honesty this app should adopt. ([Kumu Templates](https://docs.kumu.io/guides/templates), [Kumu First Steps](https://docs.kumu.io/getting-started/first-steps))

---

## Layout anti-patterns to avoid

### 1. “Everything auto forever”
Never storing or curating layout when the map would clearly benefit from a stable overview.

### 2. “Everything manual”
Trying to hand-place a large evolving market graph from the start.

### 3. Geography as default truth
Making the main map look like a geographic plot when the core ontology is relational.

### 4. Instant re-layout on every interaction
Creates disorientation and makes focused analysis impossible.

### 5. Full-label mode on dense graphs
This kills readability immediately.

### 6. Decorative centering of nodes unrelated to structure
Layout should reflect meaning, not just aesthetics.

---

## Recommended implementation logic for Codex

### On initial load
- validate data
- load default view
- load assigned layout preset if available
- otherwise run force layout

### On view switch
- apply new view filters
- determine whether the target view expects `force`, `preset`, or `geo`
- apply that layout mode
- animate lightly if feasible

### On metric change
- do not re-layout
- only update styling/sizing

### On node selection
- do not re-layout
- only center/focus/highlight

### On major filter change
- optionally recompute layout for visible subset if needed
- do not do this on every tiny control change unless performance remains acceptable

---

## Acceptance criteria for layout

The layout strategy is good enough when:

1. the overview view immediately reveals meaningful structure
2. dense parts of the graph remain explorable
3. switching views does not feel random or chaotic
4. geography is available without taking over the product
5. selected actors remain easy to inspect without destroying orientation
6. the app remains readable as the dataset grows

---

## Bottom line

The layout strategy should make the atlas feel intentional.

The user should feel:
- that clusters mean something,
- that geography is available when useful,
- and that the map is structured rather than accidental.

That is the bar.
