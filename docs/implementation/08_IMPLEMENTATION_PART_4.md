# Implementation Guide
## Part 4 — NetworkX Engine, Graph Builder and Feature Pipeline

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 04_GRAPH_MODEL
- 08_IMPLEMENTATION_PART_3

---

# Purpose

This document specifies the complete implementation of the Graph Engine.

Unlike Graph Databases, the graph exists only during computation.

Once the investigation finishes, the graph disappears.

The persistent artifacts are:

- Features
- Explanation Packet
- Metrics

---

# Architecture

```
Canonical Tables

↓

Node Builder

↓

Edge Builder

↓

NetworkX Graph

↓

Feature Pipeline

↓

Timeline

↓

Graph Paths

↓

Explanation Packet
```

---

# Graph Engine

Current implementation

```
Python

↓

NetworkX
```

Future

```
Neo4j

Neptune

TigerGraph
```

No business logic changes.

---

# Graph Object

Implementation

```python
graph = nx.DiGraph()
```

Every node

```
node_id

node_type

properties
```

Every edge

```
edge_type

properties
```

---

# Graph Builder

Input

```
nodes_df

edges_df
```

Output

```
NetworkX Graph
```

Steps

```
Create Graph

↓

Insert Nodes

↓

Insert Edges

↓

Validate

↓

Return
```

---

# Node Loader

Algorithm

```
For each row

↓

Add Node

↓

Attach Properties
```

Duplicate nodes

↓

Ignored.

---

# Edge Loader

Algorithm

```
For each row

↓

Add Edge

↓

Attach Properties
```

Duplicate edges

↓

Ignored.

---

# Graph Validation

Checks

```
Missing Nodes

Broken Edges

Disconnected Root

Invalid Types
```

Failure

↓

Reject Graph.

---

# Subgraph Builder

Purpose

Avoid building the enterprise graph.

Input

```
Root Node

↓

Traversal Depth

↓

Filters
```

Output

```
Investigation Graph
```

---

# Supported Roots

```
Client

Contract

Top Contract

Movement

AML Case

External Account
```

---

# Traversal

Default

```
BFS

Depth = 3
```

AML

```
Depth = 5
```

Future

Configurable.

---

# Filters

Current

```
Time Window

Movement Type

Contract

Client

Top Contract
```

Future

```
Country

Device

Watchlist

UBO
```

---

# Graph Cache

Current

```
Disabled
```

Reason

Graphs are temporary.

Future

Optional cache

↓

Redis.

---

# Feature Pipeline

Pipeline

```
Graph

↓

Structural Features

↓

Financial Features

↓

Relationship Features

↓

Behavior Features

↓

AML Features
```

Output

```
Feature Dictionary
```

---

# Structural Features

```
Node Count

Edge Count

Degree

Density

Connected Components
```

---

# Financial Features

```
Deposits

Withdrawals

Amounts

Ratios

Contracts

Top Contracts
```

---

# Relationship Features

```
Shared Accounts

Institution Count

Relationship Count

Community Size
```

---

# Behavioral Features

Future

```
Login Frequency

Geo Drift

Impossible Travel

Device Sharing
```

---

# AML Features

```
Cash In Out

Repeated Round Trip

Shared Accounts

KYC Mismatch

Top Contract Flow
```

---

# Timeline Builder

Input

```
Graph
```

Algorithm

```
Collect Events

↓

Sort by Timestamp

↓

Generate Timeline
```

Output

```
Ordered Events
```

---

# Path Builder

Purpose

Generate business-readable paths.

Input

```
Graph
```

Output

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

Never output internal IDs.

---

# Serialization

Supported

```
JSON

CSV

PyVis

Explanation Packet
```

Future

```
GraphML

GEXF
```

---

# Memory Management

Graphs remain in memory only.

```
Create

↓

Analyze

↓

Destroy
```

Never persist the graph object.

---

# Performance

Expected graph size

```
100

↓

5000 nodes
```

Execution

```
<5 seconds

Simple Case

<30 seconds

Complex Case
```

---

# Parallel Execution

Allowed

```
Multiple Workers

↓

Independent Graphs
```

Forbidden

```
Shared Graph Objects
```

---

# Metrics

Capture

```
Build Time

Traversal Time

Feature Time

Timeline Time

Serialization Time

Node Count

Edge Count
```

---

# Logging

Graph Builder logs

```
Root Node

Traversal Depth

Nodes Loaded

Edges Loaded

Duration

Failures
```

---

# Error Handling

Recoverable

```
Empty Graph

Missing Optional Nodes
```

Fatal

```
Broken Root

Invalid Canonical Data

Circular Builder Failure
```

---

# Future Optimizations

```
GraphFrames

Rustworkx

cuGraph

Parallel Traversal
```

Only if justified by workload.

---

# Design Rules

Rule 1

Graphs are disposable.

---

Rule 2

Features survive.

Graphs do not.

---

Rule 3

Graph Builder owns graph creation.

---

Rule 4

Traversal never exceeds configured depth.

---

Rule 5

Graph outputs are deterministic.

---

# ADR

ADR-027

Temporary Graph Engine

Decision

Graphs are materialized in memory using NetworkX and discarded after feature extraction.

Reason

Minimizes infrastructure complexity while preserving graph semantics.

---

# End of Part 4

Next

```
08_IMPLEMENTATION_PART_5.md
```

Topics

- REST API
- FastAPI Services
- Workbench
- Streamlit Prototype
- Packet Store
- Observability
- Deployment
- CI/CD
