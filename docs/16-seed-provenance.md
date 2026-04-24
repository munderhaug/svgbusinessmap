# Seed dataset provenance

## Purpose
The seed datasets exist to help Codex and future developers build the first working version of the market atlas against a realistic structure.

They are intended to test:
- schema validation
- graph rendering
- filter behavior
- view switching
- profile panel rendering
- neighborhood highlighting

They are **not** intended to be treated as the final production market database.

## What is factual vs illustrative
### Likely factual or grounded fields
These fields are intended to align with real-world organizations and plausible classifications:
- actor labels
- broad actor types
- broad buyer-type categories
- broad municipality and zone placement
- high-level sector and subsector labels
- some relationship types

### Illustrative or provisional fields
These fields should be treated as model- or demo-oriented until production research populates them more rigorously:
- profile summaries
- why-it-matters text
- likely sponsor roles
- likely needs
- likely buying modes
- scores
- tags like `tier_1`, `opportunity_high`, or similar
- some relationship strengths

## Why this matters
If Codex assumes the seed dataset is production-clean, it may:
- hardcode assumptions into the UI
- overfit layout or filters to a toy dataset
- treat provisional fields as authoritative business truth

The correct mental model is:
- the **schema** is authoritative
- the **seed data** is representative
- later data-population work will replace or expand the seed content

## Practical implementation rule
Build the UI so that:
- missing fields degrade gracefully
- placeholder scores do not break styling or sorting
- profile sections can hide when data is absent
- filters remain robust with incomplete or evolving metadata
