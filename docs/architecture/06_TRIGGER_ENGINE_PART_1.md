# Trigger Engine
## Part 1 — Event-Driven Architecture and Trigger Philosophy

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 02_ARCHITECTURE
- 04_GRAPH_MODEL
- 05_AML_CASES

---

# Purpose

This document defines the Trigger Engine.

The Trigger Engine is the orchestration layer that transforms business events into AML investigations.

It is the heart of the AI AML Layer.

---

# Philosophy

The Trigger Engine never decides AML outcomes.

Its responsibility is only to answer:

```
Should this business event generate
an investigation candidate?
```

Everything after that belongs to

- Graph Engine
- Feature Engine
- Explanation Engine
- Human Analyst

---

# Why a Trigger Engine?

Traditional AML

```
Event

↓

Rule

↓

Alert
```

AI AML Layer

```
Business Event

↓

Trigger

↓

Graph Projection

↓

Feature Extraction

↓

Explanation Packet

↓

LLM

↓

AML Case

↓

Human Review
```

The trigger is the orchestration mechanism.

---

# Event-Driven Architecture

Everything begins with a business event.

Examples

```
Deposit

Withdrawal

KYC Update

Contract Creation

Login

Future

Order

Execution

Watchlist Match
```

The Trigger Engine subscribes to business events.

It never polls business tables directly.

---

# Trigger Definition

A Trigger is

```
A deterministic rule that determines whether
a business event should create an investigation candidate.
```

A trigger is NOT

- an AML case
- an alert
- an explanation
- an ML model

---

# Trigger Lifecycle

```
Business Event

↓

Normalization

↓

Trigger Evaluation

↓

Trigger Fired?

↓

Yes

↓

Queue

↓

Worker

↓

Graph Projection

↓

AML Case
```

---

# Trigger Responsibilities

The Trigger Engine is responsible for

- Event validation
- Deduplication
- Threshold evaluation
- Trigger selection
- Priority assignment
- Queue publication

The Trigger Engine is NOT responsible for

- graph traversal
- LLM calls
- case disposition
- analyst workflows

---

# Trigger Categories

Triggers belong to one category.

```
Financial

Behavior

Relationship

Corporate

Operational

Manual
```

---

# Financial Triggers

Examples

```
TRG_CASH_IN_OUT

TRG_REPEATED_ROUND_TRIP

TRG_TOP_CONTRACT

TRG_NEW_DESTINATION
```

---

# Behavioral Triggers

Future

```
TRG_IMPOSSIBLE_TRAVEL

TRG_NEW_COUNTRY

TRG_SHARED_DEVICE

TRG_LOGIN_SPIKE
```

---

# Relationship Triggers

Future

```
TRG_SHARED_ACCOUNT

TRG_SHARED_DEVICE

TRG_COMMUNITY

TRG_CLUSTER
```

---

# Corporate Triggers

Future

```
TRG_UBO_DISCOVERY

TRG_UBO_WATCHLIST

TRG_HIGH_RISK_UBO
```

---

# Operational Triggers

Internal

```
TRG_REBUILD_CASE

TRG_REFRESH_PACKET

TRG_REPLAY

TRG_BATCH_REPROCESS
```

---

# Manual Triggers

Requested by analysts.

```
TRG_MANUAL_REVIEW

TRG_REGENERATE_PACKET

TRG_FORCE_GRAPH
```

---

# Trigger Inputs

A trigger always receives

```
Event

↓

Canonical Entity

↓

Configuration
```

Never raw SQL.

---

# Trigger Outputs

A trigger produces

```
Trigger Event

↓

Queue Message
```

Nothing else.

---

# Trigger Properties

Every trigger has

```
trigger_id

trigger_type

priority

enabled

owner

version

configuration
```

Future

```
precision

false_positive_rate

last_updated
```

---

# Trigger States

```
Registered

↓

Enabled

↓

Executing

↓

Completed

↓

Disabled
```

---

# Trigger Registry

Every trigger must exist in the registry.

Example

| Trigger | Category | Enabled |
|----------|----------|----------|
| TRG_CASH_IN_OUT | Financial | Yes |
| TRG_NEW_DESTINATION | Financial | Yes |
| TRG_REPEATED_ROUND_TRIP | Financial | Yes |
| TRG_MANUAL_REVIEW | Manual | Yes |

The registry is the source of truth.

---

# Trigger Priority

Suggested priorities

```
LOW

MEDIUM

HIGH

CRITICAL
```

Priority determines queue order.

Not AML severity.

---

# Trigger Versioning

Every execution stores

```
trigger_id

trigger_version

configuration_version
```

Guarantees reproducibility.

---

# Design Rules

Rule 1

Triggers are deterministic.

---

Rule 2

Triggers never call the LLM.

---

Rule 3

Triggers never compute graph features.

---

Rule 4

Triggers never generate explanations.

---

Rule 5

Triggers only publish work.

---

# ADR

ADR-015

Trigger Engine

Decision

Business events are converted into investigation candidates through deterministic triggers.

Reason

Separates orchestration from analytics.

---

# End of Part 1

Next

```
06_TRIGGER_ENGINE_PART_2.md
```

Topics

- Queue Architecture
- Trigger Queue Schema
- Worker Architecture
- Retry Strategy
- Dead Letter Queue
- Priority Queues
- Scheduling
- Trigger Execution Flow
