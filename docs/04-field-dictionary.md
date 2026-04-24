# 04 — Field dictionary

## Purpose

This document defines the meaning, display intent, and operational use of every field in the canonical schema.

It exists to stop three common failure modes:
- different people using the same field to mean different things
- Codex guessing how a field should be displayed or filtered
- future data-entry work drifting away from the ontology

This file should be read together with:
- `02-ontology.md`
- `03-schema-explainer.md`
- `schema/market-map.schema.json`

---

## Reading guide

Each field entry includes:
- **Key** — canonical JSON field name
- **NO label** — recommended Norwegian display label
- **EN label** — recommended English display label
- **Type** — data type
- **Required** — whether the schema requires it
- **Allowed values** — enum or expected pattern
- **Filter** — whether the UI should allow filtering on it
- **Profile** — whether it should appear in the node/edge profile panel
- **Style** — whether it may drive color, size, shape, badges, or emphasis
- **Notes** — interpretation guidance

### Display rules

- Canonical keys stay in **English snake_case**
- User-facing labels may be **Norwegian-first** in the UI
- The UI may later support language toggling, but the data model stays stable

---

# A. Top-level dataset fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `meta` | Metadata | Metadata | object | yes | schema object | no | no | no | Dataset-level information |
| `nodes` | Noder | Nodes | array | yes | node objects | yes | yes | yes | Primary actor list |
| `edges` | Relasjoner | Edges | array | yes | edge objects | yes | yes | yes | Typed relationships |
| `views` | Visninger | Views | array | yes | view objects | yes | no | yes | Saved interpretation lenses |
| `layout_presets` | Layoutforvalg | Layout presets | array | yes | layout objects | yes | no | yes | Saved placements / viewports |
| `source_refs` | Kilder | Source refs | array | yes | source objects | yes | yes | no | Reusable evidence library |

---

# B. `meta` fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `meta.project_id` | Prosjekt-ID | Project ID | string | yes | stable slug/id | no | no | no | Internal stable identifier |
| `meta.title` | Tittel | Title | string | yes | free text | no | no | no | Human-readable dataset title |
| `meta.description` | Beskrivelse | Description | string | no | free text | no | no | no | Short dataset description |
| `meta.version` | Versjon | Version | string | yes | semver `x.y.z` | no | no | no | Dataset contract version |
| `meta.updated_at` | Oppdatert | Updated at | date | yes | ISO date | no | no | no | Last dataset update |
| `meta.default_view_id` | Standardvisning | Default view ID | string | no | valid view id | no | no | yes | Which view the app opens in |
| `meta.locale` | Språk | Locale | string | no | e.g. `en`, `no` | no | no | no | Default display locale |
| `meta.corridor_description` | Korridorbeskrivelse | Corridor description | string | no | free text | no | no | no | Narrative scope reminder |

---

# C. Node identity fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `id` | ID | ID | string | yes | schema pattern | yes | no | no | Stable machine key |
| `label` | Navn | Label | string | yes | free text | yes | yes | yes | Main visible node name |
| `aliases` | Alias | Aliases | array[string] | no | free text | search | yes | no | Improves search and recognition |
| `type` | Nodetype | Node type | enum | yes | see schema | yes | yes | yes | Graph identity class |
| `buyer_type` | Kjøpertype | Buyer type | enum | yes | private/public/hybrid/growth/channel/unknown | yes | yes | yes | Commercial logic, not graph identity |
| `sector` | Sektor | Sector | string | yes | controlled later | yes | yes | yes | Broad market grouping |
| `subsector` | Undersektor | Subsector | string | no | controlled later | yes | yes | yes | Finer market grouping |
| `tags` | Tagger | Tags | array[string] | yes | free text | yes | yes | badge | Lightweight editorial labeling |
| `source_refs` | Kildereferanser | Source refs | array[id] | yes | source ids | yes | yes | no | Evidence pointers |

### `type` allowed values
- `organization`
- `public_entity`
- `hybrid_entity`
- `ecosystem_actor`
- `cluster`
- `institution`
- `investor`
- `business_unit`

