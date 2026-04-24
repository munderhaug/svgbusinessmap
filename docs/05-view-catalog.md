# 05 — View catalog

## Purpose

This document defines the first set of saved views the market atlas should support.

The product is not meant to behave like one giant flat graph.
It should behave like one underlying dataset explored through multiple disciplined lenses.

This follows the most useful Kumu concept to borrow for the product: a **view** is a collection of filters, decorations, and settings that changes what is visible and how it is styled without changing the underlying map data. ([Kumu Views Guide](https://docs.kumu.io/guides/views))

The goal of this document is to tell Codex:
- which views exist in version 1
- what each view is for
- how each view should look by default
- what questions each view should help answer
- what should and should not be visible in each view

---

## Design principles for views

### 1. One dataset, many lenses

A view is not a duplicate graph.
A view is a rule-based interpretation of the same graph.

### 2. Every view must answer a distinct question

If two views answer the same question, one of them should be removed.

### 3. Views should reduce complexity, not multiply it

A new view is justified only if it makes navigation or interpretation meaningfully easier.

### 4. Views are product features, not analyst conveniences

The user should be able to understand what a view is for without needing to inspect raw JSON.

---

## Shared interaction expectations across all views

Unless overridden, every view should inherit these defaults:

- click node -> open right-side profile panel
- hover node -> highlight first-degree neighborhood
- search operates across `label`, `aliases`, `sector`, `subsector`, and profile text
- weak edges may be visually de-emphasized
- filters combine with AND logic
- the graph should always remain explorable without showing every label at once

---

## View list for version 1

The first version should support these ten views:

1. `overview`
2. `private_buyers`
3. `public_and_hybrid`
4. `growth_and_product_led`
5. `design_relevant`
6. `ai_relevant`
7. `software_and_product`
8. `geographic_corridor`
9. `ecosystem_hubs`
10. `opportunity_overlay`

---

# 1. `overview`

## Purpose

This is the default opening lens.

It should answer:

> Who are the relevant actors in the corridor, what kinds of actors are they, and how are they broadly connected?

## Visible node scope
- all node types except optional future-only experimental types

## Visible edge scope
- `ownership`
- `partner_of`
- `member_of`
- `collaborates_with`
- `channel_to`

Hide by default:
- weak `adjacent_to`
- low-signal `operates_in`
- procurement-route edges unless strategically important

## Default styling
- color by `buyer_type`
- size by neutral or mild `scores.commercial_attractiveness` if present
- shape/badge by `type`
- label mode: `focused`
- edge visibility: `strong_only`

## Default controls
- search
- buyer type filter
- sector filter
- municipality filter
- node type filter
- show/hide weak edges

## When to use it
- first entry point
- general orientation
- stakeholder overview
- identifying major clusters of actors

## What success looks like
The user can open this view and immediately tell:
- what kinds of actors are present
- where the public/private/hybrid balance sits
- which actors look central or isolated

---

# 2. `private_buyers`

## Purpose

This view isolates the private commercial market.

It should answer:

> Which private actors matter, and how are they structured relative to one another?

## Node filter
Include only nodes where:
- `buyer_type = private`

Optionally include:
- `business_unit` nodes belonging to private parents if they materially matter

## Edge filter
Prefer:
- `ownership`
- `partner_of`
- `member_of`
- `collaborates_with`

Hide by default:
- `procures_via`
- weak ecosystem routing edges unless needed

## Default styling
- color by `sector`
- size by `scores.commercial_attractiveness`
- label mode: `focused`
- edge visibility: `strong_only`

## Default controls
- sector filter
- size band filter
- design relevance filter
- AI relevance filter
- software relevance filter
- opportunity band filter

## When to use it
- exploring the private market only
- spotting sector concentration
- identifying strong private targets without public noise

---

# 3. `public_and_hybrid`

## Purpose

This view isolates the actors whose logic is shaped by procurement, public ownership, or regulated hybrid behavior.

It should answer:

> Which public and hybrid actors matter, and what structures shape access to them?

## Node filter
Include nodes where:
- `buyer_type = public`
- `buyer_type = hybrid`

## Edge filter
Prioritize:
- `ownership`
- `procures_via`
- `collaborates_with`
- `channel_to`
- `member_of`

## Default styling
- color by `attributes.procurement_regime`
- size by `scores.access_difficulty` or `scores.commercial_attractiveness` depending on toggle
- label mode: `focused`
- edge visibility: `focused_only` or `strong_only`

## Default controls
- procurement regime filter
- municipality filter
- node type filter
- confidence filter
- relationship status filter

## When to use it
- exploring public-sector structure
- separating public from hybrid procurement logic
- understanding institutional access routes

## Important note
This view should avoid turning into a procurement workflow board.
It is still a structural market view.

---

# 4. `growth_and_product_led`

## Purpose

This view isolates actors more likely to behave like product or growth buyers.

It should answer:

> Which actors are product-led or growth-oriented, and where might faster-moving demand sit?

## Node filter
Include nodes where:
- `buyer_type = growth`

Optionally also include:
- `organization` nodes with high `product_relevance`
- selected `investor` or `ecosystem_actor` nodes if they clarify the growth ecosystem

## Edge filter
Prioritize:
- `partner_of`
- `member_of`
- `channel_to`
- `collaborates_with`

## Default styling
- color by `sector`
- size by `scores.short_term_opportunity`
- label mode: `focused`
- edge visibility: `strong_only`

## Default controls
- product relevance filter
- AI relevance filter
- likely buying mode filter
- opportunity band filter

## When to use it
- identifying likely fast movers
- spotting product-heavy companies
- exploring scale-up and ecosystem structure

---

# 5. `design_relevant`

## Purpose

This view highlights actors where design is likely to matter.

It should answer:

> Where is design likely to be a meaningful wedge in this market?

## Node filter
Include nodes where:
- `attributes.design_relevance = high`
- optionally also `medium` if user expands the filter

## Edge filter
Prefer structural edges already visible in other views.
Do not invent demand edges.

## Default styling
- color by `attributes.design_relevance`
- size by `scores.short_term_opportunity`
- label mode: `top_ranked` or `focused`
- edge visibility: `focused_only`

## Default controls
- buyer type filter
- municipality filter
- likely needs filter
- buying mode filter

## When to use it
- design-led opportunity scanning
- comparing public and private design relevance
- seeing where service design vs UI/UX could matter

---

# 6. `ai_relevant`

## Purpose

This view highlights actors where AI is likely to be relevant.

It should answer:

> Where does AI seem commercially or operationally relevant across the market?

## Node filter
Include nodes where:
- `attributes.ai_relevance = high`
- optionally `medium` on expansion

## Edge filter
Use standard relationship edges.
Do not create AI nodes in version 1.

## Default styling
- color by `sector`
- size by `attributes.ai_relevance` or `scores.short_term_opportunity`
- label mode: `top_ranked`
- edge visibility: `focused_only`

## Default controls
- buyer type filter
- procurement regime filter
- opportunity band filter
- confidence filter

## When to use it
- AI-specific opportunity scanning
- identifying sector clusters of AI relevance
- distinguishing hype from structural relevance

---

# 7. `software_and_product`

## Purpose

This view highlights actors where software and product work are likely to matter.

It should answer:

> Which actors look most relevant for software/product delivery rather than only advisory or service-design work?

## Node filter
Include nodes where either:
- `attributes.software_relevance = high`
- `attributes.product_relevance = high`

## Edge filter
Use standard structural edges, especially those that expose company and ecosystem topology.

## Default styling
- color by `sector`
- size by combined relevance or `scores.commercial_attractiveness`
- label mode: `focused`
- edge visibility: `strong_only`

## Default controls
- software relevance filter
- product relevance filter
- likely buying mode filter
- size band filter

## When to use it
- product delivery targeting
- identifying where pods/teams make sense
- comparing different sectors' product intensity

---

# 8. `geographic_corridor`

## Purpose

This is the place-oriented lens.

It should answer:

> How is the actor landscape distributed across the corridor from Egersund to Rennesøy?

## Node scope
- all relevant actors with location metadata

## Edge scope
- keep structural edges available, but visually subordinate them to place

## Default styling
- color by `buyer_type`
- size by `scores.commercial_attractiveness` or neutral size
- geo overlay: true
- label mode: `top_ranked`
- edge visibility: `focused_only`

## Default controls
- municipality filter
- corridor zone filter
- buyer type filter
- sector filter

## When to use it
- corridor scanning
- comparing municipality density
- understanding geographic concentration of actors

## Important note
This should be a secondary lens, not the product’s default worldview.
The product is relationship-first, not map-pin-first.

---

# 9. `ecosystem_hubs`

## Purpose

This view highlights connective actors rather than just buyers.

It should answer:

> Which actors function as hubs, connectors, clusters, or routes into the market?

## Node scope
Prioritize:
- `ecosystem_actor`
- `cluster`
- `institution`
- selected high-degree organizations

## Edge scope
Prioritize:
- `member_of`
- `channel_to`
- `partner_of`
- `collaborates_with`

## Default styling
- color by `type`
- size by graph metric overlay when available (for example degree)
- label mode: `all` for a trimmed subset or `focused` for larger sets
- edge visibility: `all` or `strong_only` depending on density

## Default controls
- node type filter
- metric selector
- municipality filter
- buyer type filter

## When to use it
- finding leverage points
- spotting non-obvious connectors
- understanding the market as a system rather than a target list

This aligns well with Kumu’s metrics concept, where graph metrics are used to decorate or interpret a map without changing the underlying data model. ([Kumu Metrics Guide](https://docs.kumu.io/guides/metrics))

---

# 10. `opportunity_overlay`

## Purpose

This view is the commercial overlay view.

It should answer:

> Given the current interpretation data, which actors appear most commercially interesting right now?

## Node scope
- all nodes with opportunity-related metadata

## Edge scope
- standard structural edges
- weak edges hidden by default

## Default styling
- color by `attributes.opportunity_band`
- size by `scores.commercial_attractiveness`
- optional halo/badge by `scores.confidence_level`
- label mode: `top_ranked`
- edge visibility: `focused_only`

## Default controls
- opportunity band filter
- confidence filter
- buyer type filter
- procurement regime filter
- likely needs filter

## When to use it
- high-level prioritization
- comparing public/private/hybrid opportunity patterns
- quickly scanning where the strongest overlays sit

## Important note
This is still a **market view**, not a sales pipeline.
It must not acquire stage, owner, or workflow behavior.

---

## Recommended default order in the UI

The order of views in the UI should be:

1. Overview
2. Private buyers
3. Public and hybrid
4. Growth and product-led
5. Design relevant
6. AI relevant
7. Software and product
8. Geographic corridor
9. Ecosystem hubs
10. Opportunity overlay

Why this order?
Because it moves from:
- broad orientation
- to market segmentation
- to capability lenses
- to geography
- to system structure
- to commercial overlay

That sequence feels more natural than opening with a highly opinionated or overly operational view.

---

## Views that should NOT exist in version 1

Do not create these yet:
- procurement workflow view
- pipeline view
- meeting status view
- source quality dashboard view
- editing/admin view
- event timeline view
- demand-theme-node view
- relationship audit view for end users

These may become useful later, but they are not core to a readable version 1.

---

## Kumu-inspired controls to borrow conceptually

The Kumu controls system is a useful reference because it treats filters, focus, clustering, legends, and view toggles as explicit interaction components rather than hidden state. That is the right product philosophy to borrow. ([Kumu Controls Guide](https://docs.kumu.io/guides/controls), [Kumu Controls Reference](https://docs.kumu.io/overview/advanced-editor-hub/controls-reference))

Version 1 of this product should conceptually support:
- search
- filter controls
- focus control
- view toggle
- legend
- metric selector
- optional clustering or grouping behavior later

But these should be implemented in a way that fits the app’s own UI, not copied from Kumu literally.

---

## Bottom line

The product should not feel like:
- one overwhelming graph,
- or a dashboard with a graph attached.

It should feel like:
- one coherent market atlas,
- with several intentional ways of seeing the same system.

That is what this view catalog is meant to enforce.
