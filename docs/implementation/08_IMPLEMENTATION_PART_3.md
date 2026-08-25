# Implementation Guide
## Part 3 — Canonical Entity Builder, Graph Materialization and Versioning

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 03_DATA_MODEL
- 04_GRAPH_MODEL
- 08_IMPLEMENTATION_PART_2

---

# Purpose

This document defines how canonical business entities become graph entities.

This is the bridge between relational storage and graph computation.

No Graph Database is required.

---

# Architecture

```
Gold Tables

↓

Canonical Builders

↓

Nodes

Edges

Events

↓

Graph Builder

↓

NetworkX

↓

Features
```

---

# Canonical Builders

Current builders

```
Client Builder

KYC Builder

Contract Builder

ContractGroup Builder

CashMovement Builder

ExternalAccount Builder

Institution Builder

Case Builder
```

Each builder has only one responsibility.

---

# Entity Builder Pattern

Each builder receives

```
DataFrame

↓

Validation

↓

Normalization

↓

Canonical Entity
```

Never reads other builders.

---

# Client Builder

Input

```
gold_clients
```

Output

```
Client Nodes
```

Business Key

```
client_id
```

Node ID

```
Client:{client_id}
```

---

# KYC Builder

Input

```
gold_kyc
```

Output

```
KYCProfile Nodes
```

Node ID

```
KYCProfile:{hash}
```

Hash

```
client

review_date

profile_version
```

---

# Contract Builder

Input

```
gold_contracts
```

Output

```
Contract Nodes
```

Node ID

```
Contract:{contract_id}
```

---

# ContractGroup Builder

Input

```
top_contract
```

Output

```
ContractGroup
```

Purpose

Aggregate contracts.

---

# CashMovement Builder

Input

```
Deposits

Withdrawals
```

Output

```
CashMovement Nodes
```

Movement ID

Stable SHA256

Generated from

```
Movement Type

Client

Contract

Amount

Timestamp

Accounts
```

---

# External Account Builder

Input

```
ordering_account

beneficiary_account
```

Output

```
ExternalBankAccount
```

Never duplicates nodes.

---

# House Account Builder

Input

```
GBM Concentration Accounts
```

Output

```
HouseConcentrationAccount
```

Properties

```
exclude_from_reasoning

exclude_from_centrality

exclude_from_shared_account_detection
```

---

# Institution Builder

Input

```
payer_institution

beneficiary_institution
```

Output

```
FinancialInstitution
```

---

# Case Builder

Input

```
AML Detection Results
```

Output

```
AMLCase Nodes
```

Cases never become source data.

---

# Edge Builder

Edge Builder receives

```
Canonical Entities
```

Creates

```
Ownership

Economic

Operational

AML
```

relationships.

---

# Edge Rules

No duplicate edges.

Every edge has

```
edge_id

edge_type

from

to

observed_at
```

---

# Stable Hash IDs

Every node uses deterministic IDs.

Algorithm

```
SHA256

↓

Business Key

↓

Canonical ID
```

Example

```
Client:123

↓

SHA not required

Business key already unique
```

CashMovement

Requires generated hash.

---

# Temporal Validity

Every canonical entity contains

```
valid_from

valid_to

observed_at
```

Current

```
Append Only
```

Future

```
Slowly Changing Dimensions
```

---

# Versioning

Entities

```
entity_version
```

Builders

```
builder_version
```

Canonical Model

```
canonical_version
```

Graph

```
graph_version
```

---

# Canonical Events

Every entity creation emits

```
ENTITY_CREATED
```

Every update

```
ENTITY_UPDATED
```

Every deletion

```
ENTITY_DELETED
```

These events activate triggers.

---

# Graph Materialization

Current

```
nodes.csv

edges.csv

↓

NetworkX
```

Future

```
Neo4j

↓

Cypher
```

Business model unchanged.

---

# Materialization Pipeline

```
Canonical Tables

↓

Node Builder

↓

Edge Builder

↓

CSV / DataFrame

↓

NetworkX

↓

Subgraph
```

No graph persistence.

---

# Builder Validation

Each builder validates

```
Business Key

Required Fields

Null Keys

Duplicates

Schema
```

Reject invalid entities.

---

# Builder Metrics

Track

```
Nodes Created

Edges Created

Duplicates Removed

Rejected Rows

Duration
```

---

# Recovery

Builders are

```
Idempotent
```

Running twice

↓

Same graph.

---

# Design Rules

Rule 1

One Builder

↓

One Entity

---

Rule 2

Builders never call each other.

---

Rule 3

Graph IDs are deterministic.

---

Rule 4

Canonical entities are immutable.

---

Rule 5

Graph materialization is temporary.

---

# ADR

ADR-026

Canonical Entity Builders

Decision

Every canonical business entity has its own dedicated builder before graph construction.

Reason

Improves modularity, testing and migration to future graph engines.

---

# End of Part 3

Next

```
08_IMPLEMENTATION_PART_4.md
```

Topics

- NetworkX Implementation
- Graph Builder
- Subgraph Builder
- Graph Cache
- Feature Extraction Pipeline
- Performance
- Memory Management
- Graph Serialization
