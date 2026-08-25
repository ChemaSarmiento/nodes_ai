# SQL Model
## Part 3 — Canonical Views, SQL Templates and Feature Queries

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 09_SQL_MODEL_PART_1
- 09_SQL_MODEL_PART_2

---

# Purpose

This document defines the SQL layer that feeds the Trigger Engine.

Unlike canonical tables, SQL Templates are execution artifacts.

Their responsibility is to retrieve exactly the data required to build a temporary investigation graph.

---

# SQL Philosophy

Every Trigger owns exactly one SQL Template.

```
Trigger

↓

SQL Template

↓

Canonical Dataset

↓

Graph
```

SQL never produces AML conclusions.

SQL never creates cases.

---

# SQL Layers

```
Canonical Tables

↓

Views

↓

Materialized Views

↓

SQL Templates

↓

Graph Builder
```

---

# Canonical Views

Views simplify business access.

Examples

```
vw_clients

vw_contracts

vw_cash_movements

vw_kyc

vw_cases
```

Views contain no AML logic.

---

# Materialized Views

Purpose

Accelerate trigger execution.

Current

```
mv_cash_movements

mv_client_summary

mv_contract_summary

mv_top_contract_summary

mv_trigger_candidates
```

---

# SQL Template Library

Naming

```
QRY_<DOMAIN>_<PURPOSE>_V<version>
```

Examples

```
QRY_CLIENT_CONTEXT_V1

QRY_CONTRACT_CONTEXT_V1

QRY_CASH_IN_OUT_V1

QRY_REPEATED_ROUND_TRIP_V1

QRY_SHARED_ACCOUNT_V1

QRY_KYC_PROFILE_V1
```

---

# SQL Template Contract

Every SQL template returns

```
nodes

edges

events
```

Optional

```
metadata

statistics

warnings
```

Never returns

```
Narratives

Scores

Recommendations
```

---

# Client Context Query

Purpose

Retrieve complete client context.

Returns

```
Client

Contracts

KYC

Cash Movements

Cases
```

Input

```
client_id
```

---

# Contract Context Query

Purpose

Retrieve contract projection.

Returns

```
Contract

Top Contract

Movements

External Accounts
```

Input

```
contract_id
```

---

# Cash In Out Query

Purpose

Support

```
TRG_CASH_IN_OUT
```

Returns

```
Deposits

Withdrawals

Contracts

Top Contract

Timeline
```

Input

```
client_id

contract_id
```

---

# Repeated Round Trip Query

Purpose

Support

```
TRG_REPEATED_ROUND_TRIP
```

Returns

```
Deposit Pairs

Withdrawal Pairs

Cycle Counts

External Accounts
```

---

# Shared Account Query

Purpose

Support

```
TRG_SHARED_ACCOUNT
```

Returns

```
External Account

Related Clients

Related Contracts

Movement Count
```

---

# KYC Query

Purpose

Support

```
TRG_KYC_PROFILE
```

Returns

```
Latest KYC

Historical KYC

Monthly Deposits

Expected Bucket
```

---

# Case Query

Purpose

Rebuild investigation.

Returns

```
Case

Packet

Features

Timeline

Graph Metadata
```

---

# Feature Queries

Some features are cheaper in SQL.

Examples

```
Monthly Deposits

Deposit Count

Withdrawal Count

Average Amount

Contract Count

Institution Count
```

NetworkX computes

```
Paths

Communities

Graph Structure
```

---

# Trigger Candidate Views

Purpose

Reduce graph workload.

Examples

```
mv_cash_in_out_candidates

mv_round_trip_candidates

mv_shared_account_candidates

mv_kyc_candidates
```

Workers consume candidates.

---

# Incremental Queries

Preferred

```
Last Successful Timestamp

↓

New Rows

↓

Canonical Update
```

Never full refresh if unnecessary.

---

# Replay Queries

Purpose

Rebuild historical investigations.

Input

```
Date Range

Trigger

Entity
```

Output

Canonical Dataset.

---

# Analytics Views

Examples

```
vw_trigger_metrics

vw_packet_metrics

vw_feedback_metrics

vw_case_metrics
```

Read-only.

---

# Performance Guidelines

Query should return

```
<10,000 rows
```

for one investigation.

If larger

↓

Refine filters.

---

# Query Optimization

Always

```
Predicate Pushdown

Projection

Partition Elimination
```

Never

```
Cartesian Joins

SELECT *

Nested Views > 3 levels
```

---

# SQL Versioning

Every template stores

```
query_id

version

owner

created_at
```

Workers record

```
query_version
```

in every packet.

---

# Validation

Every SQL template validated for

```
Execution Time

Returned Schema

Missing Columns

Business Keys
```

---

# Design Rules

Rule 1

One Trigger

↓

One SQL Template

---

Rule 2

SQL retrieves.

Graph computes.

---

Rule 3

SQL never explains.

---

Rule 4

Templates are versioned.

---

Rule 5

Views contain no AML logic.

---

# ADR

ADR-032

SQL Template Library

Decision

Every trigger executes through a versioned SQL template returning canonical datasets only.

Reason

Provides deterministic graph construction and simplifies testing.

---

# End of Part 3

Next

```
09_SQL_MODEL_PART_4.md
```

Topics

- Feature Store Tables
- Analytics Tables
- Packet Persistence
- Audit Tables
- Feedback Tables
- Trigger History
- Query Performance Monitoring
- Data Governance
