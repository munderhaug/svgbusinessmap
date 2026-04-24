# 13 — Acceptance criteria

## Purpose

This document defines what “done” means for version 1 of the market atlas.

Without this file, the build risks drifting into one of two bad states:
- technically impressive but strategically wrong
- visually polished but operationally incomplete

This document gives Codex, and later you, a concrete bar for judging whether the app is ready.

---

## Acceptance philosophy

Version 1 succeeds if it becomes a **clear, usable, read-only market atlas**.

Version 1 does **not** need to be:
- a Kumu clone
- a graph editor
- a CRM
- a collaboration platform
- a procurement tool
- a polished enterprise platform

The bar is not “feature completeness.”
The bar is “usable truth.”

---

## Release-level acceptance criteria

Version 1 is acceptable when all of the following are true:

1. the dataset validates cleanly
2. the graph renders reliably
3. the user can navigate the graph through views, filters, search, and focus
4. the user can inspect rich actor profiles
5. the graph remains readable under realistic density
6. the product preserves the actor-centric ontology
7. the product does not drift into editor or CRM behavior

---

# A. Data integrity criteria

## A1. Schema validation

The app must:
- validate the JSON dataset against the canonical contract before render
- fail clearly on invalid data
- not silently patch missing required fields

### Accept when
- valid dataset loads successfully
- invalid dataset produces human-readable error state

---

## A2. Referential integrity

The app must detect and reject or clearly surface:
- edges pointing to missing nodes
- nodes or edges referencing missing source refs
- views referencing missing layout presets
- default view referencing missing view ID

### Accept when
- these errors are surfaced clearly and do not fail silently

---

## A3. Stable IDs

The system must rely on stable IDs for:
- nodes
- edges
- views
- layouts
- source refs

### Accept when
- IDs are used consistently throughout runtime and state
- selection, focus, and linking depend on IDs rather than labels

---

# B. Graph rendering criteria

## B1. Initial render

The app must:
- load the default view
- render the graph canvas
- show visible nodes and edges according to the active view
- preserve interactive responsiveness

### Accept when
- app opens into a coherent view without visual or runtime failure

---

## B2. Graph readability

The graph must remain readable under realistic view density.

### Accept when
- labels are not all-on by default in dense views
- weak edges are suppressible
- hover and focus make local structure legible
- selected nodes are visually unmistakable

---

## B3. Graph responsiveness

The graph must feel responsive during:
- pan/zoom
- node hover
- node click
- view switch
- filter changes

### Accept when
- routine interactions do not feel visibly sluggish on realistic datasets

---

# C. View system criteria

## C1. Saved views exist and work

The app must support the saved view model defined in the view catalog.

### Minimum required views
- `overview`
- `private_buyers`
- `public_and_hybrid`
- `growth_and_product_led`
- `design_relevant`
- `ai_relevant`
- `software_and_product`
- `geographic_corridor`
- `ecosystem_hubs`
- `opportunity_overlay`

### Accept when
- all required views can be selected
- each view applies its own filters/style/layout behavior
- view switching is reliable and predictable

---

## C2. View meaning is preserved

A view must feel like a different lens on the same system, not like an unrelated graph mode.

### Accept when
- the user can understand what a view is for
- view switching preserves the underlying identity of the data
- views do not break the ontology

---

# D. Search and filter criteria

## D1. Search

The app must support search across the specified fields.

### Accept when
- actor lookup works from the top bar
- selecting a search result centers and opens the correct actor
- empty search results are handled clearly

---

## D2. Filters

The app must support the core filters defined in the interaction and field specs.

### Minimum filter groups
- buyer type
- sector
- municipality / zone
- procurement regime
- likely needs
- likely buying modes
- opportunity band
- confidence level

### Accept when
- filters combine with AND logic
- active filter count is visible
- result counts update
- clear-all works

---

# E. Focus and navigation criteria

## E1. Node focus

The app must allow neighborhood focus around a selected actor.

### Accept when
- node selection highlights the selected actor and local neighborhood
- first-degree focus works
- second-degree focus works if implemented
- clear focus returns the graph to the current view state

---

## E2. Navigation reversibility

The app must always let the user recover orientation.

### Accept when
- reset view works
- recenter works
- close profile works
- clear selection works
- graph does not get “stuck” in hidden state

---

# F. Profile panel criteria

## F1. Node profile completeness

A selected actor must show a useful profile.

### Required sections
- header
- snapshot
- narrative summary
- market interpretation
- relationship context
- evidence
- notes

### Accept when
- each section renders when data exists
- empty or missing optional fields degrade gracefully
- profile supports navigation to related actors where specified

---

## F2. Edge profile support

The app must support a compact relationship profile.

### Accept when
- edge click opens a meaningful edge summary
- source, target, type, strength, and evidence are visible

---

## F3. Empty profile state

### Accept when
- the profile panel communicates what to do when nothing is selected
- it does not look broken or unfinished

---

# G. Legend and encoding criteria

## G1. Legend accuracy

The legend must reflect the active view’s encoding.

### Accept when
- node color meaning is accurate
- node size meaning is accurate when size encodes data
- legend updates on relevant view change

---

## G2. Visual hierarchy

### Accept when
- selected node is unmistakable
- hover state is visible but not overpowering
- non-relevant context can fade into the background

---

# H. Layout criteria

## H1. Default structural layout

### Accept when
- overview uses a readable network-first layout
- clusters and hubs are visually interpretable

## H2. Geographic view

### Accept when
- a geographic lens exists
- geography does not become the default worldview
- overlapping geographic actors are handled reasonably

## H3. Layout stability

### Accept when
- view switching does not feel random or destructive
- layout does not violently re-run on routine interactions

---

# I. Performance criteria

## I1. Realistic scale

The app should remain comfortably usable on realistic subsets and a meaningful full dataset.

### Working target for version 1
- 50-node seed graph: excellent
- 200–500 node graph: comfortable
- 1,000+ node graph: still usable through viewing/filtering discipline

### Accept when
- interaction remains practical on representative datasets
- dense views do not collapse into unreadable label or edge clutter by default

---

# J. Architecture and code quality criteria

## J1. Separation of concerns

### Accept when
- graph rendering logic is separated from controls and profile logic
- data validation is separated from rendering
- view logic is data-driven, not hard-coded into components

---

## J2. Source-of-truth discipline

### Accept when
- canonical JSON remains the source of truth
- UI state does not mutate raw dataset structure

---

## J3. Non-goal enforcement

### Accept when
- no editing UI exists
- no CRM/pipeline logic exists
- no auth/backend complexity is introduced
- no hidden platform ambitions appear in the codebase

---

# K. User-success criteria

The app is good enough if a user can do this without instruction:

1. open the app
2. understand what they are looking at
3. switch to a relevant view
4. search for an actor
5. inspect the actor’s profile
6. understand local relationships
7. filter to a subset
8. reset and continue exploring

If that sequence feels confusing, version 1 is not done.

---

# L. Explicit non-acceptance conditions

Version 1 should be considered **not ready** if any of these are true:
- the graph renders but the ontology is visually incoherent
- the profile panel is too thin to interpret actors properly
- dense graphs require all labels to understand anything
- filters feel ambiguous or brittle
- views feel arbitrary instead of meaningful
- the product behaves like a half-built editor or CRM

---

## Bottom line

Done means:
- trustworthy data,
- readable structure,
- useful profiles,
- disciplined views,
- and smooth exploration.

Not perfection.
But definitely not ambiguity.
