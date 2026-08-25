# Implementation Guide
## Part 5 — Services, APIs, Workbench, Observability and Deployment

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Trigger Engine
- Explanation Engine
- Graph Model

---

# Purpose

This document defines the application layer that exposes the AI AML Layer to users.

Unlike previous documents, this one describes deployable services.

---

# Microservices

Current architecture

```
Canonical API

↓

Trigger API

↓

Graph Service

↓

Packet Service

↓

LLM Service

↓

Workbench API
```

Each service owns one responsibility.

---

# Service Catalog

Current

```
canonical-service

trigger-service

graph-service

packet-service

llm-service

workbench-service
```

Future

```
notification-service

regulatory-service

analytics-service

feedback-service
```

---

# Canonical Service

Responsibilities

```
Read canonical entities

Read canonical events

Read canonical relationships
```

Never performs graph computation.

---

# Trigger Service

Responsibilities

```
Execute trigger

Replay trigger

Manual execution

Trigger registry
```

API

```
POST /trigger/execute

POST /trigger/replay

GET /trigger

GET /trigger/{id}
```

---

# Graph Service

Responsibilities

```
Build Graph

Build Subgraph

Extract Features

Timeline

Graph Paths
```

API

```
POST /graph/build

GET /graph/client/{id}

GET /graph/case/{id}

GET /graph/contract/{id}
```

---

# Packet Service

Responsibilities

```
Packet Generation

Packet Validation

Packet Persistence
```

API

```
POST /packet/build

GET /packet/{id}
```

---

# LLM Service

Responsibilities

```
Prompt Builder

Prompt Routing

Model Selection

LLM Invocation

Validation
```

API

```
POST /explain
```

Input

```
Explanation Packet
```

Output

```
Structured Explanation
```

---

# Workbench Service

Responsibilities

```
Dashboard

Case View

Graph View

Timeline

Feedback
```

Future

React application.

Current

Streamlit prototype.

---

# Streamlit Prototype

Current screens

```
Home

↓

Case List

↓

Case Detail

↓

Timeline

↓

Graph

↓

Explanation

↓

Feedback
```

---

# Graph Visualization

Current

```
PyVis HTML
```

Embedded in Streamlit.

Future

```
React

Cytoscape

Graphistry
```

---

# API Standards

REST

JSON

OpenAPI

Versioned

Every endpoint returns

```
request_id

timestamp

version
```

---

# Authentication

Current

```
API Key
```

Future

```
OAuth

IAM

JWT

SSO
```

---

# Authorization

Roles

```
Admin

AML Analyst

Supervisor

Data Scientist

Read Only
```

Future

Row-level security.

---

# Logging

Every request logs

```
request_id

user

service

duration

status

case_id

trigger_run_id
```

---

# Metrics

Infrastructure

```
CPU

Memory

Latency

Errors
```

Business

```
Cases Generated

Packets Generated

Analyst Feedback

Trigger Success
```

AI

```
Prompt Count

Token Count

Cost

Validation Failures
```

---

# Observability

Recommended

```
CloudWatch

Grafana

Prometheus
```

Future

```
OpenTelemetry
```

---

# Deployment

Current

```
Docker

↓

ECS Fargate
```

Alternative

```
Kubernetes
```

---

# Infrastructure

```
S3

↓

Athena

↓

Redshift

↓

Trigger Engine

↓

SQS

↓

Workers

↓

Packet Store

↓

Workbench
```

---

# Secrets

Use

```
AWS Secrets Manager
```

Never store

```
API Keys

Passwords

Tokens
```

inside repository.

---

# CI/CD

Pipeline

```
Git

↓

PR

↓

Unit Tests

↓

Integration Tests

↓

Docker Build

↓

Deploy

↓

Smoke Test
```

---

# Environments

```
Local

↓

Development

↓

QA

↓

Production
```

Every environment has independent configuration.

---

# Backup

Persist

```
Packets

Feedback

Metrics

Canonical Tables
```

Do not back up temporary graphs.

---

# Disaster Recovery

Recover

```
Canonical Data

↓

Replay Triggers

↓

Rebuild Packets

↓

Continue
```

Graphs never need recovery.

---

# Design Rules

Rule 1

Services are stateless.

Rule 2

Packets are immutable.

Rule 3

Graph is ephemeral.

Rule 4

Configuration is external.

Rule 5

APIs expose business objects, not implementation details.

---

# ADR

ADR-028

Service Architecture

Decision

The AI AML Layer is implemented as loosely coupled stateless services communicating through immutable contracts.

Reason

Allows independent scaling, easier testing and future replacement of components.

---

# End of Part 5

Next

```
08_IMPLEMENTATION_PART_6.md
```

Topics

- End-to-End Architecture
- Local Development Workflow
- Production Deployment
- Operational Checklist
- Security Checklist
- Performance Checklist
- MVP Readiness Checklist
- Implementation Summary