### `buyer_type` allowed values
- `private`
- `public`
- `hybrid`
- `growth`
- `channel`
- `unknown`

---

# D. Node location fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `location` | Lokasjon | Location | object | yes | schema object | yes | yes | no | Structured geography |
| `location.municipality` | Kommune | Municipality | string | yes | free text | yes | yes | group/view | Primary geographic filter |
| `location.subarea` | Delområde | Subarea | string | no | free text | yes | yes | group/view | E.g. Rennesøy, Bryne area, Ullandhaug |
| `location.zone` | Korridorsone | Corridor zone | enum | yes | see schema | yes | yes | color/group | High-level corridor segmentation |
| `location.latitude` | Breddegrad | Latitude | number | no | -90 to 90 | geo | no | geo | For optional geographic layout |
| `location.longitude` | Lengdegrad | Longitude | number | no | -180 to 180 | geo | no | geo | For optional geographic layout |

### `location.zone` allowed values
- `south`
- `south_jaeren`
- `east_jaeren`
- `core`
- `north_edge`
- `outside_corridor`
- `unknown`

---

# E. Node attribute fields

## E1. Organizational attributes

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `attributes.size_band` | Størrelsesklasse | Size band | enum | yes | micro/small/medium/large/enterprise/unknown | yes | yes | size/secondary badge | Coarse size proxy |
| `attributes.ownership_type` | Eierform | Ownership type | enum | yes | private/public/mixed/foundation/cooperative/unknown | yes | yes | badge | Legal/ownership logic |
| `attributes.procurement_regime` | Anskaffelsesregime | Procurement regime | enum | yes | see schema | yes | yes | badge/color overlay | Critical for public/hybrid interpretation |
| `attributes.relationship_status` | Relasjonsstatus | Relationship status | enum | yes | none/known/warm/active/existing_client/unknown | yes | yes | badge | Internal relationship awareness |
| `attributes.opportunity_band` | Mulighetsnivå | Opportunity band | enum | yes | tier_1/tier_2/tier_3/tier_4/watchlist/unknown | yes | yes | size/color/halo | Commercial overlay, not identity |

### `attributes.size_band` allowed values
- `micro`
- `small`
- `medium`
- `large`
- `enterprise`
- `unknown`

### `attributes.ownership_type` allowed values
- `private`
- `public`
- `mixed`
- `foundation`
- `cooperative`
- `unknown`

### `attributes.procurement_regime` allowed values
- `private`
- `public_procurement`
- `utilities`
- `hybrid`
- `framework`
- `dps`
- `health_centralized`
- `unknown`

### `attributes.relationship_status` allowed values
- `none`
- `known`
- `warm`
- `active`
- `existing_client`
- `unknown`

### `attributes.opportunity_band` allowed values
- `tier_1`
- `tier_2`
- `tier_3`
- `tier_4`
- `watchlist`
- `unknown`

## E2. Relevance attributes

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `attributes.design_relevance` | Designrelevans | Design relevance | enum | yes | none/low/medium/high | yes | yes | color/filter | Relevance to design/services/UX |
| `attributes.ai_relevance` | KI-relevans | AI relevance | enum | yes | none/low/medium/high | yes | yes | color/filter | Relevance to AI/automation/data-led wedge |
| `attributes.software_relevance` | Programvarerelevans | Software relevance | enum | yes | none/low/medium/high | yes | yes | color/filter | Relevance to software delivery |
| `attributes.product_relevance` | Produktrelevans | Product relevance | enum | yes | none/low/medium/high | yes | yes | color/filter | Relevance to product work |

## E3. Demand attributes

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `attributes.likely_needs` | Sannsynlige behov | Likely needs | array[enum] | yes | see schema | yes | yes | highlight/filter | Core demand profile |
| `attributes.likely_buying_modes` | Sannsynlige kjøpsformer | Likely buying modes | array[enum] | yes | see schema | yes | yes | highlight/filter | How the actor is likely to buy |

