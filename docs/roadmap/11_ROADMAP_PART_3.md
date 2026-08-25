# Roadmap
## Part 3 — Enterprise Roadmap, Phase Evolution and Product Vision

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Roadmap Part 1
- Roadmap Part 2

---

# Purpose

This document defines the long-term evolution of the AI AML Layer beyond the MVP.

The MVP solves a specific operational problem.

The long-term objective is to become the intelligence layer for Compliance.

---

# Product Vision

Current

```
AML Investigation Assistant
```

↓

Future

```
AI Compliance Platform
```

↓

Enterprise

```
Financial Intelligence Platform
```

---

# Evolution Strategy

The platform evolves through successive layers.

```
Phase 1

↓

Phase 2

↓

Phase 3

↓

Enterprise
```

No architectural redesign should be required.

---

# Phase 1

## MVP

Duration

```
≈3 Months
```

Capabilities

```
Canonical Layer

Trigger Engine

NetworkX

AML Cases

Explanation Packet

LLM

Workbench
```

Supported Data

```
Clients

Contracts

Deposits

Withdrawals

KYC
```

Goal

Operational investigation assistant.

---

# Phase 2

## Extended AML

Duration

```
3-6 Months
```

New Sources

```
Orders

Executions

Funds

Positions

Transfers

Watchlists
```

New Cases

```
Layering

Round Trip Trading

Low Liquidity

Watchlist Hits
```

New Graph Nodes

```
Instrument

Order

Execution

Position
```

Goal

Move from Cash AML to Investment AML.

---

# Phase 3

## Corporate AML

Duration

```
6-9 Months
```

New Domains

```
KYB

UBO

Corporate Structures

Representatives

Corporate Documents
```

New Cases

```
UBO Discovery

Corporate Risk

Control Chains

Indirect Ownership

Representative Risk
```

Goal

Relationship-centric AML.

---

# Phase 4

## Behavioral Intelligence

Duration

```
9-12 Months
```

New Sources

```
Login

Geo

Session

Device

IP

Behavior
```

New Cases

```
Impossible Travel

Device Sharing

Geo Drift

Behavior Drift

Velocity
```

Goal

Merge Fraud and AML signals.

---

# Phase 5

## Knowledge Intelligence

Duration

```
12-18 Months
```

New Components

```
Knowledge Graph

Document Graph

Semantic Search

Case Similarity

Cross Investigation Search
```

Future AI

```
GraphRAG

Agentic Investigation

Case Recommendation
```

Goal

Institutional memory.

---

# Phase 6

## Enterprise Compliance Platform

Domains

```
AML

Fraud

KYC

KYB

Sanctions

PEP

Adverse Media

Operational Risk
```

Common Intelligence Layer

↓

One Investigation Platform.

---

# Technology Evolution

Current

```
NetworkX
```

↓

Future

```
Neo4j

Neptune

TigerGraph
```

↓

Eventually

```
Distributed Graph
```

Business model unchanged.

---

# Data Evolution

Current

```
Relational

↓

Graph Compute
```

Future

```
Streaming

CDC

Near Real-Time
```

Eventually

```
Continuous Intelligence
```

---

# AI Evolution

Current

```
Explanation
```

↓

Future

```
Recommendation
```

↓

Later

```
Investigation Planning
```

↓

Enterprise

```
Compliance Copilot
```

Human remains decision maker.

---

# Future Workbench

Current

```
Timeline

Graph

Explanation
```

Future

```
Evidence Explorer

Case Similarity

Investigation History

Graph Search

Regulatory References

Analyst Collaboration
```

---

# Success Metrics

MVP

```
Investigation Time

Packet Quality

Analyst Acceptance
```

Phase 2

```
Case Quality

False Positive Trend

Coverage
```

Phase 3

```
Relationship Discovery

Corporate Coverage
```

Enterprise

```
Investigation Productivity

Regulatory Readiness

Analyst Capacity
```

---

# Long-Term Architecture

```
Canonical Layer

↓

Event Layer

↓

Trigger Layer

↓

Graph Layer

↓

Feature Layer

↓

Explanation Layer

↓

Workbench

↓

Feedback

↓

Continuous Learning
```

Architecture remains stable.

Only capabilities grow.

---

# Enterprise Vision

The AI AML Layer becomes

```
The Intelligence Layer
```

for

```
Compliance

Fraud

Risk

KYC

Investigations
```

Everything shares

- Canonical Model
- Trigger Engine
- Graph
- Explanation Engine

---

# Future Research

Potential topics

```
Graph Neural Networks

Temporal Graphs

Knowledge Graph Embeddings

Hybrid Retrieval

Adaptive Thresholds

Behavioral Baselines

Entity Resolution

Cross Institution Networks
```

---

# Final Success Criteria

The platform succeeds if:

- New AML cases are added without redesign.
- New data sources plug into the canonical model.
- Graph engine can be replaced transparently.
- AI remains explainable.
- Investigations remain reproducible.
- Human review remains mandatory.

---

# ADR

ADR-042

Long-Term Platform Vision

Decision

Design the AI AML Layer as a reusable intelligence platform rather than an isolated AML solution.

Reason

The same architecture supports AML, Fraud, KYC, KYB and future Compliance capabilities with minimal architectural changes.

---

# Roadmap Complete

The roadmap now consists of

```
Part 1

MVP

------------

Part 2

Execution

------------

Part 3

Enterprise Vision
```

Together they define

- MVP
- Delivery Plan
- Team Responsibilities
- Milestones
- Future Evolution
- Enterprise Strategy

---

# Next Document

```
docs/backlog/12_BACKLOG_PART_1.md
```

Topics

- Product Backlog
- Technical Debt
- Future Features
- Research Topics
- Nice-to-Have Features
