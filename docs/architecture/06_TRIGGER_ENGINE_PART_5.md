# Trigger Engine
## Part 5 — End-to-End Trigger Execution, APIs, Deployment and Operations

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Trigger Engine Parts 1–4
- Graph Model
- AML Cases

---

# Purpose

This document closes the Trigger Engine specification.

It defines:

- End-to-End execution
- Trigger APIs
- Deployment
- State Machine
- Testing
- Operational Runbooks

---

# End-to-End Flow

```
Business Event

↓

Canonical Model Updated

↓

Trigger Engine

↓

Trigger Registry

↓

Configuration

↓

Queue

↓

Worker

↓

SQL Template

↓

Canonical Dataset

↓

NetworkX

↓

Graph Features

↓

Timeline

↓

Explanation Packet

↓

Persist

↓

Packet Event

↓

LLM Worker

↓

Workbench
```

---

# Example

## Deposit Event

```
Deposit

↓

Canonical Movement

↓

TRG_CASH_IN_OUT

↓

Queue

↓

Worker

↓

QRY_CASH_IN_OUT

↓

NetworkX

↓

Timeline

↓

Packet

↓

LLM

↓

Analyst
```

---

# Trigger State Machine

```
REGISTERED

↓

ENABLED

↓

QUEUED

↓

RUNNING

↓

COMPLETED
```

Possible transitions

```
RUNNING

↓

FAILED

↓

RETRY

↓

COMPLETED
```

or

```
FAILED

↓

DLQ
```

---

# Trigger API

Future internal API.

---

## Execute Trigger

```
POST

/triggers/{trigger_id}/execute
```

Body

```json
{
    "entity_type":"Contract",
    "entity_id":"CTR-123"
}
```

Returns

```json
{
    "trigger_run_id":"..."
}
```

---

## Replay

```
POST

/triggers/replay
```

Body

```json
{
    "trigger_id":"TRG_CASH_IN_OUT",
    "start_date":"...",
    "end_date":"..."
}
```

---

## Manual Trigger

```
POST

/triggers/manual
```

Used by analysts.

---

## Registry

```
GET

/triggers
```

Returns

```
Trigger List

Configuration

Status
```

---

# Deployment

Recommended

```
EventBridge

↓

SQS FIFO

↓

ECS Fargate Workers

↓

Redshift

↓

S3

↓

Packet Store
```

Alternative

```
Kafka

↓

Python Workers
```

---

# Scaling

Workers scale independently.

```
Queue

↓

Worker Pool

↓

Packet Builder
```

No coordination.

---

# Configuration Deployment

Configuration is versioned.

```
config/

trigger_registry.yaml

trigger_thresholds.yaml

packet_registry.yaml
```

Every deployment references a configuration version.

---

# Testing Strategy

## Unit Tests

Trigger Logic

```
Given Event

↓

Expected Trigger?
```

---

## Integration Tests

SQL Template

↓

Graph

↓

Packet

---

## Replay Tests

Historical data

↓

Replay

↓

Packet comparison

---

## Regression Tests

Same input

↓

Same packet

Required.

---

# Operational Runbook

## Queue Growth

Action

- Scale workers
- Check SQL latency
- Inspect DLQ

---

## DLQ Growth

Action

- Review failed trigger
- Validate configuration
- Replay after fix

---

## Packet Failure

Action

- Validate schema
- Rebuild graph
- Replay trigger

---

## SQL Failure

Action

- Verify canonical tables
- Validate SQL template version
- Retry

---

# Monitoring Dashboard

Widgets

```
Trigger Rate

Queue Size

Average Runtime

Failures

Retries

DLQ

Packet Success

Worker CPU

Worker Memory
```

---

# Trigger KPIs

Operational

```
Trigger Throughput

Execution Latency

Retry Rate
```

Quality

```
Case Generation Rate

Trigger Precision

False Positive Rate
```

Business

```
Analyst Acceptance

Case Rebuild Rate

Replay Rate
```

---

# Security

Trigger workers receive

```
Read-only

Canonical Tables
```

Never

```
Update

Delete

Modify

Business Data
```

Workers only write

```
Packets

Metrics

Logs
```

---

# Design Rules

Rule 1

Every trigger is deterministic.

Rule 2

Every execution is reproducible.

Rule 3

Queue messages are immutable.

Rule 4

Workers never own state.

Rule 5

Graph computation is temporary.

Rule 6

Explanation Packet is the only contract passed to downstream AI.

---

# Complete Trigger Engine

The Trigger Engine specification consists of

```
Part 1

Philosophy

-----------

Part 2

Queue

-----------

Part 3

Registry

SQL Templates

-----------

Part 4

Workers

Packet Builder

-----------

Part 5

Deployment

APIs

Operations
```

Together they define the orchestration backbone of the AI AML Layer.

---

# ADR

ADR-019

Trigger Engine Architecture

Decision

All AI AML execution begins with deterministic event-driven triggers and ends with an immutable Explanation Packet.

Reason

This architecture separates orchestration, graph computation, explanation generation and human decision-making, enabling scalability, auditability and future migration without changing business logic.

---

# Trigger Engine Complete

Next document

```
docs/architecture/07_EXPLANATION_ENGINE_PART_1.md
```

Topics

- AI Explanation Philosophy
- Why LLMs
- Explanation Packet Contract
- Prompt Orchestration
- Human Review Boundary
- Regulatory References
- Explainability Principles
