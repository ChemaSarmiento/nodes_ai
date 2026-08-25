# Roadmap
## Part 1 — MVP (12 Weeks)

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

---

# Purpose

This roadmap defines the execution strategy for the first MVP of the AI AML Layer.

The objective is to deliver a fully operational end-to-end platform in approximately 12 weeks while minimizing infrastructure changes.

The MVP intentionally avoids:

- Graph Databases
- Streaming
- Production-grade UI
- Trading data
- UBO Discovery
- Fraud Models

Those capabilities belong to later phases.

---

# MVP Objectives

At the end of Week 12 the platform must be capable of:

- Loading canonical business data.
- Building temporary graphs with NetworkX.
- Detecting AML cases.
- Generating Explanation Packets.
- Calling the LLM.
- Presenting investigations in a Workbench.
- Capturing analyst feedback.

---

# MVP Scope

Current data

```
Clients

Contracts

KYC

Deposits

Withdrawals
```

Current AML cases

```
Cash In Out

Repeated Round Trip

Shared External Account

New Destination

KYC Mismatch

Top Contract Flow
```

---

# Milestone 1

## Project Foundation

Weeks

```
1
```

Deliverables

- Repository
- Documentation
- Architecture
- Canonical Business Model
- Graph Model
- ADRs

Owner

```
AI Data Science
```

Dependencies

None.

---

# Milestone 2

## Canonical Data Layer

Weeks

```
2
```

Deliverables

- Bronze
- Silver
- Gold
- Canonical Tables
- Node Builder
- Edge Builder

Owner

```
Global Task

+

AI Data Science
```

Dependencies

Data Governance.

---

# Milestone 3

## Graph Engine

Weeks

```
3
```

Deliverables

- NetworkX
- Graph Builder
- Timeline
- Paths
- Feature Engine

Owner

```
AI Data Science
```

Dependencies

Canonical Layer.

---

# Milestone 4

## Trigger Engine

Weeks

```
4
```

Deliverables

- Trigger Registry
- Queue
- Workers
- SQL Templates

Owner

```
AI Data Science
```

Dependencies

Graph Engine.

---

# Milestone 5

## AML Cases

Weeks

```
5
```

Deliverables

Current cases

```
Cash In Out

Repeated Round Trip

Shared Account

New Destination

KYC

Top Contract
```

Owner

```
AML

+

AI Data Science
```

---

# Milestone 6

## Explanation Engine

Weeks

```
6
```

Deliverables

- Explanation Packet
- Prompt Builder
- LLM Worker
- JSON Validation

Owner

```
AI Data Science
```

---

# Milestone 7

## Workbench

Weeks

```
7
```

Deliverables

- Streamlit
- Timeline
- Graph
- Packet Viewer

Owner

```
AI Data Science
```

---

# Milestone 8

## End-to-End Integration

Weeks

```
8
```

Deliverables

Complete pipeline

```
Canonical

↓

Trigger

↓

Graph

↓

Packet

↓

LLM

↓

Workbench
```

---

# Milestone 9

## Replay

Weeks

```
9
```

Deliverables

Historical replay.

Replay Queue.

Replay Packets.

---

# Milestone 10

## Validation

Weeks

```
10
```

Deliverables

- Unit Tests
- Integration Tests
- Golden Dataset
- Replay Validation

---

# Milestone 11

## Shadow Mode

Weeks

```
11
```

Run against historical production data.

No analyst impact.

Capture

```
Latency

Cases

Packets

Feedback
```

---

# Milestone 12

## MVP Delivery

Weeks

```
12
```

Deliverables

Operational MVP

Documentation

Deployment

Handover

---

# Weekly Deliverables

| Week | Deliverable |
|-------|-------------|
| 1 | Repository + Docs |
| 2 | Canonical Layer |
| 3 | Graph Engine |
| 4 | Trigger Engine |
| 5 | AML Cases |
| 6 | Explanation Engine |
| 7 | Workbench |
| 8 | Integration |
| 9 | Replay |
|10 | Validation |
|11 | Shadow Mode |
|12 | MVP Release |

---

# Owners

| Area | Owner |
|------|-------|
| Data | Data Governance |
| Ingestion | Global Task |
| AML/TI | AML/TI |
| AI AML Layer | AI Data Science |
| Business Validation | AML Operations |

---

# Critical Path

```
Documentation

↓

Canonical Layer

↓

Graph

↓

Triggers

↓

Cases

↓

Packets

↓

LLM

↓

Workbench

↓

Validation

↓

MVP
```

---

# MVP Exit Criteria

The MVP is complete when:

- Canonical model is operational.
- Graph Builder works.
- Six AML cases execute.
- Explanation Packets are generated.
- LLM explanations are available.
- Analysts can investigate cases.
- Feedback is captured.

---

# ADR

ADR-040

MVP Delivery Strategy

Decision

Deliver value incrementally over twelve weeks while preserving the long-term architecture.

Reason

Reduces implementation risk and enables early business validation.

---

# End of Part 1

Next

```
11_ROADMAP_PART_2.md
```

Topics

- Detailed Weekly Plan
- Sprint Backlog
- Risks
- Dependencies
- Resource Allocation
- Success Metrics
