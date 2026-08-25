# Trigger Engine
## Part 4 — Workers, Graph Builder and Packet Builder

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 06_TRIGGER_ENGINE_PART_1
- 06_TRIGGER_ENGINE_PART_2
- 06_TRIGGER_ENGINE_PART_3
- 04_GRAPH_MODEL

---

# Purpose

Once a trigger has been accepted and placed into the queue,
the Trigger Worker becomes responsible for transforming a business event into an Explanation Packet.

This document specifies the execution pipeline.

---

# Worker Philosophy

Workers are:

- Stateless
- Deterministic
- Idempotent
- Horizontally Scalable

Workers never keep internal state.

Every execution can be reconstructed.

---

# Worker Pipeline

```
Queue Message

↓

Load Trigger

↓

Load Configuration

↓

Execute SQL Template

↓

Canonical Dataset

↓

Build Graph

↓

Extract Features

↓

Build Timeline

↓

Generate Explanation Packet

↓

Persist Results

↓

Publish Event
```

---

# Worker Components

```
Trigger Worker

↓

SQL Executor

↓

Graph Builder

↓

Feature Builder

↓

Timeline Builder

↓

Packet Builder

↓

Persistence

↓

Publisher
```

Each component has one responsibility.

---

# SQL Executor

Purpose

Retrieve the canonical dataset.

Input

```
trigger_run_id

entity_type

entity_id

configuration
```

Output

```
nodes_df

edges_df

events_df
```

No graph logic occurs here.

---

# Graph Builder

Purpose

Build a temporary investigation graph.

Technology

```
NetworkX
```

Input

```
nodes_df

edges_df
```

Output

```
nx.DiGraph
```

Rules

- Directed graph
- Temporary
- No persistence
- No caching

---

# Graph Validation

Before analysis

Validate

```
Graph Connected

Required Nodes Exist

No Missing Root

No Invalid Edge Types
```

If validation fails

```
Worker Error

↓

Retry

↓

DLQ
```

---

# Feature Builder

Purpose

Compute graph-derived features.

Input

```
Graph

Timeline

Configuration
```

Output

```
Feature Dictionary
```

Categories

```
Structural

Financial

Relationship

Behavior

AML
```

---

# Timeline Builder

Purpose

Convert graph events into chronological sequence.

Output

```
Timeline
```

Example

```
08:05

Deposit

↓

08:20

Withdrawal

↓

09:10

Deposit
```

Timeline is sorted by business timestamp.

---

# Path Builder

Purpose

Generate human-readable graph paths.

Input

```
Graph
```

Output

```
Client

↓

Contract

↓

Withdrawal

↓

External Account
```

Paths are stored as ordered node labels.

Never expose internal IDs.

---

# Packet Builder

Purpose

Convert graph outputs into canonical packet.

Input

```
Graph

Features

Timeline

Paths

Trigger Metadata
```

Output

```
Explanation Packet
```

Packet Builder never calls the LLM.

---

# Packet Validation

Required

```
Summary

Timeline

Features

Evidence

Graph Paths

Recommended Action
```

If required field missing

Reject packet.

---

# Packet Persistence

Store

```
packet_id

trigger_run_id

packet_version

packet_json

created_at
```

Never overwrite.

Always append.

---

# Graph Persistence

Current

```
No
```

Only persist

```
Packet

Features

Metrics
```

Graph is discarded.

---

# Publisher

After successful packet generation

Publish

```
PACKET_CREATED
```

Future subscribers

```
LLM Worker

Workbench

Notification Service

Analytics
```

---

# Error Handling

Errors

```
Configuration

↓

SQL

↓

Graph

↓

Packet

↓

Persistence

↓

Publishing
```

Each stage reports independently.

---

# Retry Strategy

Retry only transient failures.

Retry

```
Database Timeout

Temporary IO

Queue Failure
```

Never retry

```
Invalid Configuration

Schema Error

Business Validation Failure
```

---

# Idempotency

Rule

Same

```
trigger_run_id
```

must produce identical packet.

Workers should check

```
Already Processed?
```

before execution.

---

# Performance

Expected execution

```
< 5 seconds

Simple Case
```

```
< 30 seconds

Large Investigation
```

No LLM included.

---

# Parallelism

Allowed

```
Multiple Workers

↓

Different Trigger Runs
```

Forbidden

```
Multiple Workers

↓

Same trigger_run_id
```

---

# Logging

Every stage logs

```
Start

Finish

Duration

Status

Error

Rows Returned

Graph Size

Packet Size
```

---

# Monitoring Metrics

```
Worker Count

Queue Depth

Execution Time

SQL Time

Graph Time

Packet Time

Failure Rate
```

---

# Design Rules

Rule 1

Workers never access UI.

---

Rule 2

Workers never call analysts.

---

Rule 3

Graph Builder only builds graphs.

---

Rule 4

Packet Builder never computes graph features.

---

Rule 5

Graph is destroyed after packet generation.

---

# ADR

ADR-018

Stateless Workers

Decision

Workers are stateless and disposable.

Reason

Horizontal scaling.

Easy deployment.

Deterministic execution.

---

# End of Part 4

Next

```
06_TRIGGER_ENGINE_PART_5.md
```

Topics

- End-to-End Trigger Examples
- Trigger State Machine
- Trigger APIs
- Deployment
- Sequence Diagrams
- Testing Strategy
- Operational Runbooks
- Trigger Engine Summary
