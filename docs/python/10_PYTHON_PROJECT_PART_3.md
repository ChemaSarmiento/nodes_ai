# Python Project
## Part 3 — Graph Engine, Feature Engine and Packet Builder

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Graph Model
- Trigger Engine
- Python Project Part 2

---

# Purpose

This document defines the implementation of the computational core of the AI AML Layer.

This is where canonical entities become temporary investigation graphs.

The graph exists only during execution.

---

# Execution Flow

```
Canonical Objects

↓

Graph Builder

↓

NetworkX

↓

Feature Engine

↓

Timeline Builder

↓

Path Builder

↓

Packet Builder

↓

Explanation Packet
```

---

# GraphBuilder

The GraphBuilder is responsible for constructing a temporary graph from canonical entities.

It does not know SQL.

It does not know LLMs.

It only understands

```
Nodes

Edges
```

---

# GraphBuilder Interface

```python
class GraphBuilder:

    def build(
        self,
        nodes,
        edges
    ) -> nx.DiGraph

    def validate()

    def destroy()
```

---

# Graph Construction

Algorithm

```
Create Graph

↓

Load Nodes

↓

Load Edges

↓

Validate

↓

Return
```

Graph is immutable after construction.

---

# Node Loading

Each node contains

```
node_id

node_type

properties
```

Implementation

```python
graph.add_node(
    node_id,
    **properties
)
```

---

# Edge Loading

Each edge contains

```
edge_type

properties

timestamps
```

Implementation

```python
graph.add_edge(
    source,
    target,
    **properties
)
```

---

# Validation

Required

```
Root Exists

No Broken Edges

No Invalid Node Types

No Duplicate IDs
```

If validation fails

↓

Raise

```
GraphValidationError
```

---

# Graph Traversal

Default

```
Breadth First Search
```

Reason

Better investigation context.

Maximum depth

```
5
```

Configurable.

---

# Graph Filters

Supported

```
Client

Contract

Top Contract

Case

Time Window
```

Future

```
Country

Device

Watchlist

UBO
```

---

# TimelineBuilder

Purpose

Produce chronological business events.

Input

```
Graph
```

Output

```python
TimelineEvent[]
```

Sorted by

```
business_timestamp
```

---

# Timeline Schema

```python
TimelineEvent

timestamp

event_type

entity

amount

description
```

---

# PathBuilder

Purpose

Generate business-readable paths.

Input

```
Graph
```

Output

```python
GraphPath[]
```

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

---

# Path Ranking

Current

Shortest path.

Future

```
Most Relevant

Highest Risk

Highest Amount
```

---

# Feature Engine

Purpose

Compute reusable investigation features.

Input

```
Graph
```

Output

```
Feature Dictionary
```

---

# Feature Pipeline

```
Graph

↓

Structural

↓

Financial

↓

Relationship

↓

Behavior

↓

AML

↓

Dictionary
```

---

# Structural Extractor

Returns

```
Node Count

Edge Count

Density

Degree

Connected Components
```

---

# Financial Extractor

Returns

```
Deposits

Withdrawals

Ratios

Contracts

Top Contracts
```

---

# Relationship Extractor

Returns

```
Shared Accounts

Institution Count

Relationship Count
```

---

# Behavioral Extractor

Future

Returns

```
Login Count

Geo Drift

Impossible Travel

Device Count
```

---

# AML Extractor

Returns

```
Cash In Out

Repeated Round Trip

Shared Accounts

KYC Mismatch

Top Contract Flow
```

---

# Feature Object

```python
FeatureSet

financial

relationship

behavior

aml

structural
```

All serializable.

---

# PacketBuilder

Purpose

Transform graph outputs into Explanation Packets.

Input

```
Features

Timeline

Graph Paths

Trigger Context
```

Output

```
ExplanationPacket
```

---

# Packet Schema

```python
ExplanationPacket

metadata

case

timeline

graph

features

evidence

missing_evidence

recommendation
```

Builder does not call the LLM.

---

# Packet Serialization

Supported

```
JSON

Parquet

S3

Redshift
```

Future

```
Avro
```

---

# Packet Validation

Checks

```
Required Sections

Null Values

JSON Schema

Feature Completeness

Timeline Completeness
```

Reject invalid packets.

---

# Memory Lifecycle

```
Canonical Objects

↓

Graph

↓

Features

↓

Packet

↓

Destroy Graph
```

Graph memory is released immediately after packet generation.

---

# Serialization Strategy

Persist

```
Packets

Features

Metrics
```

Never persist

```
NetworkX Objects
```

---

# Performance Targets

Graph Build

```
<2 sec
```

Feature Extraction

```
<1 sec
```

Timeline

```
<500 ms
```

Packet

```
<500 ms
```

Total

```
<5 sec
```

excluding LLM.

---

# Design Rules

Rule 1

GraphBuilder owns graph creation.

---

Rule 2

FeatureEngine owns calculations.

---

Rule 3

PacketBuilder owns serialization.

---

Rule 4

Graph objects never survive execution.

---

Rule 5

Only ExplanationPackets are shared with downstream services.

---

# ADR

ADR-036

Graph Processing Pipeline

Decision

Separate graph construction, feature extraction and packet generation into independent components.

Reason

Improves modularity, testability and allows replacing any stage without affecting the others.

---

# End of Part 3

Next

```
10_PYTHON_PROJECT_PART_4.md
```

Topics

- FastAPI implementation
- Trigger Worker implementation
- Queue Consumer
- Dependency Injection
- Configuration
- Logging
- Observability
- Error Handling
