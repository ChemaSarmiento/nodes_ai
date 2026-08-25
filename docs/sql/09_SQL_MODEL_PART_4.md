# SQL Model
## Part 4 — Analytics Layer, Feature Store, Audit Model and Governance

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 09_SQL_MODEL_PART_1
- 09_SQL_MODEL_PART_2
- 09_SQL_MODEL_PART_3

---

# Purpose

This document specifies the persistence layer used after graph computation.

Unlike canonical tables, these datasets are analytical artifacts.

They never replace source systems.

---

# Data Architecture

```
Canonical Tables

↓

Trigger

↓

NetworkX

↓

Feature Extraction

↓

Analytics Layer

↓

Explanation Packet

↓

Workbench
```

---

# Analytics Layer

Contains

```
Features

Metrics

Packets

Feedback

Trigger History
```

Purpose

- Reporting
- Monitoring
- Reproducibility

---

# Feature Store

Current implementation

```
Relational Tables
```

Future

```
Dedicated Feature Store
```

Examples

```
Feast

Vertex Feature Store

SageMaker Feature Store
```

Business model remains unchanged.

---

# ai_aml_features

Purpose

Persist graph-derived features.

DDL

```sql
create table ai_aml_features (

    feature_id varchar(512),

    case_id varchar(512),

    trigger_run_id varchar(256),

    feature_name varchar(256),

    feature_value varchar(512),

    feature_type varchar(64),

    graph_version varchar(32),

    created_at timestamp
);
```

---

# Feature Categories

```
Structural

Financial

Relationship

Behavior

AML
```

---

# Example

```
cash_in_out_cycles

↓

3
```

```
shared_accounts

↓

1
```

```
deposit_total

↓

1,250,000
```

---

# ai_aml_packets

Purpose

Persist immutable Explanation Packets.

DDL

```sql
create table ai_aml_packets (

    packet_id varchar(512),

    case_id varchar(512),

    trigger_run_id varchar(256),

    packet_version varchar(64),

    prompt_version varchar(64),

    model_version varchar(64),

    packet_json super,

    created_at timestamp
);
```

Packets are append-only.

Never updated.

---

# ai_aml_feedback

Purpose

Persist analyst feedback.

DDL

```sql
create table ai_aml_feedback (

    feedback_id varchar(512),

    case_id varchar(512),

    analyst_id varchar(256),

    disposition varchar(64),

    usefulness_score integer,

    comments varchar(max),

    created_at timestamp
);
```

---

# Feedback Categories

```
Accepted

Rejected

Edited

Escalated

False Positive
```

---

# ai_aml_trigger_history

Purpose

Audit trigger execution.

DDL

```sql
create table ai_aml_trigger_history (

    trigger_run_id varchar(256),

    trigger_id varchar(128),

    trigger_version varchar(64),

    query_version varchar(64),

    packet_version varchar(64),

    worker_version varchar(64),

    duration_ms integer,

    status varchar(32),

    created_at timestamp
);
```

---

# ai_aml_worker_history

Purpose

Observe execution behavior.

Fields

```
worker_id

host

duration

memory

cpu

errors
```

---

# Packet History

Never overwrite packets.

Instead

```
Packet V1

↓

Packet V2

↓

Packet V3
```

All remain available.

---

# Analytics Views

Examples

```
vw_packet_acceptance

vw_trigger_performance

vw_feedback_summary

vw_feature_distribution

vw_case_latency
```

---

# Monitoring Tables

```
ai_aml_metrics

ai_aml_logs

ai_aml_events_history
```

Purpose

Support observability dashboards.

---

# Governance

Every persisted object contains

```
graph_version

trigger_version

query_version

packet_version

model_version
```

This guarantees full reconstruction.

---

# Data Lineage

```
Source

↓

Canonical

↓

Trigger

↓

Graph

↓

Features

↓

Packet

↓

LLM

↓

Feedback
```

Every layer stores provenance.

---

# Data Retention

Canonical

```
Long-term
```

Features

```
Long-term
```

Packets

```
Long-term
```

Logs

```
Configurable
```

Temporary Graphs

```
Never persisted
```

---

# Governance Rules

Rule 1

Canonical data is immutable.

---

Rule 2

Feature values are reproducible.

---

Rule 3

Packets are immutable.

---

Rule 4

Feedback never modifies packets.

---

Rule 5

Audit tables are append-only.

---

# Operational Metrics

Track

```
Packets Generated

Feedback Received

Average Review Time

Trigger Success

Worker Failures

Replay Count
```

---

# Data Quality Metrics

```
Missing Keys

Broken Relationships

Duplicate Nodes

Duplicate Edges

Rejected Rows
```

Future

```
Freshness

Completeness

Coverage
```

---

# SQL Security

Read

```
Canonical

Analytics
```

Write

```
Packets

Metrics

Feedback
```

No service updates canonical business data.

---

# ADR

ADR-033

Analytics Persistence

Decision

Persist only derived analytical artifacts after graph execution.

Reason

Separates immutable business data from derived AI artifacts while maintaining complete auditability.

---

# End of Part 4

Next

```
09_SQL_MODEL_PART_5.md
```

Topics

- Complete SQL Architecture
- SQL Coding Standards
- Repository Structure
- Naming Conventions
- Performance Checklist
- Deployment Checklist
- Migration to GraphDB
- SQL Model Summary
