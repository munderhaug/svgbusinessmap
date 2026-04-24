# DECISIONS

## Locked decisions

### Product shape
1. **Viewer first, not editor first.**
2. **Read-only v1.** No in-app authoring, editing, or ontology management.
3. **Market atlas, not CRM.** No sales-pipeline workflow or next-action infrastructure in v1.
4. **Actor-centric graph.** Organizations and related institutional actors are the primary nodes.
5. **Demand stays as metadata in v1.** Demand, relevance, and likely buying modes are node/profile fields, not primary graph objects.

### Data model
6. **Canonical JSON is the source of truth.** UI state and rendering must derive from the JSON model.
7. **Schema-first implementation.** All runtime data must validate against `schema/market-map.schema.json`.
8. **Typed views.** The app must support multiple saved views over a single dataset rather than multiple ad hoc map definitions.

### UX and information architecture
9. **Profile-first intelligence.** Rich company and actor information should live in the profile panel rather than cluttering the canvas.
10. **Geography is secondary.** Geo is a separate lens, not the default truth of the market.
11. **Color encodes buyer type by default.** Size is a metric-driven overlay, not a fixed meaning.
12. **Sparse labels by default.** Dense label rendering is only appropriate at higher zoom or in focused contexts.

### Technical architecture
13. **Frontend-only v1.** No database, API, or authentication unless explicitly added later.
14. **React + TypeScript + Vite** for the application shell.
15. **Sigma.js + graphology** for graph rendering and graph data handling.
16. **Zustand** for client UI state.
17. **Zod + JSON Schema** for runtime contract enforcement.
18. **Vitest + React Testing Library + Playwright** for unit, integration, and end-to-end testing.

## Still intentionally open
- Whether geo rendering ships in the first public UI or only as a prepared dataset/view
- Whether business-unit nodes appear in the first production seed beyond the current examples
- Whether profile-side scoring appears by default or under an advanced section
- How much view state is mirrored into the URL in the first release
