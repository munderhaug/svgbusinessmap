# 03 — Schema explainer

## Purpose

This document explains the canonical JSON schema for the market atlas.

The schema file is the machine-readable contract.
This document is the human-readable explanation of why it is shaped the way it is.

Together, they define the source of truth that the viewer app must load, validate, and render.

---

## Design objective

The schema is designed to balance five competing needs:

1. **Strict enough** to prevent ontology drift
2. **Flexible enough** to support evolving market data
3. **Simple enough** for manual seed-data creation
4. **Expressive enough** for a Kumu-inspired viewer
5. **Stable enough** to survive UI changes later

This is why the schema is intentionally conservative.

It does not try to model every possible future feature.
It models exactly what version 1 needs to render a useful, actor-centric market atlas.

---

## Top-level structure

The root object has six required top-level keys:

- `meta`
- `nodes`
- `edges`
- `views`
- `layout_presets`
- `source_refs`

### Why these six?

Because they separate the concerns that should stay separate.

- `meta` describes the dataset as a whole
- `nodes` define actors
- `edges` define relationships
- `views` define how the same graph is interpreted visually
- `layout_presets` define optional saved placements
- `source_refs` centralize evidence references so sources are reusable instead of duplicated inside every node and edge

This is a deliberate break from messy JSON files where each node carries full duplicated citation blobs and ad hoc view logic.

---

## `meta`

The `meta` object exists so the dataset can identify itself and tell the app how to open.

### Required fields
- `project_id`
- `title`
- `version`
- `updated_at`

### Optional but important fields
- `description`
- `default_view_id`
- `locale`
- `corridor_description`

### Why `default_view_id` matters

Because the graph should not necessarily open in a raw or maximal state.
The product should always have a deliberate default view.

---

## `nodes`

Nodes are the primary objects in the system.

Each node represents an actor with an independent identity in the market map.

### Required node fields
- `id`
- `label`
- `type`
- `buyer_type`
- `sector`
- `location`
- `attributes`
- `profile`
- `scores`
- `tags`
- `source_refs`

### Why `subsector` is optional

Because sector models tend to evolve while the product is being learned.
A strict subsector taxonomy this early would create false precision.

### Why `aliases` exists

Because real-world actors often have:
- legal names
- brand names
- abbreviations
- common short forms

Search quality will be materially better if aliases are supported from the start.

---

## Node identity model

### `id`

Every node ID must be stable and machine-safe.

The schema requires IDs to match a constrained pattern so they can safely be used for:
- rendering keys
- routing state
- saved views
- future exports

### Good examples
- `org_lyse`
- `public_stavanger_kommune`
- `cluster_nordic_edge`
- `unit_altibox_consumer`

### Bad examples
- `Lyse AS`
- `Stavanger kommune!!!`
- `123`
- `node with spaces`

The key rule is: IDs are not labels.

---

## `type` versus `buyer_type`

This distinction is intentional and important.

### `type`
Describes what kind of node this is in the graph.

Examples:
- `organization`
- `public_entity`
- `hybrid_entity`
- `cluster`

### `buyer_type`
Describes the commercial logic the actor follows.

Examples:
- `private`
- `public`
- `hybrid`
- `growth`
- `channel`

### Why both are needed

Because graph identity and market logic are not the same thing.

A node can be:
- `type = organization`
- `buyer_type = growth`

Or:
- `type = ecosystem_actor`
- `buyer_type = channel`

If you collapse these into one field, the model becomes less truthful.

---

## `location`

Location is modeled as structured metadata, not as a node.

### Required fields
- `municipality`
- `zone`

### Optional fields
- `subarea`
- `latitude`
- `longitude`

### Why this is the right level for v1

The product is relationship-first, not map-pin-first.

You still need enough location structure to:
- filter by municipality
- differentiate corridor zones
- support a future geo view
- distinguish Stavanger subareas like Rennesøy when needed

But geography does not need to become a graph object yet.

---

## `attributes`

This object stores structured, filterable metadata that shapes interpretation.

