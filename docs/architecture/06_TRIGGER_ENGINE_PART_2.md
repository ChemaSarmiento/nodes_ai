# Trigger Engine
## Part 2 — Queue Architecture, Workers and Execution Flow

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 06_TRIGGER_ENGINE_PART_1

---

# Purpose

This document specifies the asynchronous execution model of the Trigger Engine.

The Trigger Engine must never execute heavy computations synchronously.

Instead, every trigger produces work that is processed asynchronously.

---

# Why Queues?

Without queues

```
Business Event

↓

Trigger

↓

SQL

↓

Graph

↓

LLM

↓

Response
```

Problems

- Slow
- Blocking
- Hard to scale
- No retries
- Poor observability

---

With queues

```
Business Event

↓

Trigger

↓

Queue

↓

Worker

↓

Graph

↓

Packet

↓

LLM
```

Each responsibility becomes independent.

---

# Queue Principles

The queue is responsible only for transporting work.

It never performs business logic.

It never evaluates AML.

It never builds graphs.

---

# Queue Types

Current

```
Trigger Queue
```

Future

```
Packet Queue

LLM Queue

Notification Queue

Feedback Queue

Replay Queue
```

---

# Recommended Technologies

AWS

```
EventBridge

↓

SQS FIFO

↓

Dead Letter Queue
```

Future

```
Kafka

Kinesis

RabbitMQ
```

---

# Queue Message

Every message represents exactly one trigger execution.

Schema

```json
{
  "trigger_run_id":"...",
  "trigger_id":"...",
  "trigger_version":"...",
  "priority":"HIGH",
  "entity_type":"Contract",
  "entity_id":"...",
  "business_key":"...",
  "event_timestamp":"...",
  "configuration_version":"..."
}
```

---

# Required Fields

```
trigger_run_id

trigger_id

entity_type

entity_id

priority

created_at
```

Optional

```
top_contract

client_id

case_id

replay

requested_by
```

---

# Trigger Run ID

Every execution receives

```
trigger_run_id
```

Example

```
TRG-20260825-000000123
```

Everything downstream references this identifier.

---

# Priority Queues

Four logical priorities

```
LOW

MEDIUM

HIGH

CRITICAL
```

Recommended implementation

Single FIFO queue

+

Priority field

Worker scheduling based on priority.

---

# Worker Philosophy

Workers are stateless.

Every worker receives

```
Queue Message

↓

Reads Data

↓

Produces Output

↓

Terminates
```

Workers never cache investigations.

---

# Worker Responsibilities

Trigger Worker

Responsible for

```
Read Queue

↓

Execute SQL Template

↓

Build Graph

↓

Generate Features

↓

Create Explanation Packet
```

Not responsible for

```
Calling Analyst

Disposition

Business Decisions
```

---

# Trigger Worker Flow

```
Receive Message

↓

Validate

↓

Read Configuration

↓

Execute SQL Template

↓

Receive Nodes

↓

Receive Edges

↓

Build NetworkX Graph

↓

Compute Features

↓

Timeline

↓

Graph Paths

↓

Explanation Packet

↓

Persist

↓

Publish Packet
```

---

# Retry Strategy

Failures happen.

Every worker supports retries.

Recommended

```
Retry 1

↓

Retry 2

↓

Retry 3

↓

Dead Letter Queue
```

Never retry forever.

---

# Dead Letter Queue

Purpose

Store failed executions.

Reasons

```
SQL Failure

Graph Failure

Packet Validation

Unexpected Exception
```

Never silently discard work.

---

# Idempotency

Workers must be idempotent.

Running twice should produce the same packet.

Requirement

```
trigger_run_id
```

guarantees reproducibility.

---

# Deduplication

Before execution

Verify

```
trigger_run_id
```

If already processed

Skip.

---

# Trigger Configuration

Each trigger loads configuration.

Example

```yaml
TRG_CASH_IN_OUT:

  enabled: true

  window_days: 7

  withdrawal_ratio: 0.70

  priority: HIGH
```

Configuration must be external.

Never hardcode thresholds.

---

# Trigger Registry

Every execution validates

```
Trigger Exists

↓

Enabled

↓

Configuration Loaded
```

Otherwise

Reject execution.

---

# Scheduling

Supported modes

```
Real Time

Batch

Manual

Replay
```

---

# Replay

Purpose

Recompute historical investigations.

Workflow

```
Historical Event

↓

Replay Queue

↓

Worker

↓

Packet v2
```

Original packet is preserved.

---

# Audit

Every execution stores

```
trigger_run_id

trigger_id

worker_version

configuration_version

started_at

finished_at

status

duration_ms
```

---

# Monitoring

Metrics

```
Messages Received

Messages Processed

Failures

Retries

DLQ Count

Average Duration

Queue Depth
```

---

# Scaling

Workers scale horizontally.

```
Queue

↓

Worker 1

Worker 2

Worker 3

Worker N
```

No coordination required.

---

# Design Rules

Rule 1

One queue message equals one trigger execution.

---

Rule 2

Workers are stateless.

---

Rule 3

Queue never contains business data.

Only references.

---

Rule 4

SQL is executed inside workers.

---

Rule 5

Every execution is auditable.

---

# ADR

ADR-016

Asynchronous Trigger Processing

Decision

All trigger executions are asynchronous.

Reason

Improves scalability, resiliency and observability.

---

# End of Part 2

Next

```
06_TRIGGER_ENGINE_PART_3.md
```

Topics

- SQL Template Library
- Trigger Registry
- Trigger Configuration
- Trigger Dependency Graph
- Manual Triggers
- Replay Engine
- Trigger Governance
- Trigger Metrics
