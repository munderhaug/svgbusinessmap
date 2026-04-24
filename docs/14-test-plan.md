# 14 — Test Plan

## Purpose

This document turns the acceptance criteria into concrete test work for Codex.

The goal is not to maximize test volume. The goal is to protect the parts of the product that are easiest to break and hardest to notice:
- schema integrity,
- graph rendering,
- view logic,
- filter logic,
- focus logic,
- profile correctness,
- and basic performance under realistic graph density.

---

## Test philosophy

Version 1 should use three layers of testing:

1. **unit tests** for parsing, normalization, filtering, view application, and utility logic
2. **integration tests** for component interaction and graph-driven UI behavior
3. **end-to-end tests** for the actual user journeys that matter in the atlas

A graph product can appear to work while thinking badly. The tests must therefore verify logic, not just rendering.

---

## Test pyramid

### Unit tests
Use for:
- schema parsing
- reference validation
- field coercion failures
- filter application
- view rule application
- style rule resolution
- node/edge indexing
- neighborhood calculation
- metric selector logic
- URL state parsing and serialization

### Integration tests
Use for:
- app boot with valid dataset
- app failure with invalid dataset
- filter controls updating visible graph state
- view changes updating filter + styling state
- selection updating profile panel
- neighborhood highlighting
- weak-edge hiding
- legend updates
- empty-state rendering

### End-to-end tests
Use for:
- open app
- load seed data
- switch views
- search a node
- select a node
- inspect profile
- apply multiple filters
- clear filters
- enable focus mode
- change sizing metric
- reload and preserve URL state

---

## Test environments

### Local development
- Vite dev server
- small seed dataset
- medium stress-test dataset

### CI environment
- headless browser for Playwright
- deterministic fixture datasets
- schema validation as a required check

---

## Fixture datasets

Codex should create and maintain at least four fixtures.

### 1. `seed.market-map.min.json`
Purpose:
- smallest valid graph
- smoke tests

Recommended size:
- 5–8 nodes
- 5–10 edges

### 2. `seed.market-map.json`
Purpose:
- realistic exploratory development
- UI interaction tests

Recommended size:
- 20–40 nodes
- 25–80 edges

### 3. `stress.market-map.json`
Purpose:
- performance and dense-view behavior

Recommended size:
- 250–500 nodes
- 600–2500 edges

### 4. `invalid.market-map.json`
Purpose:
- error handling and failure-path tests

Recommended contents:
- missing required fields
- broken source refs
- orphan edges
- invalid enum values
- invalid layout preset references

---

## Unit test catalog

### Schema and parsing
- should parse a valid dataset
- should fail on invalid enum values
- should fail when required fields are missing
- should fail when an edge references a non-existent node
- should fail when a node references a non-existent source ref
- should preserve canonical field values without mutation

### Indexing and normalization
- should build a node index by ID
- should build an edge index by ID
- should group nodes by type
- should group nodes by buyer type
- should resolve source refs for node profiles
- should resolve layout preset references

### Filter logic
- should apply AND logic across active filters
- should apply `eq` correctly
- should apply `in` correctly
- should apply `contains` correctly for arrays
- should apply numeric range filters correctly
- should return zero matches without crashing

### View logic
- should merge view defaults with user state correctly
- should apply node filters from the selected view
- should apply edge filters from the selected view
- should respect weak-edge threshold
- should respect label mode rules

### Graph logic
- should return first-degree neighbors correctly
- should return second-degree neighbors correctly
- should not duplicate neighbors in focused mode
- should preserve graph connectivity state when filters change

### URL state
- should serialize selected view to URL
- should serialize filter state to URL
- should restore state from URL on load
- should ignore malformed URL state safely

---

## Integration test catalog

### App boot
- app renders with valid seed data
- app shows readable error state with invalid data
- app shows loading state before graph initialization

### View switching
- changing view updates legend
- changing view updates node color mapping
- changing view updates edge visibility logic
- changing view resets incompatible control state when required

### Search and selection
- typing a valid node label returns the node
- selecting a search result focuses the graph on that node
- selecting a node opens the profile panel
- closing the profile panel preserves graph state

### Filters
- buyer type filter reduces node set correctly
- municipality filter reduces node set correctly
- sector filter reduces node set correctly
- multiple filters combine with AND logic
- clearing filters restores full active view

### Focus tools
- one-hop focus highlights immediate neighbors
- two-hop focus expands the visible neighborhood correctly
- resetting focus restores the current view

### Edge controls
- hiding weak edges changes graph rendering but not node availability
- focused-only edge mode suppresses unrelated edges

### Profile panel
- profile panel displays summary block
- profile panel displays demand fields
- profile panel displays source list
- profile panel handles missing optional fields gracefully

---

## End-to-end journeys

### Journey 1 — Basic orientation
1. Open the app
2. Default overview view loads
3. Legend is visible
4. Graph is interactive
5. A node can be selected
6. Profile panel opens with correct content

### Journey 2 — Private buyer exploration
1. Open app
2. Switch to `private-buyers`
3. Filter by municipality = Stavanger
4. Search for a named organization
5. Select it
6. Verify profile content and visible neighborhood

### Journey 3 — Public + hybrid exploration
1. Switch to `public-and-hybrid`
2. Filter by procurement regime
3. Toggle weak-edge visibility
4. Select a public node
5. Verify profile fields and connected ecosystem actors

### Journey 4 — AI relevance exploration
1. Switch to `ai-relevant`
2. Set node size metric to short-term opportunity
3. Use focus mode on a selected node
4. Verify visible neighbors and profile details

### Journey 5 — URL persistence
1. Apply view + filters + selection
2. Refresh page
3. Verify state is restored

---

## Performance checks

Version 1 does not need enterprise-scale benchmarking, but it does need practical guardrails.

### Minimum checks
- initial graph load with seed dataset should feel immediate
- initial graph load with 250+ nodes should remain usable
- pan/zoom should remain fluid under normal view density
- focus mode should not visibly freeze on realistic neighborhood size
- switching views should remain responsive

### Manual QA checks
- labels should not become unreadable noise at default zoom
- dense views should not render all labels by default
- profile opening should feel instant after node selection
- control panel interactions should not reinitialize the whole graph unnecessarily

---

## Accessibility checks

Even though this is a graph-heavy product, basic accessibility still matters.

Minimum checks:
- all controls keyboard reachable
- visible focus states on controls
- search input accessible by keyboard only
- side panel readable in a screen-zoom scenario
- contrast on text, legend, and badges acceptable
- non-pointer alternative for selecting a node from search results

---

## Regression priorities

The highest regression risks are:
1. schema drift
2. filter logic drift
3. view rule drift
4. profile/source mismatch
5. broken focus logic
6. broken URL state restoration

Any release candidate should always run tests covering these first.

---

## CI gates

The build should fail if any of the following are true:
- canonical seed data does not validate
- type checking fails
- unit tests fail
- integration tests fail
- end-to-end smoke path fails

Optional but recommended:
- lint warnings treated as errors in CI

---

## Done condition for the test plan

The test plan is complete enough for Codex when:
- every acceptance criterion maps to at least one test,
- the seed dataset is testable,
- the invalid dataset has defined failure behavior,
- and the main exploratory journeys are covered end-to-end.