### `attributes.likely_needs` allowed values
- `design_execution`
- `service_design`
- `strategic_design`
- `innovation_support`
- `ai_enablement`
- `software_modernization`
- `software_development`
- `product_development`
- `team_augmentation`
- `development_partner`

### `attributes.likely_buying_modes` allowed values
- `specialist`
- `paired_specialists`
- `pod`
- `team`
- `managed_delivery`
- `retained_partner`
- `framework_route`
- `unknown`

---

# F. Node profile fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `profile.summary` | Oppsummering | Summary | string | yes | free text | search | yes | no | Short actor description |
| `profile.why_it_matters` | Hvorfor viktig | Why it matters | string | yes | free text | search | yes | no | Why this actor matters in the map |
| `profile.relevant_to_olavstoppen` | Relevans for Olavstoppen | Relevant to Olavstoppen | string | no | free text | search | yes | no | Human commercial interpretation |
| `profile.likely_sponsor_roles` | Sannsynlige sponsorroller | Likely sponsor roles | array[string] | no | free text | yes | yes | badge optional | Who might own the need internally |
| `profile.notes` | Notater | Notes | array[string] | no | free text | no | yes | no | Concise analyst notes |

### Profile writing rules
- Keep `summary` factual and compact
- Keep `why_it_matters` interpretive but grounded
- Use `relevant_to_olavstoppen` for commercial framing
- Keep `notes` short and atomic, not essay-like

---

# G. Node score fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `scores.commercial_attractiveness` | Kommersiell attraktivitet | Commercial attractiveness | number | no | 0–100 | yes | yes | size/color | Optional commercial overlay |
| `scores.short_term_opportunity` | Kortsiktig mulighet | Short-term opportunity | number | no | 0–100 | yes | yes | size/color | Timing-oriented overlay |
| `scores.access_difficulty` | Tilgangsvanskelighet | Access difficulty | number | no | 0–100 | yes | yes | opacity/color | Friction overlay |
| `scores.confidence_level` | Tillitsnivå | Confidence level | enum | yes | low/medium/high | yes | yes | badge | Indicates evidence strength |
| `scores.score_date` | Scoringsdato | Score date | date | no | ISO date | yes | yes | no | Indicates freshness of scoring |

### Score display rules
- Never treat scores as node identity
- Scores may influence size or secondary styling in some views
- The default overview should not visually over-index on scores unless explicitly requested by the view

---

# H. Edge fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `id` | Relasjons-ID | Edge ID | string | yes | schema pattern | yes | no | no | Stable machine key |
| `source` | Kilde | Source | string | yes | node id | yes | no | no | Start node ID |
| `target` | Mål | Target | string | yes | node id | yes | no | no | End node ID |
| `type` | Relasjonstype | Edge type | enum | yes | see schema | yes | yes | style/filter | Typed relationship meaning |
| `label` | Etikett | Label | string | no | free text | search | yes | edge label | Human-readable relation label |
| `directed` | Rettet | Directed | boolean | yes | true/false | yes | no | arrow | Whether relation direction matters |
| `strength` | Styrke | Strength | number | yes | 0–1 | yes | yes | opacity/width | Visual importance proxy |
| `attributes.notes` | Notater | Notes | array[string] | no | free text | no | yes | no | Concise relationship notes |
| `attributes.confidence_level` | Tillitsnivå | Confidence level | enum | no | low/medium/high | yes | yes | badge | Relationship evidence confidence |
| `tags` | Tagger | Tags | array[string] | yes | free text | yes | yes | badge | Editorial or temporary labels |
| `source_refs` | Kildereferanser | Source refs | array[id] | yes | source ids | yes | yes | no | Evidence pointers |

### `type` allowed values
- `ownership`
- `subsidiary_of`
- `partner_of`
- `member_of`
- `collaborates_with`
- `channel_to`
- `procures_via`
- `adjacent_to`
- `operates_in`

### Edge usage rules
- Prefer strong semantic types over vague ones
- `adjacent_to` should be rare
- `strength` is a display aid, not pseudo-scientific precision

