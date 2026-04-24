# 01 — Non-goals

## Purpose

This document defines what the market atlas **is not**. Its job is to protect the build from scope creep, category mistakes, and premature platform ambitions.

The product thesis is the source of truth for what the product *is*. This file is the source of truth for what the product must *not become* during version 1.

## Core principle

Version 1 is a **read-only, Kumu-inspired market intelligence viewer** built around actor nodes, relationship edges, rich profiles, saved views, and interactive controls.

It is **not** a general-purpose mapping platform.

## Non-goals for version 1

### 1. Not a Kumu clone

The goal is to borrow from Kumu's useful concepts — elements, connections, profiles, views, controls, and optional metrics — without reproducing Kumu feature-for-feature or recreating its product scope. Kumu's documented architecture explicitly distinguishes between maps, views, fields, profiles, and controls; those are the abstractions worth borrowing, not its full platform surface. citeturn383638search0

**Therefore, version 1 must not include:**
- feature parity with Kumu
- presentation mode or storytelling mode
- collaboration and commenting layers
- workspace management or account administration
- full model-editing or authoring workflows
- complex template ecosystems

### 2. Not a graph editor

The first release is a **viewer**, not an editor.

The graph should be navigable, filterable, searchable, and inspectable — but not editable inside the product UI.

**Therefore, version 1 must not include:**
- drag-to-create nodes
- drag-to-create edges
- edge retyping in the UI
- node field editing in the UI
- inline schema editing
- layout authoring tools beyond basic viewport movement
- multi-user map authoring
- import wizard UIs

Data changes should happen in the canonical JSON source, not in the app.

### 3. Not a CRM or pipeline tool

The product is for **market understanding**, not opportunity management.

**Therefore, version 1 must not include:**
- sales pipelines
- deal stages
- opportunity kanban boards
- activity logging
- follow-up reminders
- task assignment
- revenue forecasting
- CRM sync
- account ownership workflows

It is acceptable to display commercial interpretation fields such as relevance, likely need, or optional scoring overlays. It is not acceptable to turn the product into a sales operating system.

### 4. Not a procurement execution tool

Public-sector and hybrid procurement context matters for understanding market structure, but the product is not meant to run tenders or manage procurement workflows.

**Therefore, version 1 must not include:**
- tender response workflows
- procurement calendar management
- document submission flows
- bid/no-bid workflow automation
- contract lifecycle management
- framework agreement administration
- compliance workflow engines

Procurement information may be represented as profile metadata and filters only.

### 5. Not a BI dashboard

The graph may use counts, simple metrics, and overlays, but the product is not a general analytics dashboard.

**Therefore, version 1 must not include:**
- broad KPI dashboarding
- tabular business intelligence workspace features
- arbitrary chart builders
- report scheduling
- OLAP-style exploration
- data warehouse semantics

The graph is the primary interface. Metrics support the graph; they do not replace it.

### 6. Not a knowledge management suite

The atlas may store concise evidence, source references, and short analyst notes, but it is not a full internal wiki or document system.

**Therefore, version 1 must not include:**
- long-form note-taking environments
- document collaboration
- nested notebook structures
- page builders
- wiki linking semantics beyond graph edges
- rich-text authoring systems

### 7. Not a backend-heavy platform

The first release should be as lightweight as possible.

**Therefore, version 1 must not include unless strictly necessary:**
- custom backend APIs
- authentication and roles
- multi-tenant architecture
- relational database infrastructure
- server-managed editing state
- cloud sync
- background jobs

The preferred v1 architecture is a frontend application that loads and validates a canonical JSON dataset locally.

### 8. Not a custom graph science product

The product may later support graph metrics or overlays, but v1 is not meant to compete with graph-analysis tools.

**Therefore, version 1 must not include:**
- advanced graph querying languages
- full graph-database features
- pathfinding workbenches
- algorithm tuning interfaces
- visual rule builders for graph analysis

Simple, practical metrics that improve navigation are acceptable. Deep network-analysis tooling is not a v1 requirement.

### 9. Not an uncontrolled design playground

Visual polish matters, but novelty is not the goal. The interface should be clear, restrained, and operational.

**Therefore, version 1 must not include:**
- ornamental motion for its own sake
- overly dense always-on labels
- decorative node types with no semantic meaning
- gratuitous 3D effects
- visual metaphors that obscure the data model

The graph should privilege readability over spectacle.

### 10. Not an attempt to solve data collection in the UI

The product should consume structured data, not become the place where all messy market research is performed.

**Therefore, version 1 must not include:**
- source scraping in the UI
- in-app enrichment bots
- browser automation for market research
- embedded research workbenches
- fuzzy merge tools exposed to end users

Those workflows belong upstream of the viewer.

## What is allowed even though it is adjacent

The following are allowed in version 1 because they improve exploration without breaking the product boundary:

- search
- filters
- saved views
- node profile panel
- evidence/source references
- optional scoring overlays
- simple graph metrics for visual emphasis
- geo-oriented view as a separate lens
- JSON import/export support at the file level

These align with the Kumu concepts worth borrowing: profiles, views, controls, metrics, and optional geo representations. citeturn383638search0

## Decision rule for new feature requests

A new feature should be rejected from version 1 if it primarily serves any of these goals:
- editing the model inside the app
- running sales operations
- running procurement operations
- replacing analytics software
- replacing documentation software
- replacing databases or CRMs

When evaluating ambiguous requests, use this test:

> Does this feature make the actor network easier to understand and navigate, or does it start turning the product into an operating system for adjacent work?

If it is the latter, it should be excluded from version 1.

## Why this boundary matters for Codex

Codex is well suited to implementing well-scoped product definitions, structured codebases, and staged feature delivery across isolated work environments. OpenAI’s published Codex materials describe it as a tool for building software in parallel across scoped tasks, not as a reason to leave the product undefined and let the agent improvise the product itself. citeturn383638search0turn383638search1

That means this file is not administrative fluff. It is an implementation constraint.

If this file is ignored, the most likely failure mode is predictable: the project drifts from a readable market atlas into a half-built editor, half-built CRM, and half-built dashboard.

## Bottom line

Version 1 should be judged successful if it becomes a **clear, interactive market atlas**.

It should be judged unsuccessful if it becomes:
- a clone,
- a platform,
- an editor,
- a CRM,
- or a dashboard disguised as a graph.