### Required attribute fields
- `size_band`
- `ownership_type`
- `procurement_regime`
- `design_relevance`
- `ai_relevance`
- `software_relevance`
- `product_relevance`
- `likely_needs`
- `likely_buying_modes`
- `opportunity_band`
- `relationship_status`

### Why these are grouped under one object

Because they are all:
- structured
- filterable
- visually useful
- secondary to identity but essential to interpretation

Grouping them avoids flattening the node into dozens of top-level fields.

### Why some fields are enumerated

Fields like `size_band`, `procurement_regime`, `relevance`, `likely_buying_modes`, and `opportunity_band` are constrained by enums because consistency matters more than freedom here.

If left unconstrained, you quickly get garbage states like:
- `AI = High`
- `AI relevance = high-ish`
- `artificial intelligence relevance = High`

The schema prevents that drift.

### Why `sector` is not enum-constrained

Because sector taxonomy is still likely to evolve during early use.

Constraining `sector` too early would make the schema brittle and force constant schema revisions. So the contract keeps `sector` flexible, while more operational attributes are constrained.

---

## `profile`

The profile is the narrative layer of the node.

### Required fields
- `summary`
- `why_it_matters`

### Optional fields
- `relevant_to_olavstoppen`
- `likely_sponsor_roles`
- `notes`

### Why narrative matters here

The graph alone cannot resolve ambiguity.
Two actors can look structurally similar but matter for different reasons.

The profile is where the app becomes a market atlas rather than a bare network chart.

---

## `scores`

Scores are optional in interpretation but structurally supported from version 1.

### Supported score fields
- `commercial_attractiveness`
- `short_term_opportunity`
- `access_difficulty`
- `confidence_level`
- `score_date`

### Why only `confidence_level` is required

Because the app should be able to load nodes even when full scoring has not been completed yet.

This is a useful design choice:
- seed data can start incomplete
- confidence can still govern styling and caution
- scoring can be layered in later without breaking the schema

### Why scores are node-level, not edge-level

Because the current product is actor-centric.
If later you need relationship scoring, that can be added separately.

---

## `tags`

Tags exist for lightweight labeling and future view support.

Use them for:
- curated flags
- hand-picked subsets
- transitional labels
- testing markers

Do not use tags as a replacement for structured fields.

A good test:
- if something will be filtered and styled regularly, make it a structured field
- if something is ad hoc or editorial, a tag is fine

---

## `source_refs`

Nodes and edges reference top-level source IDs rather than carrying full source objects inline.

### Why this is better

It avoids duplication.

Without centralized source refs, one official source might get repeated across:
- 15 nodes
- 8 edges
- 3 views

That becomes brittle.

Centralizing sources gives you:
- cleaner data
- easier updates
- consistent evidence display
- simpler provenance handling

---

## `edges`

Edges represent typed relationships between nodes.

### Required fields
- `id`
- `source`
- `target`
- `type`
- `directed`
- `strength`
- `tags`
- `source_refs`

### Optional fields
- `label`
- `attributes`

### Why `strength` is required

Because even in version 1, the app needs a consistent way to:
- hide weak edges
- de-emphasize clutter
- prioritize more meaningful relationships visually

This is not meant as mathematical precision.
It is a display and interpretation aid.

### Why `label` is optional

Because many edges will already be sufficiently explained by:
- source node
- target node
- typed relationship

Requiring labels on every edge would encourage noisy redundancy.

---

## Edge attributes

The schema keeps edge attributes intentionally light.

Currently supported:
- `notes`
- `confidence_level`

This is enough for version 1.
Anything richer would likely be premature.

---

## `views`

Views are one of the most important parts of the data model.

A view is not a separate dataset.
It is a rule-based lens over the same graph.

### Required view fields
- `id`
- `label`
- `node_filters`
- `edge_filters`
- `style`

### Optional fields
- `description`
- `defaults`
- `layout_preset_id`

### Why this matters