---

# I. View fields

## I1. View identity

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `views[].id` | Visnings-ID | View ID | string | yes | schema pattern | yes | no | no | Stable key |
| `views[].label` | Visningsnavn | View label | string | yes | free text | yes | no | yes | User-facing view name |
| `views[].description` | Beskrivelse | Description | string | no | free text | no | no | no | Explains the lens |
| `views[].layout_preset_id` | Layoutforvalg-ID | Layout preset ID | string | no | valid layout id | yes | no | yes | Optional saved layout binding |

## I2. View filters

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `views[].node_filters` | Nodefiltre | Node filters | array[filterRule] | yes | rule objects | yes | no | yes | Rules applied to nodes |
| `views[].edge_filters` | Relasjonsfiltre | Edge filters | array[filterRule] | yes | rule objects | yes | no | yes | Rules applied to edges |
| `views[].node_filters[].field` | Felt | Field | string | yes | dotted path | no | no | no | e.g. `buyer_type`, `attributes.ai_relevance` |
| `views[].node_filters[].operator` | Operator | Operator | enum | yes | see schema | no | no | no | Rule logic |
| `views[].node_filters[].value` | Verdi | Value | scalar/array | no | depends on operator | no | no | no | Comparison target |

### Allowed operators
- `eq`
- `neq`
- `in`
- `not_in`
- `gte`
- `lte`
- `contains`
- `exists`

## I3. View style

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `views[].style.node_color_by` | Nodefarge etter | Node color by | string | no | field path | no | no | yes | Controls node color encoding |
| `views[].style.node_size_by` | Nodestørrelse etter | Node size by | string | no | field path | no | no | yes | Controls node size encoding |
| `views[].style.node_shape_by` | Nodeform etter | Node shape by | string | no | field path | no | no | yes | Optional shape logic |
| `views[].style.label_mode` | Etikettmodus | Label mode | enum | no | focused/all/none/top_ranked | no | no | yes | Controls label density |
| `views[].style.edge_visibility` | Relasjonssynlighet | Edge visibility | enum | no | all/strong_only/focused_only | no | no | yes | Edge display policy |
| `views[].style.show_legend` | Vis forklaring | Show legend | boolean | no | true/false | no | no | yes | Whether legend is visible |
| `views[].style.show_geo_overlay` | Vis geo-lag | Show geo overlay | boolean | no | true/false | no | no | yes | Whether geo hints are shown |

## I4. View defaults

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `views[].defaults.focus_hops` | Fokusdybde | Focus hops | integer | no | 1–3 | no | no | behavior | Neighborhood depth on focus |
| `views[].defaults.show_profile_on_click` | Vis profil ved klikk | Show profile on click | boolean | no | true/false | no | no | behavior | Standard click behavior |
| `views[].defaults.hide_weak_edges_below` | Skjul svake relasjoner under | Hide weak edges below | number | no | 0–1 | no | no | behavior | Threshold for clutter control |

---

# J. Layout preset fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `layout_presets[].id` | Layout-ID | Layout ID | string | yes | schema pattern | yes | no | no | Stable key |
| `layout_presets[].label` | Navn | Label | string | yes | free text | yes | no | yes | Human name |
| `layout_presets[].type` | Layouttype | Layout type | enum | yes | force/preset/geo | yes | no | yes | Rendering intent |
| `layout_presets[].positions` | Posisjoner | Positions | array | no | layoutPosition objects | no | no | yes | Saved node coordinates |
| `layout_presets[].viewport` | Utsnitt | Viewport | object | no | x/y/zoom | no | no | yes | Saved camera state |
| `layout_presets[].positions[].node_id` | Node-ID | Node ID | string | yes | valid node id | no | no | no | Node being positioned |
| `layout_presets[].positions[].x` | X | X | number | yes | any number | no | no | no | Canvas coordinate |
| `layout_presets[].positions[].y` | Y | Y | number | yes | any number | no | no | no | Canvas coordinate |
| `layout_presets[].positions[].pinned` | Låst | Pinned | boolean | no | true/false | no | no | no | Whether layout should preserve position |
| `layout_presets[].viewport.x` | X | X | number | no | any number | no | no | no | Camera x |
| `layout_presets[].viewport.y` | Y | Y | number | no | any number | no | no | no | Camera y |
| `layout_presets[].viewport.zoom` | Zoom | Zoom | number | no | > 0 | no | no | no | Camera zoom |

