# docs/graph/04_GRAPH_MODEL_PART_3.md

---
title: Graph Model
subtitle: Part 3 — AML Projections, NetworkX Model and Feature Engineering
version: 1.0
status: Draft
owner: AI AML Layer
depends_on:
  - 04_GRAPH_MODEL_PART_1.md
  - 04_GRAPH_MODEL_PART_2.md
---

# Purpose

This document defines how the logical graph is projected into AML investigations.

Unlike a traditional graph database, the AI AML Layer computes **temporary investigation graphs** using NetworkX.

The graph is not persisted.

The graph is reconstructed for every investigation.

---

# Graph-as-Compute

The graph is **not** the storage layer.

```
Redshift

Athena

S3

↓

SQL Templates

↓

NetworkX

↓

Subgraph

↓

Features

↓

Explanation Packet
```

The graph exists only during computation.

---

# Investigation Graph

Every investigation starts from one anchor.

Possible anchors:

```
Client

Contract

TopContract

CashMovement

ExternalBankAccount

AML Case
```

Everything else is discovered by traversal.

---

# Projection Types

The graph supports multiple projections.

---

## Client Projection

Question

```
Show me everything about this client.
```

Root

```
Client
```

Includes

```
Contracts

KYC

Cash Movements

External Accounts

Cases
```

---

## Contract Projection

Question

```
Show me everything related to this contract.
```

Root

```
Contract
```

Includes

```
Deposits

Withdrawals

Top Contract

External Accounts

Cases
```

---

## Contract Group Projection

Question

```
Show me the complete customer relationship.
```

Root

```
ContractGroup
```

Includes

```
All contracts

All movements

All external accounts

AML Cases
```

---

## Cash Flow Projection

Question

```
Explain this movement.
```

Root

```
CashMovement
```

Shows

```
Origin

Destination

Contract

Client
```

---

## AML Case Projection

Question

```
Why does this AML case exist?
```

Root

```
AMLCase
```

Includes

```
Client

Contracts

Cash Movements

Accounts

Typology

Timeline
```

---

# Traversal Rules

Maximum default depth

```
3
```

Example

```
AML Case

↓

CashMovement

↓

Contract

↓

Client
```

---

For investigations

Maximum

```
5
```

Never traverse the complete graph.

---

# NetworkX Model

Graph type

```python
nx.DiGraph()
```

Reason

Relationships have direction.

Money flows.

Ownership flows.

Investigations flow.

---

# Node Schema

```python
{
    "id": "...",
    "type": "...",
    "business_key": "...",
    "properties": {},
}
```

---

# Edge Schema

```python
{
    "id": "...",
    "type": "...",
    "amount": ...,
    "operation_date": ...,
    "properties": {}
}
```

---

# Graph Construction

Pseudo flow

```
SQL

↓

Nodes DataFrame

↓

Edges DataFrame

↓

NetworkX

↓

Subgraph

↓

Feature Extraction
```

---

# Subgraph Builder

Input

```
anchor_node

depth

filters
```

Output

```
NetworkX DiGraph
```

---

# Graph Filters

Supported

```
Time Window

Movement Type

Contract

Top Contract

Client

AML Case
```

Future

```
Country

Device

Watchlist

UBO
```

---

# Excluded Nodes

These nodes never participate in AML reasoning.

```
HouseConcentrationAccount
```

Future

```
Infrastructure

Internal technical nodes
```

Reason

Infrastructure is not economic ownership.

---

# Feature Engineering

Features are computed from the subgraph.

Never from raw SQL.

---

## Structural Features

```
node_count

edge_count

connected_components

density
```

---

## Financial Features

```
deposit_total

withdrawal_total

deposit_count

withdrawal_count

average_deposit

average_withdrawal
```

---

## Relationship Features

```
external_accounts

shared_accounts

contract_count

top_contract_count

institution_count
```

---

## Behavioral Features (future)

```
device_count

geo_count

login_frequency

velocity
```

---

## AML Features

```
cash_in_out_cycles

round_trip_cycles

new_destination_accounts

shared_external_accounts

kyc_bucket_mismatch

multi_contract_activity
```

---

# Timeline

Every graph generates a timeline.

Example

```
2026-07-01

Deposit

↓

2026-07-03

Withdrawal

↓

2026-07-08

Deposit

↓

2026-07-10

Withdrawal
```

Timeline becomes part of the Explanation Packet.

---

# Graph Paths

Important output.

Example

```
ExternalBankAccount

↓

Deposit

↓

Contract

↓

Withdrawal

↓

ExternalBankAccount
```

Saved as

```json
[
    "ExternalAccount",
    "Deposit",
    "Contract",
    "Withdrawal",
    "ExternalAccount"
]
```

---

# AML Projection

Every AML Case stores

```
Related Nodes

Related Edges

Timeline

Features

Graph Paths
```

Never raw SQL.

---

# Explanation Packet

Generated from

```
Graph

+

Features

+

Timeline
```

Never directly from tables.

---

# Graph Visualization

Library

```
PyVis
```

Future

```
Cytoscape

Graphistry

D3
```

---

# Node Colors

Client

Blue

Contract

Purple

CashMovement

Red

ExternalBankAccount

Orange

AMLCase

Dark Red

AMLTypology

Violet

HouseConcentrationAccount

Light Gray

---

# Edge Colors

Economic

Blue

Operational

Gray

AML

Red

Ownership

Purple

Reference

Green

---

# Graph Layout

Preferred

```
Hierarchical

Economic Flow

Left

↓

Right
```

AML Cases

Centered.

Infrastructure

Bottom.

---

# Performance Rules

Never build the full graph.

Always

```
Filter

↓

Build

↓

Analyze

↓

Destroy
```

NetworkX graphs are temporary.

---

# Migration Strategy

Current

```
SQL

↓

NetworkX
```

Future

```
Neo4j

↓

Cypher
```

No business model changes.

Only execution changes.

---

# ADR

ADR-007

Temporary Investigation Graphs

Decision

Graphs are ephemeral.

Reason

Lower cost.

Simple deployment.

Reuse Redshift.

Future compatible with GraphDB.

---

# End of Part 3

Next

```
04_GRAPH_MODEL_PART_4.md
```

Topics

- AML Case Graphs
- Cash In Out
- Repeated Round Trip
- Shared External Account
- KYC Mismatch
- New Destination Account
- Future UBO Graph
- Future Fraud Graph
- Complete investigation examples
