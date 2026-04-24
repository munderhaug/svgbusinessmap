# 02 — Ontology

## Purpose

This document defines the conceptual model of the market atlas.

It answers six foundational questions:
1. What are the primary things in the system?
2. What kinds of relationships can exist between them?
3. What information belongs on a node versus an edge?
4. What information should be modeled as metadata instead of as a visual object?
5. What assumptions does the product make about actor-centric mapping?
6. What future extensions are allowed without breaking version 1?

This document is the conceptual bridge between:
- the product thesis,
- the canonical JSON schema,
- the UI behavior,
- and the eventual data population work.

If the ontology is wrong, the app will look sophisticated while thinking badly.

---

## Core modeling principle

The atlas is **actor-centric**.

That means:
- **actors are primary nodes**
- **relationships between actors are primary edges**
- **demand, relevance, and commercial interpretation are metadata first**

The default map should answer:

> Who are the relevant actors in this market, what kind of actors are they, how are they connected, and why do they matter?

It should not default to:
- abstract capability diagrams
- service taxonomy networks
- process maps
- sales pipelines
- conceptual strategy boards

---

## Ontology rules

### Rule 1 — Node scarcity

Nodes should represent things that deserve independent identity in the graph.

If something does not need to be clicked, filtered, profiled, or related to multiple other things, it should probably not be a node.

### Rule 2 — Metadata before abstraction

If a concept can be expressed as a field on an actor node without losing analytical power, it should remain a field.

Examples:
- AI relevance
- design relevance
- likely buying mode
- likely needs
- procurement regime
- opportunity band
- municipality

These are attributes, not first-class graph objects in version 1.

### Rule 3 — Edges must mean something specific

An edge is not decorative proximity.

Every edge must express a semantically clear relationship type that can be named and explained.

### Rule 4 — Profiles are part of the model

The ontology must assume that each node can carry a rich profile.

The graph is not the whole product. The profile panel is half the product.

### Rule 5 — One canonical source of truth

The ontology exists independently of the user interface.

The JSON model is canonical. The UI is a rendering and navigation layer over that model.

---

## Unit of analysis

The default unit of analysis is the **actor**.

In practice, an actor is any entity that matters independently in the Stavanger corridor market system.

That includes:
- companies
- public buyers
- hybrid/publicly owned commercial actors
- universities and institutions
- hospitals and health entities
- clusters and ecosystem organizations
- investors when strategically relevant
- business units, but only when they have meaningfully different relevance or buying logic from the parent

The default modeling choice is:
- prefer one actor node per meaningful market actor
- do not split into business-unit nodes unless it solves a real ambiguity

---

## Node taxonomy

Version 1 should support the following node types.

### 1. `organization`

A private operating company.

Use for:
- small, medium, and large private firms
- product companies
- industrial companies
- service companies
- subsidiaries when they matter independently

Examples:
- industrial manufacturer
- SaaS company
- consultancy-adjacent product company
- retail/service chain

### 2. `public_entity`

A public or public-law body whose logic is primarily institutional and procurement-governed.

Use for:
- municipalities
- inter-municipal companies
- universities
- hospitals
- state-linked public bodies
- public agencies when they are in-scope actors

### 3. `hybrid_entity`

A commercially active actor with public ownership, regulated context, mixed procurement behavior, or infrastructure responsibilities.

Use for:
- utilities
- ports
- transport/infrastructure actors
- publicly owned commercial companies
- entities whose business units may behave differently from the legal parent

### 4. `ecosystem_actor`

A non-buying or partially buying actor that matters because it shapes access, visibility, trust, or market coordination.

Use for:
- trade associations
- incubators
- accelerators
- business associations
- convening organizations

### 5. `cluster`

A formal cluster or organized industry network with membership and coordination importance.

Use only when the cluster matters as a distinct connective structure.

### 6. `institution`

A research, educational, or public-purpose institution that is not best modeled as a generic public entity.

Use carefully.

Examples:
- university departments if modeled independently
- research centers
- domain institutes

### 7. `investor`

An actor that matters because it shapes capital access, growth, or ecosystem leverage.

Use sparingly in version 1.

Examples:
- regional seed investor
- venture studio
- strategically important owner or capital actor

### 8. `business_unit`

A sub-entity of a larger organization that has distinct strategic relevance, buyer behavior, or relationship structure.

Use only if all three conditions are met:
1. the unit matters independently,
2. the unit has distinct commercial or demand logic,
3. modeling it separately increases clarity more than clutter.

Business units are an exception type, not a default type.

---

## Node type decision rule

When uncertain about node type, use this order of questions:

1. Is this an actor with independent market significance?
2. Can it be meaningfully profiled?
3. Does it have non-trivial relationships to several other actors?
4. Would hiding it inside another node make the map less truthful?

