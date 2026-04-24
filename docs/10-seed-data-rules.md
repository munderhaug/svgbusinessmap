# 10 — Seed Data Rules

## Purpose

The seed dataset is not the full Stavanger corridor market.

Its job is to do three things well:
1. prove that the ontology holds up with real actors,
2. give Codex a realistic dataset to build against,
3. surface ambiguity before the graph grows large.

A bad seed dataset makes the UI look clean by hiding real-world mess.
A good seed dataset contains enough variety to stress the model without drowning the build.

---

## Seed dataset objectives

The seed dataset should:
- represent multiple buyer types,
- include both private and public logic,
- include a few ecosystem and cluster actors,
- include at least one institution and one business-unit style exception,
- include enough edges to demonstrate network behavior,
- remain small enough for fast iteration.

---

## Recommended size

### Preferred development seed
- **20–40 nodes**
- **25–80 edges**

### Preferred minimal smoke seed
- **5–8 nodes**
- **5–10 edges**

The larger of these is the main seed dataset.
The smaller is only for smoke testing.

---

## Actor mix requirements

The main seed dataset should include all of the following categories.

### Private organizations
At least 6–10 nodes.
Examples:
- energy or offshore companies
- product companies
- industrial technology firms
- aquaculture technology firms

### Public entities
At least 4–8 nodes.
Examples:
- municipalities
- hospital
- inter-municipal company

### Hybrid entities
At least 1–3 nodes.
Examples:
- utility/infrastructure actors
- publicly owned commercial operators

### Institutions
At least 1–3 nodes.
Examples:
- university
- research institute

### Ecosystem actors / clusters
At least 3–6 nodes.
Examples:
- innovation company
- business association
- cluster
- cross-sector arena

### Business unit exception
At least 1 node.
This is important because the ontology explicitly allows business-unit modeling when it improves truthfulness.

---

## Geography requirements

The seed dataset should reflect the corridor logic, not only central Stavanger.

Include actors across:
- Eigersund / south
- Jæren municipalities
- Sandnes / Sola / Stavanger core
- north-edge relevance when appropriate

At minimum, the seed should cover more than one municipality cluster.

---

## Relationship requirements

The seed dataset must include multiple edge types.

Required edge categories:
- `operates_in`
- at least one of `ownership` or `subsidiary_of`
- at least one of `partner_of`, `collaborates_with`, or `member_of`

This is necessary because a graph viewer tested on one edge type is not really tested.

---

## Profile requirements

Every seed node must include:
- a meaningful summary
- a clear “why it matters” statement
- at least one source ref
- structured relevance metadata
- scores with confidence level

The profile text should not be sales copy.
It should read like market intelligence.

---

## Scoring rules for the seed

The seed dataset may contain **illustrative scores** rather than final, production-grade scoring.

Rules:
- scores must be plausible
- confidence must be explicitly marked
- scores should be treated as testing overlays, not truth claims

This matters because the seed dataset exists to test the UI and ontology, not to finalize the market ranking.

---

## Source rules

The seed should prioritize:
1. official organizational websites
2. official institutional pages
3. official cluster/about pages
4. official public-sector pages

Use secondary sources only if necessary.

Every node and edge in the seed must be explainable by its source refs.

---

## Dataset quality rules

### Required
- no orphan edges
- no duplicate node IDs
- no duplicate edge IDs
- all source refs resolve
- all views resolve to valid field names
- all layout preset references are valid

### Strongly recommended
- node labels readable to a human without opening the profile
- notes concise
- no placeholder lorem ipsum
- no obviously fake actors

---

## What the seed is not allowed to become

The seed dataset must **not** become:
- a fake graph made mostly to look connected,
- a giant partial dump of the corridor,
- a spreadsheet pasted into JSON without relationship logic,
- or a definitive ranking of the market.

---

## Done condition

The seed dataset is ready when:
- Codex can build against it,
- the graph is varied enough to stress the product,
- the ontology feels truthful under real actors,
- and the dataset is small enough to inspect manually.
