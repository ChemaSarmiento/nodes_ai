# 02_ARCHITECTURE

Version: 1.0

# AI AML Layer Architecture

## Purpose

Provide an AI-native AML architecture that transforms operational events into explainable investigations while keeping Redshift/Athena as the source of truth and NetworkX as the graph computation engine.

---

# Design Principles

- Storage != Graph
- Event-driven
- Explainability-first
- Human-in-the-loop
- Modular services
- Cloud-native
- Auditable

---

# Layered Architecture

```text
Operational Sources
        │
        ▼
S3 + Athena + Redshift
        │
        ▼
Canonical Data Model
(nodes / edges / events)
        │
        ▼
Change Detection
        │
        ▼
Trigger Engine
        │
        ▼
Queue (SQS/EventBridge/Kafka)
        │
        ▼
Trigger Worker
        │
        ▼
SQL Template Library
        │
        ▼
Subgraph Builder (NetworkX)
        │
        ▼
Graph Features + Timeline
        │
        ▼
Explanation Packet
        │
        ▼
LLM Explanation Worker
        │
        ▼
AML Workbench
        │
        ▼
Analyst Feedback
```

---

# Components

## 1. Data Layer

Sources:
- Clients
- Contracts
- KYC
- Deposits
- Withdrawals
- Alerts
- Cases
- Watchlists

Storage:
- S3
- Athena
- Redshift

---

## 2. Canonical Layer

Tables

- ai_aml_nodes
- ai_aml_edges
- ai_aml_events
- ai_aml_cash_movements
- ai_aml_cases

Purpose

Single logical representation of entities and relationships.

---

## 3. Trigger Layer

Responsibilities

- Detect changes
- Apply business rules
- Deduplicate
- Prioritize
- Publish trigger events

Trigger Types

- Automatic
- Controlled
- Batch
- Manual

---

## 4. Queue Layer

Purpose

Decouple trigger generation from expensive graph and LLM processing.

Supports

- Retry
- Priority
- DLQ
- Scaling

---

## 5. Graph Compute Layer

Technology

NetworkX

Purpose

Build temporary subgraphs only.

Graph never becomes system of record.

---

## 6. Feature Layer

Outputs

- Paths
- Neighborhoods
- Connected components
- Temporal sequences
- AML indicators

---

## 7. Explanation Layer

Produces

- Explanation Packet
- Timeline
- Evidence
- Missing evidence
- Suggested next action

LLM never queries raw data directly.

---

## 8. Workbench

Displays

- Case
- Timeline
- Graph
- Evidence
- Explanation
- Analyst decision

---

## 9. Feedback Layer

Captures

- Disposition
- Analyst comments
- False positives
- Missing evidence
- Trigger usefulness

Feeds continuous improvement.

---

# Sequence

1. Data arrives
2. Canonical model updated
3. Trigger detected
4. Queue message created
5. Worker executes SQL
6. NetworkX builds subgraph
7. Features computed
8. Explanation Packet created
9. LLM generates explanation
10. Analyst reviews
11. Feedback stored

---

# ADR Summary

ADR-001 AI AML Layer
ADR-002 Graph-as-compute
ADR-003 Trigger-driven execution
ADR-004 Explanation Packet contract
ADR-005 Human-in-the-loop
ADR-006 HouseConcentrationAccount excluded from AML reasoning

---

# Scale-up

Future replacement candidates

- NetworkX -> Neo4j / Neptune
- Batch -> Streaming
- SQL templates -> Feature services
- Manual rules -> Hybrid ML

The external APIs and Explanation Packet remain unchanged.