---

# K. Source reference fields

| Key | NO label | EN label | Type | Required | Allowed values | Filter | Profile | Style | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `source_refs[].id` | Kilde-ID | Source ID | string | yes | schema pattern | yes | no | no | Stable source key |
| `source_refs[].label` | Navn | Label | string | yes | free text | yes | yes | no | Short source label |
| `source_refs[].type` | Kildetype | Source type | enum | yes | official/primary/secondary/internal_note | yes | yes | badge | Trust class |
| `source_refs[].publisher` | Utgiver | Publisher | string | no | free text | yes | yes | no | Source publisher |
| `source_refs[].url` | URL | URL | uri | no | valid URI | no | yes | no | Direct reference |
| `source_refs[].published_at` | Publisert | Published at | date | no | ISO date | yes | yes | no | When source was published |
| `source_refs[].accessed_at` | Åpnet | Accessed at | date | no | ISO date | yes | yes | no | When analyst accessed source |
| `source_refs[].notes` | Notater | Notes | array[string] | no | free text | no | yes | no | Context or interpretation |

### `source_refs[].type` allowed values
- `official`
- `primary`
- `secondary`
- `internal_note`

---

# L. Recommended display hierarchy

## L1. Always visible in node profile header
- `label`
- `type`
- `buyer_type`
- `sector`
- `location.municipality`

## L2. Usually visible in profile summary section
- `profile.summary`
- `profile.why_it_matters`
- `profile.relevant_to_olavstoppen`

## L3. Usually visible in market interpretation section
- `attributes.likely_needs`
- `attributes.likely_buying_modes`
- `attributes.procurement_regime`
- relevance fields
- `scores.*`

## L4. Usually visible in evidence section
- `source_refs`
- `scores.confidence_level`
- `scores.score_date`

---

# M. Recommended filter priorities for v1

The first version of the UI does not need a filter for every field on day one.

## Highest-priority filters
- `buyer_type`
- `sector`
- `location.municipality`
- `location.zone`
- `attributes.procurement_regime`
- `attributes.likely_needs`
- `attributes.likely_buying_modes`
- `attributes.opportunity_band`
- `scores.confidence_level`

## Second-priority filters
- `type`
- `subsector`
- `attributes.relationship_status`
- relevance fields
- score ranges

## Lower-priority filters
- tags
- source type
- score date
- ownership type

---

# N. Recommended style priorities for v1

The UI should not style by everything.

## Good default style drivers
- node color: `buyer_type`
- node size: chosen metric or neutral fixed size
- badges: `type`, `procurement_regime`, `opportunity_band`
- edge opacity: `strength`

## Conditional style drivers by view
- `attributes.ai_relevance`
- `attributes.design_relevance`
- `scores.commercial_attractiveness`
- `scores.short_term_opportunity`

## Avoid as default style drivers
- free-text notes
- aliases
- source count
- narrative fields

---

# O. Data-entry warnings

## Do not misuse these fields

### `tags`
Do not use tags to encode what should be structured fields.

### `profile.notes`
Do not turn notes into long essays.

### `sector`
Do not use inconsistent naming variants for the same sector.

### `opportunity_band`
Do not confuse this with a CRM stage.

### `relationship_status`
Do not overload this with pipeline semantics.

### `strength`
Do not present this as mathematically exact truth.

---

# P. Bottom line

This field dictionary is the operational language of the product.

It tells Codex:
- what each field means,
- what each field is allowed to contain,
- what should be filterable,
- what belongs in profiles,
- and what may influence visual styling.

If the schema is the law, this document is the glossary the law depends on.
