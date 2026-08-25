# SQL Model
## Part 1 — Canonical SQL Architecture

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 03_DATA_MODEL
- 04_GRAPH_MODEL
- 08_IMPLEMENTATION

---

# Purpose

This document defines the canonical SQL architecture used by the AI AML Layer.

SQL is responsible for:

- Canonical data modeling
- Entity normalization
- Relationship extraction
- Trigger datasets
- Feature staging

SQL is NOT responsible for:

- Graph traversal
- AML scoring
- Explanation generation
- LLM orchestration

---

# SQL Philosophy

SQL prepares business entities.

NetworkX computes relationships.

LLM explains.

Each layer has one responsibility.

---

# Data Flow

```
Operational Sources

↓

Bronze

↓

Silver

↓

Gold

↓

Canonical Tables

↓

Trigger SQL

↓

Graph Builder
```

---

# Canonical Schemas

Recommended

```
raw

silver

gold

canonical

analytics

audit
```

---

# Schema Responsibilities

## raw

Immutable source copies.

```
crm_clients

crm_contracts

aml_alerts
```

---

## silver

Normalization.

```
clients

contracts

kyc

deposits

withdrawals
```

---

## gold

Business-ready datasets.

```
gold_clients

gold_contracts

gold_cash_movements
```

---

## canonical

Graph inputs.

```
ai_aml_nodes

ai_aml_edges

ai_aml_events

ai_aml_cases
```

---

## analytics

Derived metrics.

```
case_metrics

trigger_metrics

feedback_metrics
```

---

## audit

Operational history.

```
packet_history

trigger_history

worker_history
```

---

# Naming Convention

Tables

```
snake_case
```

Columns

```
snake_case
```

Views

```
vw_

Example

vw_client_summary
```

Materialized Views

```
mv_

Example

mv_cash_movements
```

---

# Business Keys

Never use surrogate keys for graph identity.

Current

```
client_id

contract_id

movement_id

top_contract

case_id
```

---

# Canonical Tables

Current

```
ai_aml_nodes

ai_aml_edges

ai_aml_events

ai_aml_cash_movements

ai_aml_cases
```

Future

```
ai_aml_packets

ai_aml_feedback

ai_aml_devices

ai_aml_sessions

ai_aml_watchlists
```

---

# ai_aml_nodes

Purpose

Store graph nodes.

Minimum columns

```
node_id

node_type

business_key

properties_json

observed_at

valid_from

valid_to
```

---

# ai_aml_edges

Purpose

Store graph relationships.

Minimum

```
edge_id

edge_type

from_node_id

to_node_id

properties_json

observed_at
```

---

# ai_aml_events

Purpose

Store immutable business events.

Examples

```
Deposit

Withdrawal

Future

Login

Order

Execution
```

---

# ai_aml_cash_movements

Normalized financial events.

Current

```
Deposits

Withdrawals
```

Future

```
Transfers

Internal Movements
```

---

# ai_aml_cases

Generated investigations.

Contains

```
Case Metadata

Severity

Status

Trigger

Packet Reference
```

---

# JSON Columns

Preferred

```
SUPER
```

(Redshift)

or

```
JSON
```

Other engines.

Examples

```
properties_json

features_json

packet_json
```

---

# Data Types

IDs

```
VARCHAR
```

Amounts

```
NUMERIC
```

Dates

```
TIMESTAMP
```

Flags

```
BOOLEAN
```

---

# Partition Strategy

Athena

```
year

month

day
```

Redshift

```
SORTKEY

DISTKEY
```

by

```
client_id

contract_id

operation_date
```

---

# SQL Principles

Rule 1

Business entities first.

---

Rule 2

Canonical IDs are deterministic.

---

Rule 3

Graph tables never contain business logic.

---

Rule 4

Canonical tables are append-oriented.

---

Rule 5

Graph computation never occurs in SQL.

---

# ADR

ADR-030

Canonical SQL Model

Decision

Use relational canonical tables as the single source for graph computation.

Reason

Allows Redshift/Athena to remain the system of record while enabling graph analytics without introducing a Graph Database.

---

# End of Part 1

Next

```
09_SQL_MODEL_PART_2.md
```

Topics

- DDL
- Nodes
- Edges
- Events
- Cash Movements
- Indexing
- Distribution Keys
- Performance
