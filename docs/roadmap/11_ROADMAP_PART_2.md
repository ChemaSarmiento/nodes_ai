# Roadmap
## Part 2 — Detailed Sprint Plan, Dependencies and Risks

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 11_ROADMAP_PART_1

---

# Purpose

This document translates the MVP roadmap into an executable engineering plan.

The objective is to synchronize:

- AI Data Science
- Global Task
- Data Governance
- AML/TI
- AML Business

under one execution timeline.

---

# Sprint Duration

Recommended

```
2 Weeks
```

Total

```
6 Sprints

≈12 Weeks
```

---

# Sprint 1

## Foundation

Goals

- Repository
- Documentation
- ADR
- Canonical Business Model
- Graph Model
- SQL Standards

Deliverables

```
Repository

Documentation

Architecture

Canonical Model

Graph Specification
```

Owner

```
AI Data Science
```

Dependencies

None.

---

# Sprint 2

## Canonical Layer

Goals

Normalize

```
Clients

Contracts

KYC

Deposits

Withdrawals
```

Deliverables

```
Bronze

Silver

Gold

Canonical

Node Builder

Edge Builder
```

Owner

```
Global Task

+

AI Data Science
```

Dependencies

Data Governance.

---

# Sprint 3

## Trigger + Graph

Goals

Implement

```
Trigger Registry

Trigger Worker

NetworkX

Feature Engine

Timeline

Graph Paths
```

Deliverables

Graph operational.

Owner

```
AI Data Science
```

---

# Sprint 4

## AML Cases

Goals

Implement

```
Cash In Out

Repeated Round Trip

Shared Account

New Destination

KYC Mismatch

Top Contract
```

Deliverables

Cases operational.

Dependencies

Graph.

---

# Sprint 5

## Explanation

Goals

Implement

```
Packet Builder

Prompt Builder

LLM Worker

Workbench

Feedback
```

Deliverables

Complete investigation pipeline.

---

# Sprint 6

## Validation

Goals

- Replay
- Golden Dataset
- Shadow Mode
- Metrics
- Documentation
- MVP Release

Deliverables

Production-ready MVP.

---

# Team Responsibilities

## Data Governance

Owns

```
Client

Contract

KYC

Deposits

Withdrawals
```

Responsible for

- data contracts
- data quality
- business keys

---

## Global Task

Owns

```
Ingestion

ETL

Canonical Pipeline

Scheduling
```

Responsible for

- Bronze
- Silver
- Gold
- Incremental loads

---

## AI Data Science

Owns

```
Canonical Model

Graph

Triggers

Packets

LLM

Workbench
```

Responsible for

- implementation
- architecture
- testing

---

## AML/TI

Owns

```
Alerts

Cases

Watchlists

Operational Rules
```

Responsible for

- AML validation
- trigger tuning
- replay

---

## AML Operations

Owns

```
Review

Disposition

Feedback
```

Responsible for

- business validation
- acceptance
- investigation quality

---

# Critical Dependencies

```
Documentation

↓

Canonical Data

↓

Graph

↓

Trigger

↓

Cases

↓

Packets

↓

LLM

↓

Workbench
```

Nothing should start before the previous layer stabilizes.

---

# Risks

## Risk 1

Canonical data quality

Mitigation

```
Data Contracts

Quality Reports

Golden Dataset
```

---

## Risk 2

Trigger explosion

Mitigation

```
Registry

Configuration

Thresholds

Replay
```

---

## Risk 3

Slow Graph

Mitigation

```
Subgraphs Only

Traversal Limits

Feature Optimization
```

---

## Risk 4

LLM Cost

Mitigation

```
Packet Compression

Model Routing

Prompt Optimization
```

---

## Risk 5

Hallucinations

Mitigation

```
Explanation Packets

Structured Outputs

Guardrails
```

---

# Weekly Ceremony

Monday

```
Sprint Planning
```

Wednesday

```
Architecture Review
```

Friday

```
Demo

Metrics

Risks
```

---

# KPIs

Engineering

```
Velocity

Coverage

Build Success

Deployment Time
```

Business

```
Cases

Review Time

Acceptance

False Positive Trend
```

AI

```
Packet Quality

Prompt Acceptance

Token Cost

Latency
```

---

# Success Metrics

Sprint considered complete when

- deliverables merged
- tests green
- documentation updated
- ADR created if needed
- demo accepted

---

# Release Gate

The MVP cannot be released unless

- Graph is reproducible
- Packets immutable
- Triggers deterministic
- Replay validated
- Shadow mode completed
- Documentation complete

---

# ADR

ADR-041

Incremental Delivery

Decision

Develop the AI AML Layer in architecture-first increments.

Reason

Stabilizes interfaces before implementation and reduces integration risk.

---

# End of Part 2

Next

```
11_ROADMAP_PART_3.md
```

Topics

- Gantt
- Future Roadmap
- Phase 2
- Phase 3
- Enterprise Vision
- Final Roadmap Summary
