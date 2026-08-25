# SQL Model
## Part 2 — Canonical Physical Model, DDL and Performance

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 09_SQL_MODEL_PART_1

---

# Purpose

This document defines the physical SQL implementation of the canonical model.

Unlike previous documents, this section specifies table structures, indexing strategy and physical optimization.

---

# Physical Architecture

```
Bronze

↓

Silver

↓

Gold

↓

Canonical

↓

Analytics

↓

Trigger SQL
```

Canonical tables are optimized for

- Trigger execution
- Graph materialization
- Feature extraction

---

# ai_aml_nodes

Purpose

Store every graph entity.

DDL

```sql
create table ai_aml_nodes (

    node_id varchar(512),

    node_type varchar(128),

    business_key varchar(256),

    source_table varchar(128),

    source_pk varchar(256),

    properties_json super,

    observed_at timestamp,

    valid_from timestamp,

    valid_to timestamp,

    is_active boolean,

    created_at timestamp
);
```

---

# Distribution Strategy

Recommended

```
DISTKEY

business_key
```

Sort

```
node_type

business_key
```

---

# ai_aml_edges

Purpose

Store graph relationships.

DDL

```sql
create table ai_aml_edges (

    edge_id varchar(512),

    edge_type varchar(128),

    from_node_id varchar(512),

    to_node_id varchar(512),

    source_table varchar(128),

    source_pk varchar(256),

    properties_json super,

    observed_at timestamp,

    amount numeric(18,2),

    operation_date timestamp,

    is_active boolean,

    created_at timestamp
);
```

---

# Distribution

Recommended

```
DISTKEY

from_node_id
```

Sort

```
edge_type

from_node_id

to_node_id
```

---

# ai_aml_events

Purpose

Store immutable business events.

DDL

```sql
create table ai_aml_events (

    event_id varchar(512),

    event_type varchar(128),

    entity_id varchar(512),

    entity_type varchar(128),

    event_timestamp timestamp,

    payload_json super,

    created_at timestamp
);
```

---

# ai_aml_cash_movements

Purpose

Normalize deposits and withdrawals.

DDL

```sql
create table ai_aml_cash_movements (

    movement_id varchar(512),

    movement_type varchar(32),

    client_id varchar(256),

    contract_id varchar(256),

    amount numeric(18,2),

    operation_date timestamp,

    external_account_hash varchar(512),

    house_concentration_account_hash varchar(512),

    external_institution_hash varchar(512),

    internal_institution_hash varchar(512),

    ordering_name_hash varchar(512),

    beneficiary_name_hash varchar(512),

    source_table varchar(128),

    source_row_hash varchar(512),

    created_at timestamp
);
```

---

# ai_aml_cases

Purpose

Persist AML investigations.

DDL

```sql
create table ai_aml_cases (

    case_id varchar(512),

    case_type varchar(128),

    severity varchar(32),

    status varchar(32),

    trigger_id varchar(128),

    trigger_run_id varchar(128),

    client_id varchar(256),

    contract_id varchar(256),

    top_contract varchar(256),

    packet_id varchar(512),

    created_at timestamp
);
```

---

# Future Tables

```
ai_aml_packets

ai_aml_feedback

ai_aml_devices

ai_aml_sessions

ai_aml_geolocation

ai_aml_watchlists

ai_aml_documents

ai_aml_ubo
```

---

# Business Keys

Every table has one business identifier.

Examples

```
client_id

contract_id

movement_id

case_id
```

Never expose warehouse surrogate keys.

---

# Deterministic IDs

Generated IDs

```
movement_id

packet_id

edge_id
```

Algorithm

```
SHA256
```

Business IDs

```
client_id

contract_id
```

remain unchanged.

---

# Temporal Columns

Every canonical table contains

```
observed_at

created_at
```

Whenever applicable

```
valid_from

valid_to
```

---

# JSON Strategy

Use

```
SUPER
```

for

```
properties_json

payload_json

features_json

packet_json
```

Never create hundreds of nullable columns.

---

# Compression

Recommended

```
AUTO
```

or

```
AZ64
```

for

```
NUMERIC

TIMESTAMP
```

---

# Partition Strategy

Athena

```
year

month

day
```

Partition

```
operation_date
```

---

# Materialized Views

Recommended

```
mv_cash_movements

mv_client_summary

mv_contract_summary

mv_trigger_candidates
```

Never materialize graph projections.

---

# Query Principles

SQL should

- normalize
- aggregate
- filter

SQL should never

- score AML
- build NetworkX
- generate packets
- produce narratives

---

# Performance Guidelines

Expected

```
Canonical Queries

<5 sec
```

```
Trigger Queries

<2 sec
```

```
Movement Aggregation

<10 sec
```

---

# Optimization

Use

```
Predicate Pushdown

Column Pruning

Partition Elimination
```

Avoid

```
SELECT *
```

---

# Data Retention

Current

Canonical

```
Append Only
```

Historical records are never deleted.

Soft-delete only.

---

# Quality Checks

Every load validates

```
Primary Key

Business Key

Null Keys

Duplicates

Broken References
```

Failures generate

```
Quality Report
```

not pipeline termination.

---

# Design Rules

Rule 1

Canonical tables are immutable.

Rule 2

Movement IDs are deterministic.

Rule 3

Graph IDs never change.

Rule 4

Temporal validity is preserved.

Rule 5

SQL prepares data.

Graph computes relationships.

---

# ADR

ADR-031

Canonical Physical Model

Decision

Implement the canonical graph model using relational tables optimized for graph materialization instead of graph persistence.

Reason

Allows seamless migration to a graph database in future phases while preserving the business model.

---

# End of Part 2

Next

```
09_SQL_MODEL_PART_3.md
```

Topics

- Canonical Views
- SQL Templates
- Trigger Queries
- Feature Queries
- Incremental SQL
- Replay SQL
- Materialized Views
- Analytics Layer
