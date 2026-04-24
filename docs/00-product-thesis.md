# Product Thesis

## Working title
Stavanger Corridor Market Atlas

## Product definition
A Kumu-inspired, read-only market intelligence viewer for exploring relevant actors in the Stavanger corridor market, their relationships, and the information that matters for strategic business development.

## Core purpose
Give Martin a complete, navigable overview of the relevant market from Egersund to Rennesøy so he can understand:
- who the important actors are,
- how they relate to each other,
- what type of organizations they are,
- where they sit in the corridor,
- what kinds of demand or relevance they have for Olavstoppen/Bouvet,
- and where patterns, clusters, and whitespace exist.

## Primary user
Head of business development and sales in Olavstoppen.

## Primary use case
Explore a live, filterable actor map to answer questions such as:
- Which actors matter most in this market?
- How are companies, public entities, hybrid actors, clusters, and institutions connected?
- Which actors are most relevant for design, innovation, AI, software, or product-development demand?
- Where are the strongest concentrations of opportunity?
- Which actors are structurally central even if they are not obvious sales targets?

## Product category
This product is a market atlas, not an operational sales tool.

## Product thesis statement
If we model the Stavanger corridor market as a network of actors with rich profiles, structured relationships, and multiple navigable views, then Martin can reason about the market faster and more accurately than with spreadsheets, directories, or static slides.

## Design principle
The map should optimize for clarity of reasoning, not visual spectacle.

## Version 1 scope
Version 1 is a read-only viewer built around:
- actor nodes,
- relationship edges,
- rich profile panels,
- filters,
- search,
- saved views,
- and lightweight scoring or relevance overlays.

## Primary graph model
- Actors are the primary nodes.
- Relationships between actors are the primary edges.
- Demand, relevance, buying logic, and commercial interpretation are primarily node metadata in version 1.
- Demand is not a first-class graph layer by default in version 1.

## Why this should be inspired by Kumu
The product should borrow the strongest parts of Kumu's model:
- elements and connections as the base structure,
- profiles as the main metadata surface,
- views as alternative lenses on the same underlying map,
- controls for search, filtering, focus, and exploration,
- metrics as optional overlays,
- and geo as a complementary view rather than the main organizing logic.

## Strategic thesis
The value of the product is not just storing information about companies.
The value comes from making structure visible:
- central actors,
- clusters,
- ecosystem relationships,
- public/private/hybrid differences,
- and actor-level relevance to Olavstoppen/Bouvet.

## What success looks like
The product is successful when Martin can:
1. open the map and immediately orient himself,
2. move from overview to specific actor quickly,
3. understand why an actor matters without leaving the interface,
4. switch between multiple useful views of the same market,
5. and identify patterns or strategic opportunities that would be hard to see in tabular form.

## Data philosophy
The source of truth should be a canonical JSON model owned by the project, not by a vendor UI.
The interface should render that model cleanly and consistently.

## Build philosophy
Do not build a Kumu clone.
Build a Kumu-inspired market atlas that is narrower, clearer, and better aligned to this exact use case.