If the answer is yes to most of these, it can be a node.

If not, it should probably remain metadata.

---

## Edge taxonomy

Every edge must have a typed semantic meaning.

Version 1 should support the following edge types.

### 1. `ownership`

Represents ownership or parent-child control relationships.

Use for:
- parent company -> subsidiary
- group -> owned company
- public owner -> owned company

### 2. `subsidiary_of`

A directional variant of ownership if you want explicit child -> parent semantics.

Version 1 should usually pick one ownership convention and stay consistent.

### 3. `partner_of`

Represents a strategic or operating partnership between actors.

Use for:
- delivery partnerships
- joint initiatives
- close strategic alliances

### 4. `member_of`

Represents membership or formal participation in a cluster, association, or organized network.

Use for:
- company -> cluster
- actor -> industry association

### 5. `collaborates_with`

Represents meaningful but not necessarily formal collaboration.

Use for:
- research collaboration
- innovation partnership
- development cooperation
- project-based public-private work

### 6. `channel_to`

Represents access or route-to-market relationships.

Use for:
- ecosystem actor -> company
- partner -> buyer channel
- cluster -> member access path

This is useful because some actors matter mainly because they unlock access rather than because they are direct buyers.

### 7. `procures_via`

Represents procurement-route relationships.

Use sparingly.

Examples:
- public buyer -> procurement body
- hospital -> centralized procurement organization
- actor -> dynamic purchasing system context

This should not be overused for every procurement detail.

### 8. `adjacent_to`

Represents strategic adjacency rather than formal partnership.

Use carefully.

This is the loosest edge type and should only be used when it adds explanatory value.

Examples:
- two actors in tightly linked value-chain positions
- adjacent product/market roles worth seeing together

### 9. `operates_in`

Represents structural presence in a place.

This is optional in v1 and may be handled better as metadata unless geographic nodes are later introduced.

Default recommendation: keep geography as metadata and filters, not graph edges.

---

## Edge rules

### Rule A — Prefer strong semantics

Use `ownership`, `member_of`, and `partner_of` before using vague types like `adjacent_to`.

### Rule B — Avoid redundancy

Do not create several parallel edges between the same pair unless they represent meaningfully distinct relationships that the UI will expose.

### Rule C — Edge direction should be intentional

If an edge is directional, that direction must mean something the user can understand.

### Rule D — Edge density must remain useful

If a relationship does not change interpretation, filtering, or navigation, it probably should not be shown as an edge.

---

## What is not a node in version 1

These concepts should remain metadata fields unless a later view proves otherwise.

### Demand concepts that stay as fields
- likely needs
- design relevance
- AI relevance
- software relevance
- product relevance
- innovation relevance
- likely buying modes
- likely sponsor roles
- urgency / trigger intensity
- opportunity band

### Commercial interpretation that stays as fields
- market importance
- relationship status
- score overlays
- confidence
- evidence freshness
- access difficulty

### Geography that stays as fields
- municipality
- corridor zone
- district/subarea
- latitude/longitude if needed later

### Organization descriptors that stay as fields
- size band
- sector
- subsector
- buyer type
- procurement regime
- ownership type

The user can still filter and style by these fields. They do not need to become graph objects to be useful.

---

## Buyer type model

Buyer type is not the same thing as node type.

Node type answers:
- what kind of actor is this in the graph?

Buyer type answers:
- what kind of commercial logic does this actor follow?

Version 1 should support these buyer types as metadata:
- `private`
- `public`
- `hybrid`
- `growth`
- `channel`
- `unknown`

Examples:
- a node with type `organization` may have buyer type `private` or `growth`
- a node with type `hybrid_entity` will usually have buyer type `hybrid`
- a node with type `ecosystem_actor` may have buyer type `channel`

This separation matters because graph identity and market logic are not the same thing.

---

## Relationship strength

Edges may optionally carry a `strength` field.

Purpose:
- allow visual de-emphasis of weak relationships
- support filtering or opacity rules
- help reduce clutter in dense views

Recommended version 1 meaning:
- `1.0` = highly material / foundational relationship
- `0.7` = strong and strategically meaningful
- `0.4` = moderate / useful context
- `0.2` = weak / optional / contextual only

This should be treated as a display aid, not fake precision.

---

## Node field groups

Each node should organize information into field groups.

### 1. Identity fields
- id
- label
- aliases
- node type
- buyer type

### 2. Classification fields
- sector
- subsector
- ownership type
- size band
- procurement regime

### 3. Location fields
- municipality
- corridor zone
- subarea / district
- optional coordinates

### 4. Demand and relevance fields
- likely needs
- likely buying modes
- design relevance
- AI relevance
- software relevance
- product relevance
- opportunity band

### 5. Relationship interpretation fields
- cluster memberships
- parent/group context
- partner notes
- ecosystem role

