# Implementation Guide
## Part 2 — Canonical Data Pipeline, ETL and Data Contracts

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 03_DATA_MODEL
- 04_GRAPH_MODEL
- 06_TRIGGER_ENGINE

---

# Purpose

This document defines how data enters the AI AML Layer.

Unlike traditional ETLs, this pipeline does not transform data directly into reports.

Instead, it transforms operational systems into canonical business entities.

---

# Canonical Pipeline

```
Operational Systems

↓

Landing

↓

Bronze

↓

Silver

↓

Gold

↓

Canonical Model

↓

Trigger Engine

↓

Graph

↓

Explanation
```

---

# Data Sources

Current

```
Clients

KYC

Contracts

Deposits

Withdrawals
```

Future

```
Orders

Executions

Funds

Positions

Watchlists

Login

Geo

Device

Sessions
```

---

# Bronze Layer

Purpose

Raw immutable storage.

Characteristics

```
No transformations

Append Only

Historical

Auditable
```

Format

```
Parquet
```

Storage

```
S3
```

---

# Silver Layer

Purpose

Normalize source systems.

Operations

```
Type conversion

Null handling

Deduplication

Column normalization

Business keys
```

Output

Standardized datasets.

---

# Gold Layer

Purpose

Business-ready datasets.

Examples

```
gold_clients

gold_contracts

gold_cash_movements

gold_kyc

gold_cases
```

Gold is still relational.

---

# Canonical Layer

Transforms Gold into

```
Nodes

Edges

Events

Cases
```

Current canonical tables

```
ai_aml_nodes

ai_aml_edges

ai_aml_events

ai_aml_cash_movements

ai_aml_cases
```

---

# Data Contracts

Every dataset has a contract.

Example

```
clients

↓

Contract

↓

Validation

↓

Canonical
```

---

# Contract Components

```
Schema

Business Keys

Update Frequency

Owner

Quality Rules

SLA
```

---

# Example

Dataset

```
Contracts
```

Business Key

```
contract_id
```

Owner

```
Data Governance
```

Frequency

```
Daily
```

---

# Incremental Loads

Preferred

```
CDC

↓

Incremental

↓

Canonical Update
```

Fallback

```
Full Refresh
```

Replay supported.

---

# Change Detection

Every load produces

```
Inserted

Updated

Deleted
```

Only inserted/updated entities generate triggers.

---

# Business Keys

Every canonical entity has

```
Business Key

Stable ID

Version
```

Examples

```
Client

↓

client_id
```

```
Contract

↓

contract_id
```

```
Movement

↓

movement_id
```

---

# Canonical IDs

Internal node identifiers

```
Client:{client_id}

Contract:{contract_id}

CashMovement:{movement_id}
```

Never expose database surrogate keys.

---

# Data Quality

Validation occurs before canonical loading.

Checks

```
Primary Keys

Null Keys

Duplicates

Referential Integrity

Type Validation

Business Rules
```

---

# Examples

Clients

```
No duplicate client_id
```

Contracts

```
Every contract has one client
```

Deposits

```
Every deposit has one contract
```

Withdrawals

```
Every withdrawal has destination account
```

---

# Invalid Records

Do not stop pipeline.

Instead

```
Reject

↓

Quarantine

↓

Quality Report
```

Pipeline continues.

---

# Data Quality Report

Generated every execution.

Contains

```
Rejected Rows

Missing Keys

Duplicates

Broken Relationships

Schema Drift
```

---

# Schema Evolution

Allowed

```
New Nullable Columns
```

Not allowed

```
Breaking PK

Breaking FK

Business Key Changes
```

Require ADR.

---

# Canonical Events

Every load emits

```
ENTITY_CREATED

ENTITY_UPDATED

ENTITY_DELETED
```

Trigger Engine subscribes to these events.

---

# Ownership

| Dataset | Owner |
|----------|-------|
| Clients | Data Governance |
| KYC | Data Governance |
| Contracts | Data Governance |
| Deposits | Data Governance |
| Withdrawals | Data Governance |
| Alerts | AML/TI |
| Cases | AML/TI |
| Canonical Layer | AI Data Science |

---

# Pipeline Metrics

Operational

```
Rows Read

Rows Written

Rows Rejected

Duration

Latency
```

Quality

```
Completeness

Uniqueness

Consistency

Freshness
```

---

# Recovery

If a pipeline fails

```
Retry

↓

Replay

↓

Continue
```

Never delete historical canonical data.

---

# Design Rules

Rule 1

Bronze is immutable.

Rule 2

Silver normalizes.

Rule 3

Gold models business.

Rule 4

Canonical creates graph entities.

Rule 5

Triggers consume canonical data only.

---

# ADR

ADR-025

Canonical Data Pipeline

Decision

Separate operational ingestion from graph construction through a canonical business layer.

Reason

Decouples source systems from analytics, improves reproducibility and simplifies future source integration.

---

# End of Part 2

Next

```
08_IMPLEMENTATION_PART_3.md
```

Topics

- Canonical Entity Builder
- Node Builder
- Edge Builder
- Event Builder
- Versioning
- Hash IDs
- Temporal Validity
- Graph Materialization
