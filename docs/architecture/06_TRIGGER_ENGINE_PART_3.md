# Trigger Engine
## Part 3 — SQL Template Library, Trigger Registry and Trigger Governance

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 06_TRIGGER_ENGINE_PART_1
- 06_TRIGGER_ENGINE_PART_2

---

# Purpose

The Trigger Engine does not know how to retrieve business data.

Instead, every trigger delegates retrieval to a SQL Template.

This separates

Business Logic

from

Data Retrieval.

---

# Why SQL Templates?

Without templates

```
Trigger

↓

Embedded SQL

↓

Business Logic
```

Problems

- duplicated SQL
- impossible versioning
- hard to test
- difficult maintenance

---

With templates

```
Trigger

↓

Query Template

↓

Canonical Dataset

↓

Graph
```

Business logic remains independent.

---

# SQL Template Library

Every trigger references one SQL template.

Example

```
TRG_CASH_IN_OUT

↓

QRY_CASH_IN_OUT_V1
```

---

# Naming Convention

```
QRY_<DOMAIN>_<PURPOSE>_V<version>
```

Examples

```
QRY_CLIENT_CONTEXT_V1

QRY_CONTRACT_CONTEXT_V1

QRY_CASH_IN_OUT_V1

QRY_REPEATED_ROUND_TRIP_V1

QRY_KYC_PROFILE_V1

QRY_CASE_CONTEXT_V1
```

---

# SQL Template Contract

Every SQL template must return

```
Nodes

Edges

Timeline
```

Never formatted text.

Never business conclusions.

---

# SQL Template Inputs

Templates receive

```
client_id

contract_id

top_contract

movement_id

case_id

trigger_configuration
```

Never receive entire tables.

---

# SQL Template Outputs

Minimum

```
nodes

edges

events
```

Future

```
documents

watchlists

geo

devices
```

---

# Canonical Dataset

Every SQL template returns the same logical structure.

```
nodes_df

edges_df

events_df
```

Workers never know where the data originated.

---

# Trigger Registry

The registry is the authoritative catalog of triggers.

Every trigger must exist here.

---

# Registry Schema

```
trigger_id

trigger_name

category

owner

enabled

priority

configuration_version

sql_template

packet_template

worker_version

created_at

updated_at
```

---

# Registry Example

| Trigger | SQL | Packet |
|----------|-----|--------|
| TRG_CASH_IN_OUT | QRY_CASH_IN_OUT_V1 | PKT_CASH_IN_OUT_V1 |
| TRG_REPEATED_ROUND_TRIP | QRY_RRT_V1 | PKT_RRT_V1 |
| TRG_KYC_PROFILE | QRY_KYC_PROFILE_V1 | PKT_KYC_PROFILE_V1 |

---

# Trigger Configuration

Configuration must never live in code.

Preferred

```
YAML

JSON

Configuration Table
```

---

Example

```yaml
trigger_id: TRG_CASH_IN_OUT

enabled: true

priority: HIGH

window_days: 7

withdrawal_ratio: 0.70

minimum_amount: 50000
```

---

# Packet Templates

Every trigger references one Explanation Packet template.

Example

```
TRG_REPEATED_ROUND_TRIP

↓

PKT_REPEATED_ROUND_TRIP_V1
```

This guarantees consistent outputs.

---

# Trigger Dependency Graph

Triggers may depend on other triggers.

Example

```
TRG_SHARED_ACCOUNT

↓

Community Detection

↓

Composite Risk
```

Another

```
TRG_NEW_DESTINATION

↓

Repeated Round Trip

↓

Composite AML Case
```

Dependencies must form a Directed Acyclic Graph (DAG).

Circular trigger execution is prohibited.

---

# Manual Triggers

Analysts may request execution manually.

Examples

```
TRG_MANUAL_REVIEW

TRG_REBUILD_GRAPH

TRG_REFRESH_PACKET

TRG_FORCE_CASE
```

These bypass automatic event detection.

---

# Replay Engine

Purpose

Recompute historical investigations using newer logic.

Workflow

```
Historical Event

↓

Replay Trigger

↓

Current SQL

↓

Current Packet

↓

Versioned Output
```

Replay never overwrites historical packets.

---

# Trigger Governance

Every change requires

```
Review

Approval

Version Increment
```

Never modify active triggers in place.

---

# Versioning Rules

Increase version when

- SQL changes
- Threshold changes
- Packet schema changes
- Business logic changes

Do NOT increase version for

- comments
- formatting
- documentation

---

# Trigger Metrics

Every trigger stores

```
execution_count

success_count

failure_count

retry_count

average_duration

case_generation_rate

analyst_acceptance_rate

false_positive_rate
```

Future

```
precision

recall

f1_score
```

---

# Trigger Health

Status

```
Healthy

Warning

Degraded

Disabled
```

Conditions

Healthy

```
Failure Rate < 1%
```

Warning

```
1-5%
```

Degraded

```
>5%
```

Disabled

Manual.

---

# Observability

Every execution emits

```
Logs

Metrics

Trace ID

Trigger Run ID
```

Future

OpenTelemetry.

---

# Design Rules

Rule 1

One trigger references one SQL template.

---

Rule 2

SQL templates never return explanations.

---

Rule 3

Business logic never embeds SQL.

---

Rule 4

Registry is the source of truth.

---

Rule 5

Every execution is reproducible through version identifiers.

---

# ADR

ADR-017

SQL Template Library

Decision

Separate business orchestration from data retrieval through versioned SQL templates.

Reason

Improves maintainability, testing and reproducibility.

---

# End of Part 3

Next

```
06_TRIGGER_ENGINE_PART_4.md
```

Topics

- Trigger Workers
- Packet Builder
- Feature Builder
- Graph Builder
- Worker Orchestration
- Error Handling
- Idempotency
- Performance Optimization
