# docs/graph/04_GRAPH_MODEL_PART_6.md

---
title: Graph Model
subtitle: Part 6 — Visualization, APIs, Explanation Packets and Future Evolution
version: 1.0
status: Draft
owner: AI AML Layer
depends_on:
  - 04_GRAPH_MODEL_PART_1.md
  - 04_GRAPH_MODEL_PART_2.md
  - 04_GRAPH_MODEL_PART_3.md
  - 04_GRAPH_MODEL_PART_4.md
  - 04_GRAPH_MODEL_PART_5.md
---

# Purpose

This document defines how the graph is exposed to investigators,
how it is rendered,
how it becomes an Explanation Packet,
and how the architecture evolves from NetworkX to a Graph Database without changing business logic.

---

# Visualization Philosophy

The investigator should **never** see the complete graph.

Instead, every visualization is a projection.

```
Canonical Graph

↓

Projection

↓

AML Case

↓

Visualization
```

---

# Visualization Levels

## Level 1

Customer Summary

```
Client

↓

Contracts

↓

KYC

↓

Cases
```

Purpose

Provide immediate context.

---

## Level 2

Money Flow

```
External Account

↓

Deposit

↓

Contract

↓

Withdrawal

↓

External Account
```

Purpose

Explain movement of value.

---

## Level 3

AML Investigation

```
AML Case

↓

Timeline

↓

Evidence

↓

Graph
```

Purpose

Support analyst decisions.

---

## Level 4

Future Behavioral Graph

```
Login

↓

Session

↓

Device

↓

Geo

↓

Withdrawal
```

Purpose

Behavioral AML.

---

# HTML Graph

Every investigation generates one HTML.

Example

```
case_001.html
```

Contains

```
Graph

Timeline

Node Inspector

Edge Inspector

Evidence

AML Features

Explanation
```

---

# Graph Rendering

Recommended Library

```
PyVis
```

Reason

- Interactive
- HTML output
- Lightweight
- Easy integration

Future

```
Graphistry

Cytoscape

D3
```

---

# Node Style

## Client

```
Blue

Large
```

---

## Contract

```
Purple
```

---

## CashMovement

```
Red

Square
```

---

## ExternalBankAccount

```
Orange
```

---

## HouseConcentrationAccount

```
Gray

Transparent

Low emphasis
```

Purpose

Operational only.

---

## AML Case

```
Dark Red

Largest Node
```

---

## AML Typology

```
Violet
```

---

# Edge Style

Ownership

```
Purple
```

Economic

```
Blue
```

AML

```
Red
```

Operational

```
Gray

Dashed
```

Operational edges should visually disappear.

---

# Layout

Preferred

```
Left

↓

Right

Economic Flow
```

Never radial.

Never random.

Money should always flow left → right.

---

# Graph JSON

Every graph must also be exportable.

Schema

```json
{
  "nodes": [],
  "edges": [],
  "metadata": {}
}
```

---

# Node Schema

```json
{
  "id": "Client:123",
  "type": "Client",
  "label": "Client",
  "properties": {}
}
```

---

# Edge Schema

```json
{
  "id": "...",
  "type": "HAS_CONTRACT",
  "source": "...",
  "target": "...",
  "properties": {}
}
```

---

# Metadata

```json
{
  "root_node": "...",
  "projection": "AML_CASE",
  "generated_at": "...",
  "graph_version": "1.0"
}
```

---

# Explanation Packet

Every graph produces one Explanation Packet.

Structure

```json
{
  "summary": "",
  "timeline": [],
  "graph_paths": [],
  "features": {},
  "evidence": [],
  "missing_evidence": [],
  "recommended_action": ""
}
```

---

# Graph Paths

Graph paths are human-readable.

Example

```
External Account

↓

Deposit

↓

Contract

↓

Withdrawal

↓

External Account
```

Not

```
Node IDs
```

---

# Timeline

Timeline always accompanies graph.

Example

```
08:05

Deposit

↓

09:10

Withdrawal

↓

13:00

Deposit

↓

14:50

Withdrawal
```

---

# Evidence

Evidence references graph entities.

Current

```
Cash Movements

Contracts

External Accounts

KYC
```

Future

```
Orders

Executions

Watchlists

Documents

Sessions

Devices
```

---

# Missing Evidence

Explanation Packet should explicitly state missing information.

Examples

```
Ownership of destination account unknown.

No watchlist information.

No login history.

No device information.
```

LLM must never invent evidence.

---

# Graph API

Future REST API.

```
GET

/client/{id}/graph
```

Returns

```
Client Projection
```

---

```
GET

/case/{id}/graph
```

Returns

```
AML Investigation Graph
```

---

```
GET

/contract/{id}/graph
```

Returns

```
Contract Projection
```

---

# NetworkX Boundary

NetworkX responsibilities

```
Build Graph

Compute Paths

Extract Features

Generate Timeline
```

NetworkX does NOT

```
Call LLM

Store Graph

Decide AML Cases
```

---

# LLM Boundary

LLM receives

```
Explanation Packet
```

LLM does NOT

```
Read SQL

Read Redshift

Traverse Graph

Discover Nodes
```

LLM only explains.

---

# Graph Persistence

Current

```
None
```

Graph is temporary.

Future

```
Neo4j

Neptune
```

No changes to

```
Business Model

Explanation Packet

Triggers

AML Cases
```

Only graph computation changes.

---

# Migration Strategy

Current

```
SQL

↓

NetworkX

↓

Packet
```

Future

```
SQL

↓

Neo4j

↓

Cypher

↓

Packet
```

Packet remains identical.

---

# End-to-End Example

```
Deposit arrives

↓

Trigger Engine

↓

SQL Template

↓

Subgraph

↓

NetworkX

↓

Feature Extraction

↓

Timeline

↓

Explanation Packet

↓

LLM

↓

Workbench

↓

Analyst

↓

Feedback
```

---

# Success Criteria

A graph implementation is correct if

- Graph is reproducible.
- Temporary graph can be rebuilt.
- Explanation Packet contains complete evidence.
- No infrastructure nodes influence AML.
- HouseConcentrationAccount never appears as counterparty.
- Economic flow remains deterministic.
- Timeline is preserved.
- Graph paths are reproducible.
- Visualization is readable.

---

# ADR

ADR-010

Visualization and Explanation Boundary

Decision

Graphs are generated for humans.

LLMs explain packets.

NetworkX computes.

Storage remains relational.

---

# Graph Model Completion

The Graph Model specification is now complete.

Documents

```
04_GRAPH_MODEL_PART_1

04_GRAPH_MODEL_PART_2

04_GRAPH_MODEL_PART_3

04_GRAPH_MODEL_PART_4

04_GRAPH_MODEL_PART_5

04_GRAPH_MODEL_PART_6
```

Together these define:

- Business Graph
- Node Taxonomy
- Edge Taxonomy
- Temporal Model
- AML Projections
- Graph Algorithms
- NetworkX Execution Model
- Visualization
- APIs
- Explanation Packets
- Migration Strategy

---

# Next Document

```
docs/aml/05_AML_CASES_PART_1.md
```

Topics

- AML Detection Philosophy
- Typology Taxonomy
- Case Lifecycle
- Rule Engine
- Trigger Matrix
- Severity Model
- Case State Machine