### 6. Profile narrative fields
- summary
- why it matters
- what makes it relevant
- analyst notes

### 7. Evidence fields
- source refs
- freshness
- confidence

### 8. Optional scoring fields
- commercial attractiveness
- short-term opportunity
- access difficulty

These belong on the node but should be visually secondary unless a view explicitly uses them.

---

## Profiles as ontology surface

A node is not complete without a profile.

Every node profile should support at least these conceptual sections:

### Header
- label
- actor type
- buyer type
- municipality
- sector

### Summary
- what this actor is
- why it matters in the corridor

### Market interpretation
- likely needs
- likely buying mode
- relevance pattern
- procurement behavior where relevant

### Relationship context
- parent/group
- memberships
- key linked actors

### Evidence
- sources
- freshness
- confidence

The profile is not an afterthought. It is where ambiguity gets resolved.

---

## Views as ontology overlays

Views do not change the ontology.

Views are rule-based lenses over the same underlying graph.

That means a view can:
- hide some node types
- hide some edge types
- change color
- change size
- change edge opacity
- apply filters
- emphasize certain fields

But a view should not redefine what the data means.

This is a critical modeling discipline.

---

## Metrics as overlays, not identity

Graph metrics may later exist as computed properties, but they are not part of actor identity.

Examples:
- degree
- weighted degree
- hub score
- centrality-derived display emphasis

These can influence style or prioritization later.

They should not replace the underlying semantic model.

---

## Geo as a separate lens

Geographic information should exist in the ontology, but geography should not dominate the core model.

Version 1 recommendation:
- store location on each node
- allow a geographic view later
- do not make municipality a node in the default graph

Reason:
- the product is about market structure, not cartography
- the main truth lies in relationships and profiles
- place is important, but not the primary organizing logic

---

## Extension rules for version 2+

The ontology may later extend in controlled ways.

Possible future additions:
- demand-theme nodes in a dedicated demand view
- event / trigger nodes in a timeline or event layer
- geography nodes if a place-based analysis becomes central
- loop structures if cyclical system logic becomes important
- in-app annotations if review workflows become necessary

But none of these should be added until the actor-centric model proves insufficient.

---

## Anti-patterns

Avoid these mistakes.

### Anti-pattern 1 — Attribute inflation into nodes
Turning every field into a node.

Result:
- visual clutter
- category confusion
- unreadable graph

### Anti-pattern 2 — Over-splitting companies
Creating business-unit nodes for every internal division.

Result:
- false precision
- duplicate profiles
- exploded graph size

### Anti-pattern 3 — Decorative edges
Adding connections because “they seem related.”

Result:
- loss of semantic trust
- unreadable neighborhoods

### Anti-pattern 4 — Mixed ontological levels
Putting companies, services, sectors, needs, and tactics on the same plane with no rules.

Result:
- concept map chaos instead of market intelligence

### Anti-pattern 5 — UI-first ontology
Letting the graph library determine the model.

Result:
- accidental editor logic
- fragile schema
- difficult future migration

---

## Canonical modeling examples

### Example A — private company

- node type: `organization`
- buyer type: `private`
- sector: `industrial_technology`
- likely needs: `software_modernization`, `product_development`
- likely buying modes: `pod`, `team`
- key relationships: parent group, cluster membership, strategic partnerships

### Example B — municipality

- node type: `public_entity`
- buyer type: `public`
- sector: `public_services`
- likely needs: `service_design`, `digital_service_improvement`, `AI_enablement`
- likely buying mode: `managed_delivery`, `framework_route`
- key relationships: procurement route, institutional collaborators

### Example C — utility group with mixed logic

- node type: `hybrid_entity`
- buyer type: `hybrid`
- likely needs: `customer_experience`, `software_modernization`, `AI_enablement`
- likely buying modes: `specialist`, `pod`, `team`, `partner`
- may later require business-unit nodes if different divisions behave materially differently

### Example D — cluster

- node type: `cluster`
- buyer type: `channel`
- likely needs: none or secondary
- main value: access, signaling, ecosystem visibility
- key relationships: `member_of`, `channel_to`, `collaborates_with`

---

## Decision checklist

When adding a new concept, ask:

1. Is this a real actor or a property of an actor?
2. Does it need an independent profile?
3. Will users search for it as a thing?
4. Will it have multiple meaningful relationships?
5. Does making it a node improve clarity more than it increases clutter?

If the answer is no to most of these, keep it as metadata.

---

## Bottom line

The ontology for version 1 is intentionally conservative.

It is built around:
- actor nodes,
- typed relationships,
- rich profiles,
- metadata-driven filtering,
- and view-based interpretation.

That restraint is a strength.

The goal is not to model everything that can be said about the market.

The goal is to model the market in a way that stays **truthful, navigable, and extensible**.
