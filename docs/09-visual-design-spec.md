# 09 — Visual design spec

## Purpose

This document defines the visual language for the market atlas.

The app should feel:
- analytical
- calm
- legible
- modern
- deliberate

It should **not** feel like:
- a flashy startup demo
- an abstract art network
- a dark cyber graph toy
- a business dashboard with a graph pasted into the middle

The visual system exists to help the user read structure and meaning quickly.

---

## Design principles

### 1. Clarity over spectacle

Every visual choice should help answer at least one of these questions:
- what kind of actor is this?
- how important is it in this view?
- how is it connected?
- what should I notice first?

If a visual feature does not improve interpretation, remove it.

### 2. Visual hierarchy must be obvious

The user should instantly distinguish between:
- primary selected actor
- local neighborhood
- rest of visible graph
- hidden/de-emphasized context

### 3. Color should encode meaning, not decoration

Node color is for data encoding first.
It should not be used as generic beautification.

### 4. Density must be managed deliberately

The product will eventually support dense graphs.
That means the default state must leave room for:
- zooming
- focusing
- filtering
- reading labels without chaos

### 5. The graph is not the only visual surface

The profile panel, legend, filters, view switcher, and count summaries are all part of the visual system.

---

## Overall aesthetic direction

### Primary tone
- restrained
- information-dense but breathable
- system-map inspired
- neutral enough to let the data carry the meaning

### Style reference direction
Borrow from the spirit of Kumu:
- readable graph canvas
- clear controls
- metadata-rich profiles
- multiple visual lenses

Do not imitate Kumu’s exact visual identity.

---

## Theme strategy

### Version 1 recommendation
- light theme first
- dark theme optional later

### Why light first
- easier for dense analytical reading
- cleaner for profile-heavy inspection
- better default for documents/screenshots
- lower risk of over-stylizing the product

### Core surface layers
- app background
- graph background
- panel background
- card or section surfaces
- overlay surfaces (tooltips, dropdowns, menus)

These should be visually distinct but close enough to feel unified.

---

## Node design system

## 1. Node size

Node size should be driven by the active view or metric.

### Default behavior
- medium neutral base size
- selected node slightly larger or more visually prominent
- size scaling restrained; avoid giant bubbles

### Recommended size drivers
- neutral size in overview
- commercial attractiveness in opportunity view
- degree or other metric in ecosystem view

### Anti-pattern
Do not use extreme size differences that break scanning.

---

## 2. Node color

### Primary rule
Default node color should encode `buyer_type`.

### Suggested semantic mapping
- `private` -> one hue family
- `public` -> one hue family
- `hybrid` -> one hue family
- `growth` -> one hue family
- `channel` -> one hue family
- `unknown` -> neutral grey

### Important rule
These should be defined as semantic tokens, not hard-coded visual decisions buried in components.

Example token names:
- `--color-buyer-private`
- `--color-buyer-public`
- `--color-buyer-hybrid`
- `--color-buyer-growth`
- `--color-buyer-channel`

This allows the same data-driven logic across graph, legend, badges, and profile elements.

---

## 3. Node shape

### Default recommendation
Keep shapes simple.

Version 1 should use:
- one primary node shape
- optional secondary badge/icon for node type

### Why
Too many shapes increase visual noise faster than they increase clarity.

### Optional secondary distinction
If needed, distinguish special node types using:
- outline style
- badge/icon
- subtle shape change only for a small set

Recommended special handling:
- `cluster`
- `ecosystem_actor`
- `business_unit`

But avoid a zoo of shapes.

---

## 4. Node stroke / outline

Node outlines are useful for communicating state.

### Suggested use
- default = thin neutral outline
- hovered = stronger outline
- selected = strongest outline + glow/halo
- filtered match or spotlighted = accent outline

### Important rule
Use outline and opacity changes to show state before resorting to extra decoration.

---

## 5. Node badges

Badges are useful, but only for high-value information.

### Good badge candidates
- node type
- procurement regime
- opportunity band
- confidence level

### Bad badge candidates
- every field
- long text strings
- decorative tag clouds

### Recommended badge behavior
- compact
- consistent placement
- limited count per node/profile header

---

## Edge design system

## 1. Edge color

### Default recommendation
Use a neutral edge color by default.

Why:
- edges are usually secondary to nodes
- strong node color encoding already carries key meaning
- multicolored edges quickly create clutter

### Exception
Edge type can influence style in special views, but should not become the global default.

---

## 2. Edge thickness

### Default recommendation
Moderate base thickness, with variation tied to `strength` only if the difference remains subtle.

### Selected / focused behavior
- selected edge = thicker and clearer
- edges in focused neighborhood = slightly more visible
- unrelated edges = faded

---

## 3. Edge opacity

Opacity is the most important edge control in dense views.

### Default strategy
- overview = medium-low opacity
- focused neighborhood = higher opacity
- weak edges = strongly faded or hidden

### Principle
Edges should rarely compete with nodes for primary visual attention.

---

## 4. Edge direction

If an edge is directional:
- use a subtle arrow cue
- avoid oversized arrowheads

The direction cue should be readable when inspected, but should not dominate the graph.

---

## Label system

## 1. Label priority

