# Python Project
## Part 4 — API Layer, Trigger Workers, Dependency Injection and Observability

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Python Project Part 3
- Trigger Engine
- Explanation Engine

---

# Purpose

This document defines the service layer of the AI AML Layer.

The API layer exposes functionality.

The Worker layer executes background processing.

The infrastructure layer provides logging,
configuration,
metrics and dependency injection.

---

# High Level Architecture

```
FastAPI

↓

Routers

↓

Services

↓

Repositories

↓

Graph Engine

↓

Packet Builder

↓

LLM
```

---

# FastAPI Philosophy

FastAPI only exposes APIs.

FastAPI never contains business logic.

Responsibilities

```
Authentication

Validation

Routing

Serialization
```

Everything else belongs to services.

---

# API Layout

```
api/

routers/

dependencies/

middleware/

schemas/

responses/
```

---

# Routers

Suggested routers

```
clients.py

contracts.py

cases.py

graphs.py

packets.py

triggers.py

health.py
```

---

# Example Endpoints

```
GET

/clients/{client_id}
```

Returns

Client Summary

---

```
GET

/cases/{case_id}
```

Returns

AML Case

---

```
GET

/cases/{case_id}/graph
```

Returns

Graph JSON

---

```
GET

/cases/{case_id}/packet
```

Returns

Explanation Packet

---

```
POST

/triggers/execute
```

Executes

Manual Trigger

---

```
POST

/triggers/replay
```

Starts replay.

---

# Service Layer

Each router calls one service.

```
CaseService

GraphService

TriggerService

PacketService

LLMService
```

Services coordinate business logic.

---

# Repository Layer

Repositories abstract persistence.

```
ClientRepository

ContractRepository

MovementRepository

PacketRepository

CaseRepository
```

No SQL inside services.

---

# Dependency Injection

Every service is injected.

Example

```python
CaseService(
    graph_service,
    packet_service,
    repository
)
```

Never instantiate dependencies manually.

---

# Trigger Worker

Consumes

```
Trigger Queue
```

Pipeline

```
Queue

↓

SQL

↓

Graph

↓

Packet

↓

Publish
```

No HTTP.

---

# LLM Worker

Consumes

```
Packet Queue
```

Pipeline

```
Packet

↓

Prompt

↓

LLM

↓

Validation

↓

Persistence
```

---

# Replay Worker

Purpose

Replay historical investigations.

Pipeline

```
Historical Events

↓

Trigger

↓

Packet

↓

Persist
```

Never overwrite.

Always version.

---

# Worker Registration

Each worker registers

```
worker_id

worker_version

capabilities
```

Supports monitoring.

---

# Configuration

Loaded once.

```
application.yaml

↓

Settings

↓

Injected Everywhere
```

Never read YAML repeatedly.

---

# Logging

Every request contains

```
request_id

trigger_run_id

packet_id

case_id

user

service
```

Structured JSON.

---

# Correlation IDs

Every execution has

```
request_id

↓

trigger_run_id

↓

packet_id

↓

llm_request_id
```

Everything traceable.

---

# Error Handling

Business Errors

```
ValidationError

TriggerError

PacketError

GraphError
```

Infrastructure Errors

```
Database

Queue

Network

LLM Provider
```

Separated.

---

# Middleware

Current

```
Logging

Timing

Exception Handling

Request IDs
```

Future

```
Rate Limiting

Authentication

Tracing
```

---

# Health Checks

Endpoints

```
/health

/readiness

/liveness
```

Checks

```
Database

Queue

LLM

Configuration
```

---

# Metrics

Expose

```
Prometheus

/OpenMetrics
```

Current metrics

```
Request Count

Latency

Worker Count

Packets

Graph Builds

LLM Calls
```

---

# Observability

Recommended stack

```
CloudWatch

Grafana

Prometheus

OpenTelemetry
```

---

# Security

Never expose

```
Canonical Tables

Graph Objects

Prompt Templates
```

Public APIs expose only

```
Cases

Packets

Graphs

Metrics
```

---

# Rate Limits

Protect

```
Replay

Manual Trigger

LLM Calls
```

Avoid accidental overload.

---

# Deployment

Each service runs independently.

```
FastAPI

↓

Container

↓

ECS

↓

ALB
```

Workers

↓

Independent containers.

---

# Local Development

Run

```
API

+

Workers

+

Streamlit
```

using

```
docker-compose
```

Future

```
Tilt

Dev Containers
```

---

# Testing

API

↓

Integration Tests

Workers

↓

Functional Tests

Services

↓

Unit Tests

---

# Design Rules

Rule 1

Routers never contain business logic.

---

Rule 2

Repositories never contain graph logic.

---

Rule 3

Workers never expose HTTP.

---

Rule 4

Dependency Injection everywhere.

---

Rule 5

Everything observable.

---

# ADR

ADR-037

Service-Oriented Python Architecture

Decision

Expose the AI AML Layer through stateless FastAPI services and background workers with dependency injection.

Reason

Allows independent scaling, clean separation of concerns and cloud-native deployment.

---

# End of Part 4

Next

```
10_PYTHON_PROJECT_PART_5.md
```

Topics

- Testing Strategy
- Unit Tests
- Integration Tests
- Replay Tests
- Benchmarking
- Mock Data
- Local Dataset
- Development Workflow
