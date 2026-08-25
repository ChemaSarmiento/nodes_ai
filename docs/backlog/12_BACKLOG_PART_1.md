# Product Backlog
## Part 1 — Future Features, Research and Technical Debt

---
Version: 1.0

Status: Living Document

Owner: AI AML Layer

---

# Purpose

This document contains every capability intentionally excluded from the MVP.

Nothing here is considered technical failure.

These items are future investments.

---

# Product Philosophy

The backlog is divided into:

- Product Features
- Technical Debt
- Research
- Infrastructure
- AI
- Graph
- AML
- Fraud

Priority changes over time.

---

# Priority Definitions

P0

Critical

Immediately after MVP.

---

P1

High

Next release.

---

P2

Medium

Useful.

---

P3

Research

Future.

---

# P0

## Orders

Current

```
Not Implemented
```

Add

```
Orders

Executions

Trades
```

Supports

```
Layering

Wash Trades

Mirror Trading
```

---

## Funds

Current

```
Not Implemented
```

Future Nodes

```
Fund

Instrument

Issuer
```

---

## Positions

Current

No positions.

Future

```
Position

Position Movement

NAV

Holdings
```

---

## Transfers

Support

```
Contract

↓

Contract
```

Transfers.

---

## Watchlists

Integrate

```
OFAC

UN

Internal Lists
```

---

# P1

## Devices

Nodes

```
Device

Device Fingerprint
```

Cases

```
Shared Device

New Device
```

---

## Sessions

Future

```
Session

Browser

Operating System
```

---

## Geolocation

Current

Future

```
Country

State

City

Coordinates
```

Cases

```
Impossible Travel

Country Drift

Shared Geo
```

---

## Login History

Cases

```
Night Login

Velocity

Behavior Change
```

---

# P1

## UBO Discovery

Nodes

```
Corporate Entity

Representative

Beneficial Owner
```

Edges

```
OWNS

CONTROLS

REPRESENTS
```

Cases

```
UBO Risk

Indirect Ownership

Control Chains
```

---

## Corporate Documents

Future

```
Articles

Shareholders

Power of Attorney
```

---

# P1

## Relationship Graph

Future

```
Customer

↓

Customer

↓

Representative

↓

UBO

↓

Device

↓

Account
```

Community Detection.

---

# P2

## Fraud Layer

Cases

```
ATO

Impossible Travel

SIM Swap

Shared Device

Velocity
```

---

## Knowledge Graph

Current

Graph-as-compute.

Future

```
Persistent Knowledge Graph
```

Capabilities

```
Semantic Search

Historical Context

Cross Investigation
```

---

## Semantic Retrieval

Search

```
Cases

Entities

Relationships

Packets
```

---

## GraphRAG

Future

Packet

↓

GraphRAG

↓

LLM

Current

Not Required.

---

# P2

## Multi-Agent

Agents

```
Graph Agent

AML Agent

Regulatory Agent

Narrative Agent
```

---

## Recommendation Engine

Future

```
Suggested Cases

Suggested Evidence

Suggested Investigation
```

---

## Similar Cases

Find

```
Top K

Historical Cases
```

Similarity

```
Graph

Features

Timeline
```

---

# P2

## Feature Store

Replace

```
Relational Features
```

with

Dedicated

```
Feature Store
```

---

# P3

## Graph Database

Possible

```
Neo4j

Neptune

TigerGraph
```

Migration

No business changes.

---

## Graph Neural Networks

Research

```
GraphSAGE

GAT

GIN
```

Applications

```
Risk Prediction

Community Detection

Anomaly Detection
```

---

## Temporal Graphs

Current

Static projections.

Future

```
Temporal Graph
```

---

## Streaming

Current

Batch.

Future

```
CDC

Kafka

Kinesis
```

Near Real-Time.

---

## Continuous Monitoring

Trigger

↓

Packet

↓

LLM

↓

Notification

Automatically.

---

# Technical Debt

Current

None before MVP.

Future

- Feature Store
- Streaming
- Neo4j
- Distributed Graph
- Model Registry
- Prompt Registry UI

---

# Documentation Debt

Future

- UML
- OpenAPI
- Architecture Decision Index
- GraphQL Spec
- SDK

---

# End of Part 1

Next

```
12_BACKLOG_PART_2.md
```

Topics

- Research Backlog
- AI Backlog
- Platform Backlog
- Performance Backlog
- Security Backlog
- Enterprise Features
- Product Vision
