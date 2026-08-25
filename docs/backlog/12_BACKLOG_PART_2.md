# Product Backlog
## Part 2 — Research Roadmap, Platform Evolution and Enterprise Vision

---
Version: 1.0

Status: Living Document

Owner: AI AML Layer

---

# Purpose

This document contains the long-term evolution of the AI AML Layer.

Unlike the MVP roadmap, this backlog contains capabilities that may take years to implement.

Nothing here blocks production.

Everything here increases platform maturity.

---

# Platform Vision

The AI AML Layer eventually becomes

```
Financial Intelligence Platform
```

supporting

```
AML

Fraud

KYC

KYB

Risk

Compliance
```

through one common architecture.

---

# AI Roadmap

## Phase 1

Current

```
Explanation Only
```

Capabilities

- Structured explanations
- Timeline summaries
- Evidence organization

---

## Phase 2

Reasoning

```
Suggested Investigation

Missing Evidence

Related Cases
```

Still

Human decides.

---

## Phase 3

Agentic AI

```
Graph Agent

AML Agent

Regulatory Agent

Narrative Agent

Supervisor Agent
```

Each agent owns a single responsibility.

---

## Phase 4

Compliance Copilot

Capabilities

```
Conversation

Investigation Guidance

Cross-case Navigation

Regulatory Navigation

Historical Context
```

---

# Graph Roadmap

Current

```
Graph-as-Compute
```

↓

Future

```
Persistent Knowledge Graph
```

↓

Eventually

```
Distributed Graph Platform
```

Possible technologies

```
Neo4j

Neptune

TigerGraph

GraphFrames
```

---

# Feature Engineering Roadmap

Current

Static Features

↓

Future

```
Behavior Features

Graph Features

Temporal Features

Sequence Features
```

↓

Later

```
Graph Embeddings

Customer Embeddings

Relationship Embeddings
```

---

# Machine Learning Roadmap

Current

```
Rules

Graph Features
```

↓

Future

```
Gradient Boosting

Random Forest

XGBoost
```

↓

Later

```
Graph Neural Networks
```

Possible models

```
GraphSAGE

GAT

GIN

Temporal GNN
```

---

# Behavioral Intelligence

Future Cases

```
Impossible Travel

Geo Drift

Behavior Drift

Session Hijacking

Device Rotation

Velocity Detection
```

---

# Corporate Intelligence

Future

```
UBO Discovery

Ownership Chains

Corporate Communities

Representative Risk

Control Structures
```

---

# Knowledge Intelligence

Future

```
Knowledge Graph

Document Graph

Evidence Graph

Case Graph

Semantic Search
```

---

# Retrieval

Current

```
Explanation Packet
```

↓

Future

```
Hybrid Retrieval

Semantic Retrieval

Historical Cases

Graph Retrieval
```

↓

Future

```
GraphRAG
```

Only if justified.

---

# Research Topics

Priority

## High

```
Temporal Graphs

Behavior Modeling

Relationship Risk

Sequence Detection
```

---

## Medium

```
Graph Embeddings

Community Detection

Adaptive Thresholds

Online Learning
```

---

## Low

```
LLM Agents

Autonomous Investigation

Knowledge Graph Reasoning
```

---

# Enterprise Features

Future

```
Multi-Tenant

Cross-Broker Analysis

Multi-Country

Regulatory Packages

Multi-Language
```

---

# Platform Services

Future

```
Notification Service

Recommendation Service

Similarity Service

Entity Resolution

Case Search

Evidence Search
```

---

# Security Roadmap

Future

```
Field-Level Encryption

Confidential Computing

Attribute-Based Access Control

Fine-Grained Authorization
```

---

# Performance Roadmap

Current

```
Single Region
```

↓

Future

```
Multi Region

Horizontal Workers

Distributed Queues

Streaming
```

---

# Infrastructure Roadmap

Current

```
Docker

ECS
```

↓

Future

```
Kubernetes

Autoscaling

Service Mesh
```

---

# API Roadmap

Future

```
Graph API

Packet API

Case API

Feedback API

Similarity API
```

Later

```
GraphQL

gRPC
```

---

# Documentation Roadmap

Future

```
OpenAPI

SDK

Architecture Handbook

Operations Manual

Playbooks

Incident Runbooks
```

---

# Research Backlog

Open Questions

- How should graph embeddings complement rule-based AML?
- When should GraphRAG replace packet-only prompting?
- Which behavioral features produce the highest analyst value?
- How should analyst feedback modify trigger thresholds?
- How should corporate ownership confidence be modeled?

---

# Product Principles

The platform should always remain

- Explainable
- Deterministic
- Auditable
- Modular
- Human-Centered

These principles have priority over automation.

---

# Out of Scope

The following are intentionally excluded unless business requirements change.

```
Automatic SAR Filing

Automatic Case Closure

Automatic Regulatory Decisions

Automatic Account Blocking

Fully Autonomous AML
```

---

# Success Definition

Five years from now the platform should support

```
AML

Fraud

KYC

KYB

Investigations

Compliance

Behavior Intelligence

Relationship Intelligence
```

through the same canonical architecture.

---

# ADR

ADR-043

Long-Term Product Evolution

Decision

The AI AML Layer is designed as a reusable financial intelligence platform whose architecture remains stable while new capabilities are added incrementally.

Reason

Architecture should outlive individual AML use cases.

---

# Backlog Complete

The Product Backlog consists of

```
Part 1

Product Features

Technical Debt

Research

------------

Part 2

Enterprise Vision

Platform Evolution

Research Roadmap
```

Together they define the long-term direction of the platform.

---

# Documentation Complete (v1.0)

Core documentation delivered

```
README

00_PROJECT_CONTEXT

01_EXECUTIVE_SUMMARY

02_ARCHITECTURE

03_DATA_MODEL

03_5_CANONICAL_BUSINESS_MODEL

04_GRAPH_MODEL (6 Parts)

05_AML_CASES (6 Parts)

06_TRIGGER_ENGINE (5 Parts)

07_EXPLANATION_ENGINE (4 Parts)

08_IMPLEMENTATION (6 Parts)

09_SQL_MODEL (5 Parts)

10_PYTHON_PROJECT (6 Parts)

11_ROADMAP (3 Parts)

12_BACKLOG (2 Parts)
```

Total:

- **15 documentos principales**
- **44 capítulos técnicos**
- Arquitectura, modelo de datos, grafo, SQL, Python, IA, roadmap y backlog completamente especificados.

