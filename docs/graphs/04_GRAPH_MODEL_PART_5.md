# docs/graph/04_GRAPH_MODEL_PART_5.md

---
title: Graph Model
subtitle: Part 5 — Graph Algorithms, Feature Engineering and NetworkX Implementation
version: 1.0
status: Draft
owner: AI AML Layer
depends_on:
  - 04_GRAPH_MODEL_PART_1.md
  - 04_GRAPH_MODEL_PART_2.md
  - 04_GRAPH_MODEL_PART_3.md
  - 04_GRAPH_MODEL_PART_4.md
---

# Purpose

This document defines the graph algorithms executed over the canonical graph.

The objective is **not** to calculate graph metrics for research purposes.

The objective is to generate features that improve AML investigations.

---

# Graph Computation Strategy

The graph is never computed globally.

Instead:

```
Canonical Tables

↓

SQL Filtering

↓

Temporary Subgraph

↓

NetworkX

↓

Graph Features

↓

Explanation Packet
```

The graph is disposable.

---

# Why NetworkX

Phase 1 requirements:

- Simple deployment
- Pure Python
- Easy debugging
- Small investigation graphs
- No infrastructure changes

NetworkX satisfies all of them.

Future migration to Neo4j/Neptune should require only replacing the graph computation layer.

---

# Graph Size

Recommended limits

```
Nodes

100 – 5,000
```

```
Edges

200 – 20,000
```

Per investigation.

Never compute enterprise-wide graphs.

---

# Feature Categories

Every feature belongs to one category.

```
Structural

Financial

Behavioral

Temporal

Relationship

AML
```

---

# Structural Features

## Node Count

```
number_of_nodes
```

Purpose

Estimate investigation complexity.

---

## Edge Count

```
number_of_edges
```

Purpose

Estimate relationship density.

---

## Degree

```
in_degree

out_degree

total_degree
```

Useful for

External accounts

Contracts

Clients

---

## Connected Components

Purpose

Separate disconnected investigations.

---

## Graph Density

```
density
```

Useful for

Relationship concentration.

---

# Financial Features

Computed directly from CashMovement nodes.

---

## Deposit Count

```
deposit_count
```

---

## Withdrawal Count

```
withdrawal_count
```

---

## Total Deposits

```
deposit_total
```

---

## Total Withdrawals

```
withdrawal_total
```

---

## Average Deposit

```
avg_deposit
```

---

## Average Withdrawal

```
avg_withdrawal
```

---

## Deposit / Withdrawal Ratio

```
withdrawal_total

/

deposit_total
```

---

## Contract Count

```
contracts_per_client
```

---

## ContractGroup Count

```
contracts_per_top_contract
```

---

# Temporal Features

Built from timestamps.

---

## First Activity

```
first_event
```

---

## Last Activity

```
last_event
```

---

## Active Days

```
days_active
```

---

## Deposit Frequency

```
deposits_per_day
```

---

## Withdrawal Frequency

```
withdrawals_per_day
```

---

## Average Days Between Deposit and Withdrawal

```
avg_days_between
```

---

## Maximum Velocity

```
minimum_time_between_events
```

Future

Login

↓

Deposit

↓

Withdrawal

---

# Relationship Features

---

## External Accounts

```
external_account_count
```

---

## Shared External Accounts

```
shared_external_accounts
```

---

## Financial Institutions

```
institution_count
```

---

## Contracts

```
contract_count
```

---

## Top Contracts

```
top_contract_count
```

---

## KYC Versions

```
kyc_versions
```

---

# AML Features

---

## Cash In / Cash Out

```
cash_in_out_cycles
```

---

## Repeated Round Trip

```
round_trip_cycles
```

---

## Shared External Account

```
shared_account_clients
```

---

## New Destination Account

```
is_new_destination
```

---

## KYC Mismatch

```
expected_vs_observed_ratio
```

---

## Multi Contract Flow

```
contracts_per_top_contract
```

---

# Future Behavioral Features

Future graph

```
Login

↓

Geo

↓

Device
```

Features

```
Impossible Travel

Country Drift

Night Login

Velocity

Shared Device

Shared IP
```

---

# Path Algorithms

NetworkX

```
shortest_path()

all_simple_paths()
```

Used to explain

```
How are these entities connected?
```

---

# Neighborhood

```
neighbors()

successors()

predecessors()
```

Useful for

```
Client

↓

Contracts

↓

Movements

↓

Accounts
```

---

# Traversal Strategy

Breadth First Search

Default

```
depth = 3
```

AML

```
depth = 5
```

Never unlimited.

---

# Centrality

Current

Do NOT use as AML score.

Reason

HouseConcentrationAccount would dominate.

Future

Compute only over

```
Economic Graph
```

excluding

```
HouseConcentrationAccount

Infrastructure Nodes
```

---

# Community Detection

Future.

Candidates

```
Louvain

Leiden

Label Propagation
```

Use Cases

```
Shared External Accounts

Device Communities

Counterparty Communities
```

---

# Cycle Detection

Current

Only operational validation.

Do not use for AML scoring.

Future

Useful for

```
Money Laundering Rings

Round Trips

Corporate Structures
```

---

# Feature Store

Current

Persist

```
Redshift
```

Future

Dedicated Feature Store.

---

# Explanation Features

Every Explanation Packet receives

```
Node Count

Edge Count

Timeline

Graph Paths

Relationship Features

AML Features

Financial Features
```

Never raw graph objects.

---

# NetworkX Functions

Minimum implementation

```python
build_graph()

get_client_subgraph()

get_contract_subgraph()

get_top_contract_subgraph()

get_case_subgraph()

extract_graph_features()

extract_paths()

build_timeline()
```

---

# Performance

Order of execution

```
SQL

↓

Filter

↓

Build Graph

↓

Features

↓

Destroy Graph
```

Never cache investigation graphs.

---

# Migration Strategy

Current

```
NetworkX
```

Future

```
Neo4j

TigerGraph

Neptune

GraphFrames
```

Business model remains unchanged.

Only graph engine changes.

---

# ADR

ADR-009

Graph Algorithms

Decision

Compute graph features dynamically.

Reason

Small investigation graphs.

Simple deployment.

Easy migration.

---

# Deliverables

Every graph execution produces

```
NetworkX Graph

↓

Graph Features

↓

Timeline

↓

Paths

↓

Explanation Packet
```

---

# End of Part 5

Next

```
04_GRAPH_MODEL_PART_6.md
```

Topics

- Graph Visualization
- HTML Rendering
- Investigation UI
- Graph JSON Schema
- Explanation Packet Schema
- Graph API
- Complete End-to-End Example
- GraphDB Migration Guide
