# Implementation Guide
## Part 6 — End-to-End Deployment, Operations and MVP Readiness

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Implementation Parts 1–5
- Trigger Engine
- Explanation Engine

---

# Purpose

This document completes the implementation specification.

It defines the complete execution path from source systems to analyst review.

It also defines the production readiness checklist for the MVP.

---

# End-to-End Architecture

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

Queue

↓

Trigger Worker

↓

SQL Templates

↓

NetworkX

↓

Feature Extraction

↓

Timeline

↓

Explanation Packet

↓

LLM Worker

↓

Workbench

↓

Analyst

↓

Feedback

↓

Continuous Improvement
```

---

# End-to-End Responsibilities

## Data Governance

Owns

```
Client

Contracts

KYC

Deposits

Withdrawals
```

---

## Global Task

Owns

```
Ingestion

Normalization

Canonical Pipeline

Scheduling
```

---

## AI AML Layer

Owns

```
Canonical Model

Trigger Engine

Graph

Features

Packets

LLM
```

---

## AML/TI

Owns

```
Alerts

Cases

Watchlists

Operational Rules
```

---

## AML Operations

Owns

```
Review

Disposition

Feedback

Escalation
```

---

# Local Development Workflow

```
Git Pull

↓

Load Local CSV

↓

Build Canonical Tables

↓

Generate Nodes

↓

Generate Edges

↓

NetworkX

↓

Detect Cases

↓

Generate Packets

↓

Run Streamlit
```

Everything must work without AWS.

---

# Production Workflow

```
Operational Systems

↓

S3

↓

Athena

↓

Redshift

↓

Canonical Layer

↓

Trigger Engine

↓

SQS

↓

Workers

↓

Packets

↓

LLM

↓

Workbench
```

No analyst accesses canonical tables directly.

---

# Configuration Management

Configuration is external.

```
config/

application.yaml

graph.yaml

triggers.yaml

llm.yaml

logging.yaml
```

Environment-specific overrides are allowed.

---

# Deployment Strategy

Recommended

```
Docker Images

↓

CI

↓

Artifact Registry

↓

Development

↓

QA

↓

Production
```

Never deploy directly from local machines.

---

# Release Strategy

Every release includes

```
Application Version

Graph Version

Packet Version

Trigger Version

Prompt Version
```

This allows complete reproducibility.

---

# Rollback Strategy

Rollback only application components.

Never rollback

```
Canonical Data
```

If necessary

```
Replay Triggers

↓

Rebuild Packets
```

---

# Security Checklist

Before Production

- Secrets in Secrets Manager
- IAM least privilege
- Encrypted S3 buckets
- Encrypted Redshift
- TLS everywhere
- Audit logs enabled
- Versioned prompts
- Versioned packets

---

# Performance Checklist

Expected

```
Canonical Build

<10 min
```

```
Trigger Execution

<2 sec
```

```
Graph Build

<5 sec
```

```
Packet Build

<1 sec
```

```
LLM Response

<15 sec
```

```
Workbench

<2 sec
```

---

# Operational Checklist

Every deployment verifies

```
Canonical Tables

↓

Trigger Registry

↓

Queue

↓

Workers

↓

Graph Builder

↓

Packet Builder

↓

LLM

↓

Workbench
```

Any failure blocks release.

---

# Monitoring Checklist

Infrastructure

```
CPU

Memory

Queue

Workers
```

Business

```
Cases

Triggers

Packets

Feedback
```

AI

```
Latency

Tokens

Cost

Validation
```

---

# Disaster Recovery

Recovery Order

```
Canonical Data

↓

Triggers

↓

Packets

↓

LLM

↓

Workbench
```

Graphs are never restored.

They are rebuilt.

---

# MVP Definition

The MVP is considered complete when the following are operational.

Data

- Clients
- Contracts
- KYC
- Deposits
- Withdrawals

Canonical Layer

- Nodes
- Edges
- Events

Graph

- NetworkX
- Subgraphs
- Features
- Timeline

Cases

- Cash In Out
- Repeated Round Trip
- Shared Account
- New Destination
- KYC Mismatch
- Top Contract Flow

AI

- Explanation Packet
- LLM Worker
- Structured Output

Workbench

- Case View
- Timeline
- Graph
- Feedback

---

# Non-MVP

Not required

```
Neo4j

Streaming

Orders

Executions

Funds

Devices

Geo

UBO

Fraud

Knowledge Graph DB
```

These belong to later phases.

---

# Success Criteria

The MVP succeeds if

- Canonical model is stable.
- Graphs are reproducible.
- Triggers generate deterministic cases.
- Packets are immutable.
- LLM produces structured explanations.
- Analysts can complete investigations faster.
- Feedback is captured.

---

# Architecture Summary

The AI AML Layer consists of

```
Canonical Layer

↓

Trigger Engine

↓

NetworkX

↓

Feature Engine

↓

Explanation Packet

↓

LLM

↓

Workbench
```

Each layer has a single responsibility.

No layer depends directly on implementation details of another.

---

# ADR

ADR-029

MVP Architecture

Decision

Deliver the AI AML Layer as an event-driven, graph-compute platform using relational storage and temporary graphs.

Reason

Minimizes infrastructure changes while maximizing explainability and future scalability.

---

# Implementation Guide Complete

Documents

```
08_IMPLEMENTATION_PART_1

Repository

------------------

08_IMPLEMENTATION_PART_2

Canonical Pipeline

------------------

08_IMPLEMENTATION_PART_3

Canonical Builders

------------------

08_IMPLEMENTATION_PART_4

Graph Engine

------------------

08_IMPLEMENTATION_PART_5

Services

------------------

08_IMPLEMENTATION_PART_6

Deployment & MVP
```

Together they define the complete implementation blueprint.

---

# Next Document

```
docs/sql/09_SQL_MODEL_PART_1.md
```

Topics

- Canonical SQL Architecture
- Schema Design
- Naming Standards
- Canonical Tables
- Business Keys
- Physical Model