Without data-defined views, the app becomes:
- a graph renderer with ad hoc UI state
- difficult to share or preserve
- too dependent on frontend code for interpretation

With views in data, the product becomes much more portable and predictable.

---

## Filter rules

The schema uses a generic `filterRule` object with:
- `field`
- `operator`
- optional `value`

### Why not hard-code every possible filter?

Because the app needs a reusable filter language.
A view should be able to say things like:
- `buyer_type in [public, hybrid]`
- `attributes.ai_relevance eq high`
- `scores.commercial_attractiveness gte 70`

A generic filter object is more stable than hard-coding field-specific view logic.

### Current supported operators
- `eq`
- `neq`
- `in`
- `not_in`
- `gte`
- `lte`
- `contains`
- `exists`

That is enough for version 1 without drifting into query-language complexity.

---

## View style object

The `style` object lets data decide how a view should look.

Supported properties include:
- `node_color_by`
- `node_size_by`
- `node_shape_by`
- `label_mode`
- `edge_visibility`
- `show_legend`
- `show_geo_overlay`

### Why style belongs in data

Because “overview”, “public/hybrid”, and “AI-relevant” should not require separate code branches to look different.
They should be different because the data declares different style rules.

That makes the frontend thinner and the product easier to evolve.

---

## View defaults

The `defaults` object stores interaction assumptions for a view.

Current supported properties:
- `focus_hops`
- `show_profile_on_click`
- `hide_weak_edges_below`

This is enough to influence behavior without turning the schema into a behavior engine.

---

## `layout_presets`

Layouts are stored separately from views because layout is not the same thing as interpretation.

A layout preset defines:
- an ID
- a label
- a type
- optional stored positions
- optional stored viewport

### Supported layout types
- `force`
- `preset`
- `geo`

### Why separate layout from views

Because one view may be rendered with several layouts over time.
And one layout may support more than one view.

Keeping them separate avoids hidden coupling.

---

## `source_refs`

Each source reference is reusable evidence.

### Required fields
- `id`
- `label`
- `type`

### Optional fields
- `publisher`
- `url`
- `published_at`
- `accessed_at`
- `notes`

### Why `type` is constrained

Because evidence should have a consistent trust classification.
Current allowed values:
- `official`
- `primary`
- `secondary`
- `internal_note`

That is enough to let the UI signal source quality later.

---

## Why the schema is strict in some places and loose in others

This is the core balance.

### Strict where consistency matters
- node types
- buyer types
- edge types
- buying modes
- need categories
- relevance levels
- relationship status
- procurement regime
- opportunity band

### Loose where learning is still happening
- sector
- subsector
- narrative text
- sponsor role wording
- notes

This is deliberate.

Too much strictness early creates constant schema churn.
Too little strictness creates incoherent data.

---

## What the schema does not yet try to solve

The schema intentionally does **not** solve these things yet:
- in-app editing permissions
- version history of individual node changes
- relationship timelines
- event nodes
- demand-theme nodes
- collaborative comments
- graph algorithm configuration
- backend persistence rules

Those can be added later if needed.
They should not distort version 1.

---

## Implementation implications for Codex

Codex should treat this schema as a hard contract.

That means:
- invalid JSON should fail loudly
- missing required fields should surface clearly
- the viewer should not silently invent fallback structure
- the app should be built to consume this model directly

The frontend should not “fix” broken data by guessing.
It should expose problems clearly so the source of truth stays trustworthy.

---

## Minimal valid example shape

At a minimum, a valid dataset needs:
- one `meta` object
- at least one node
- zero or more edges
- at least one view
- zero or more layout presets
- zero or more source refs

That means you can bootstrap a tiny valid graph before scaling to the full corridor.

This is intentional. The schema supports incremental build-up.

---

## Bottom line

The schema is not just a validation file.
It is the operational definition of the product’s worldview.

It says:
- what counts as an actor,
- what counts as a relationship,
- what belongs in metadata,
- how interpretation is stored,
- and how the UI should consume the graph.

If the ontology is the philosophy, the schema is the law.
