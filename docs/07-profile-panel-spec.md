# 07 — Profile panel spec

## Purpose

This document defines the right-side profile panel for the market atlas.

The panel is a first-class product surface.
The canvas shows structure. The profile explains meaning.

Kumu’s profile model is a useful conceptual reference because it treats each element and connection as having a profile with default fields and custom fields, and allows views to bring profile information to life through decorations, filters, geo maps, and other mechanisms. ([Kumu Profiles Guide](https://docs.kumu.io/guides/profiles))

This product should borrow that principle, but implement it in its own UI.

---

## Product role of the profile panel

The profile panel exists to solve a core graph problem:

> A map can show relationships at a glance, but it cannot carry all the context needed to interpret an actor well.

The panel should therefore do four jobs:

1. identify the selected actor clearly
2. explain why the actor matters
3. surface commercial and structural interpretation without cluttering the graph
4. ground interpretation in evidence

If the graph is the atlas surface, the profile is the atlas article.

---

## High-level panel rules

### 1. Single active profile

Version 1 supports one active profile at a time.

The panel may show:
- a node profile
- an edge profile
- an empty state

It should not support multi-select profile comparison in version 1.

### 2. Scrollable, sectioned layout

The panel should be a single vertical scroll area with clearly separated sections.

Avoid tabs for version 1.
Tabs usually hide information architecture problems instead of solving them.

### 3. Progressive importance

The top of the panel should contain:
- identity
- classification
- summary

Lower sections should contain:
- relationship context
- demand interpretation
- evidence
- notes

### 4. Compact but information-rich

The panel must be capable of holding meaningful intelligence without turning into a wall of text.

---

## Panel states

## State A — Empty state

Shown when no node or edge is selected.

### Purpose
Invite interaction without looking broken.

### Content
- short instruction: “Select an actor to inspect its profile”
- optional hint: “Use search, filters, or click a node”

### Must not include
- placeholder lorem ipsum
- fake statistics
- unnecessary illustrations

---

## State B — Node profile

This is the primary state.

### Section order
1. Header
2. Snapshot
3. Narrative summary
4. Market interpretation
5. Relationship context
6. Evidence
7. Notes

---

## State C — Edge profile

This is a secondary state.

### Section order
1. Edge header
2. Relationship summary
3. Evidence
4. Notes

The edge profile should always feel lighter than the node profile.

---

# Node profile specification

## 1. Header section

This section should remain fixed at the top of the panel or be visually strongest.

### Required content
- `label`
- `type`
- `buyer_type`
- `sector`
- `location.municipality`

### Optional content
- `subsector`
- `location.subarea`
- tags or badges

### Visual treatment
- name large and prominent
- classification badges directly below or beside name
- no paragraphs in the header

### Recommended badges
- buyer type
- node type
- procurement regime, if meaningful
- opportunity band, if present

---

## 2. Snapshot section

This section gives fast structural orientation.

### Purpose
Allow the user to understand the actor in 5–10 seconds without reading narrative text in full.

### Recommended fields
- municipality
- sector / subsector
- size band
- ownership type
- procurement regime
- relationship status

### Format
Use compact key-value rows or chips.
Do not use a large table.

---

## 3. Narrative summary section

This is the first true prose section.

### Required fields
- `profile.summary`
- `profile.why_it_matters`

### Optional field
- `profile.relevant_to_olavstoppen`

### Purpose of each
- `summary` = what this actor is
- `why_it_matters` = why this actor matters in the corridor/system
- `relevant_to_olavstoppen` = why this matters commercially

### Writing rules
- `summary` should be factual and restrained
- `why_it_matters` may be interpretive but should stay grounded
- `relevant_to_olavstoppen` may be more commercially interpretive, but should stay concise

---

## 4. Market interpretation section

This section holds the core commercial metadata that should not dominate the graph canvas.

### Recommended subsection: likely needs
Display:
- `attributes.likely_needs`

Format:
- chips or small labeled pills
- ordered by importance if possible

### Recommended subsection: likely buying modes
Display:
- `attributes.likely_buying_modes`

Format:
- chips or short bullet list

### Recommended subsection: relevance profile
Display:
- design relevance
- AI relevance
- software relevance
- product relevance

Format:
- compact meter, badge row, or simple labeled scale

### Recommended subsection: sponsor roles
Display:
- `profile.likely_sponsor_roles`

Format:
- short list

### Important rule
Do not overload this section with workflow language.
This is still an interpretation panel, not a sales action board.

---

## 5. Relationship context section

The graph shows direct structure visually, but the panel should summarize the most important parts in readable language.

### Recommended content
- parent/group relation if any
- number of visible first-degree connections
- cluster memberships
- key partner/channel relationships
- key institutional or ecosystem roles

### Display logic
Prefer a mix of:
- short summary sentence
- clickable linked items
- grouped lists by relationship type

### Example groups
- Parent / owner
- Key partners
- Cluster memberships
- Access channels
- Procurement relationships

### Important rule
Do not dump the full adjacency list in raw form.
Summarize intelligently.

---

## 6. Evidence section

This section grounds the profile in sources.

### Purpose
Make the map feel trustworthy.

### Recommended content
- source list from `source_refs`
- publisher
- source type badge (`official`, `primary`, etc.)
- optional date information
- confidence level from `scores.confidence_level`
- score date if relevant

### Display order
Prefer sorting sources by trust level:
1. official
2. primary
3. secondary
4. internal_note

### Optional display
- freshness badge
- expand/collapse notes per source

### Important rule
Sources should be readable without swallowing the whole panel.
Keep source rows compact.

---

## 7. Notes section

This is the lowest-priority node section.

### Source field
- `profile.notes`

### Purpose
Capture concise analyst observations that do not fit cleanly elsewhere.

### Rules
- short bullets only
- no long memos
- no operational to-do list in version 1

---

# Edge profile specification

Edge profiles should exist, but stay compact.

## 1. Edge header

Display:
- source label
- edge type
- target label
- optional human label

Format:
- `Source -> Type -> Target`

---

## 2. Relationship summary

Display:
- edge type
- direction
- strength
- confidence

Optional:
- concise notes

---

## 3. Evidence

Display:
- source refs
- source type
- publisher
- dates if available

---

## 4. Notes

Display only if present.

Keep compact.

---

# Section behavior

## Expand / collapse behavior

Version 1 should support lightweight expand/collapse only where density justifies it.

Recommended default:
- header always expanded
- snapshot expanded
- narrative expanded
- market interpretation expanded
- relationship context expanded
- evidence collapsible
- notes collapsible

This balances clarity with vertical space.

---

## Clickable content inside the panel

The panel should support internal navigation where it helps exploration.

### Click targets that should be interactive
- linked related actors
- linked parent/group actor
- cluster memberships
- source URLs

### Click behavior
When a related actor is clicked:
- that actor becomes selected
- panel updates in place
- graph centers or highlights the new selection

This makes the panel part of the navigation system, not just an information dump.

---

## Visual design rules for the panel

### 1. Strong hierarchy
Use clear section headings and spacing.

### 2. Avoid table overload
Key-value rows are fine for compact fields, but most of the panel should not feel like a spreadsheet.

### 3. Use badges sparingly
Badges are best for:
- buyer type
- node type
- procurement regime
- opportunity band
- confidence level

### 4. Make prose readable
Short paragraphs, not dense text blocks.

### 5. Support scanning first, reading second
The panel should reward both quick scanning and deeper inspection.

---

## Node profile information architecture summary

Recommended order, compactly:

1. **Header**
   - name, type, buyer type, core badges

2. **Snapshot**
   - location, sector, size, procurement, relationship status

3. **Summary**
   - summary
   - why it matters
   - relevant to Olavstoppen

4. **Market interpretation**
   - likely needs
   - likely buying modes
   - relevance profile
   - likely sponsor roles

5. **Relationship context**
   - parent
   - memberships
   - partners
   - channels

6. **Evidence**
   - sources
   - confidence
   - dates

7. **Notes**
   - analyst bullets

---

## What should NOT go in the profile panel in version 1

Do not include:
- CRM activity timeline
- task assignments
- meeting notes history
- email log
- bid pipeline state
- drag-edit forms
- inline source scraping tools
- arbitrary rich-text editing
- comparison mode between multiple actors

Those are adjacent products, not this one.

---

## Profile panel acceptance criteria

The panel is good enough when a user can click an actor and quickly answer:

1. What is this actor?
2. Why does it matter?
3. What kind of need might it have?
4. What kind of actor is it commercially?
5. How is it connected?
6. What evidence supports this interpretation?

If any of those questions requires leaving the panel too quickly, the panel is under-specified.

---

## Bottom line

The profile panel is where the graph becomes useful.

The canvas should attract curiosity.
The panel should reward it with clarity.