Labels should be selective.

### Default visible labels
- selected node
- hovered node
- key/top-ranked nodes when zoomed out if enabled
- nearby nodes in focused mode

### Do not do this
- all labels always visible in dense graphs

---

## 2. Label styling

### Requirements
- high contrast
- clean sans-serif
- compact
- stable baseline and spacing

### Suggested hierarchy
- selected node label strongest
- hovered node label second strongest
- passive labels lighter

### Background treatment
Use subtle label backing or halo only when necessary for legibility.
Avoid heavy pills around every label.

---

## 3. Truncation

Long organization names should truncate gracefully in the graph, not in the profile.

### Recommendation
- graph labels: truncation allowed
- profile header: full label shown
- tooltips: full label shown

---

## Graph state styling

## 1. Default state
- balanced density
- neutral emphasis
- no actor selected

## 2. Hover state
- hovered node brighter/clearer
- first-degree edges emphasized
- unrelated graph slightly faded

## 3. Selected state
- selected node clearly dominant
- local neighborhood visible
- profile panel open
- camera optionally centered

## 4. Focused state
- selected/focused neighborhood prominent
- rest of graph faded but not necessarily removed

## 5. Filtered state
- only matching nodes remain visible
- counts update
- legend remains accurate to current encoding

---

## Profile panel design

The right panel should feel like a structured article, not a form and not a spreadsheet.

### Section styling rules
- generous vertical spacing between sections
- small, clear section headers
- badges near top, not everywhere
- short prose blocks
- compact evidence rows

### Field styling rules
- key-value rows for compact metadata
- chips for likely needs and buying modes
- small scales or labeled indicators for relevance fields
- links visibly clickable but not distracting

### Empty state
The empty panel should feel intentional, not unfinished.

---

## Controls and panel chrome

## 1. Top bar
- quiet, compact, not oversized
- view title readable at a glance
- search visually prominent but not dominant

## 2. Left control rail / drawer
- filters grouped and scannable
- avoid overwhelming first-load control density
- allow section collapse if needed

## 3. Legend
- minimal but accurate
- context-aware to current view
- collapsible

## 4. Menus and dropdowns
- clean and fast to scan
- avoid excessive icons
- prioritize readable text

---

## Motion and transitions

### General rule
Motion should support orientation, not entertainment.

### Good motion
- smooth camera centering
- subtle fade between focus states
- lightweight panel open/close
- metric size transitions that do not jump violently

### Bad motion
- bouncing nodes
- flashy glows
- excessive easing on routine actions
- long animation chains after every filter change

---

## Semantic color token recommendations

Codex should implement a token system, not scattered color literals.

### Core token groups
- surface tokens
- text tokens
- border tokens
- buyer type tokens
- state tokens
- badge tokens
- edge tokens
- focus/selection tokens

### Suggested token examples
- `--surface-app`
- `--surface-canvas`
- `--surface-panel`
- `--text-primary`
- `--text-secondary`
- `--border-default`
- `--color-buyer-private`
- `--color-buyer-public`
- `--color-buyer-hybrid`
- `--color-buyer-growth`
- `--color-buyer-channel`
- `--color-selected`
- `--color-hover`
- `--color-edge-default`
- `--color-edge-muted`

---

## Typography

### Recommended stack
Use a modern system or neutral sans-serif stack.

### Hierarchy
- app title: medium emphasis
- view title: clear but compact
- profile header: strongest text on the screen outside graph labels
- section headers: small uppercase or small bold
- body text: compact but readable
- metadata labels: smaller, lower contrast

### Important rule
Typography should reduce cognitive load, not add brand theater.

---

## Responsive behavior

### Desktop-first
Version 1 should optimize for desktop/laptop analytical use.

### Minimum responsive goals
- control drawer collapses appropriately
- profile panel can overlay on narrower screens
- graph remains primary surface

### Non-goal
Do not optimize deeply for mobile-first use in version 1.

---

## Accessibility and contrast

### Required principles
- color should not be the only signal
- selected state should be visible through more than one cue
- badges should be readable at standard zoom
- panel text should maintain accessible contrast

### Good redundancy channels
- color
- size
- outline
- opacity
- badges
- proximity/focus

---

## Anti-patterns to avoid

### 1. Over-coloring
Too many hues destroy semantic clarity.

### 2. Graph neon aesthetic
Makes the product feel performative instead of analytical.

### 3. Oversized node scale differences
Turns the graph into a bubble chart accident.

### 4. Heavy borders on everything
Creates visual fatigue.

### 5. Excessive badge clutter
Makes nodes unreadable.

### 6. Label spam
Destroys the map faster than almost anything else.

### 7. Dense panel forms
Makes the profile feel administrative instead of interpretive.

---

## Acceptance criteria for visual design

The visual system is good enough when:

1. the user can quickly distinguish actor classes
2. the selected node is unmistakable
3. dense graphs still feel calm rather than frantic
4. the panel feels structured and readable
5. the legend accurately reflects the current encoding
6. the app feels more like an atlas than a graph experiment

---

## Bottom line

The visual system should make the product feel trustworthy, precise, and navigable.

It should help the user think more clearly about the market — not distract them from it.
