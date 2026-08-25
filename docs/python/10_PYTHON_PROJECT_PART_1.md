# Python Project
## Part 1 — Project Architecture, Standards and Package Layout

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- SQL Model
- Implementation Guide

---

# Purpose

This document defines the Python implementation of the AI AML Layer.

Unlike previous documents, this specification is code-oriented.

Everything described here should eventually exist in the repository.

---

# Python Philosophy

Python is responsible for

- Canonical processing
- Trigger execution
- Graph construction
- Feature extraction
- Packet generation
- API exposure

Python is NOT responsible for

- storing business data
- replacing Redshift
- replacing Athena

---

# Repository

```
python/

src/

tests/

examples/

requirements.txt

pyproject.toml
```

---

# Source Layout

```
src/

config/

core/

canonical/

graph/

triggers/

features/

packets/

llm/

api/

utils/

workers/
```

Every package owns one responsibility.

---

# config/

Purpose

Configuration loading.

Contains

```
settings.py

loader.py

schemas.py
```

No business logic.

---

# core/

Purpose

Common abstractions.

Contains

```
models.py

exceptions.py

logging.py

constants.py
```

Everything reusable.

---

# canonical/

Purpose

Canonical builders.

Contains

```
client_builder.py

contract_builder.py

movement_builder.py

edge_builder.py

node_builder.py
```

Produces canonical entities.

---

# graph/

Purpose

NetworkX implementation.

Contains

```
graph_builder.py

subgraph_builder.py

timeline.py

paths.py

serialization.py
```

No trigger logic.

---

# triggers/

Purpose

Trigger execution.

Contains

```
registry.py

runner.py

queue.py

config.py

validators.py
```

No graph implementation.

---

# features/

Purpose

Graph feature extraction.

Contains

```
financial.py

relationship.py

behavior.py

structural.py

aml.py
```

Returns dictionaries only.

---

# packets/

Purpose

Explanation Packet construction.

Contains

```
builder.py

schema.py

validator.py

serializer.py
```

No LLM calls.

---

# llm/

Purpose

LLM abstraction.

Contains

```
router.py

providers.py

prompt_builder.py

response_parser.py

guardrails.py
```

Everything provider-independent.

---

# api/

Purpose

Expose REST endpoints.

Contains

```
app.py

routers/

middleware/

dependencies/
```

Framework

```
FastAPI
```

---

# workers/

Purpose

Background execution.

Contains

```
trigger_worker.py

packet_worker.py

replay_worker.py
```

Stateless.

---

# utils/

Contains

```
hash.py

dates.py

json.py

files.py

metrics.py
```

Shared helpers only.

---

# Dependency Direction

Allowed

```
API

↓

Workers

↓

Triggers

↓

Graph

↓

Canonical

↓

Core
```

Forbidden

```
Graph

↓

API
```

or

```
LLM

↓

Canonical
```

---

# Object Model

Core classes

```
CanonicalNode

CanonicalEdge

CanonicalEvent

AMLCase

ExplanationPacket

TriggerExecution
```

Every object uses Pydantic.

---

# Configuration

Loaded once.

```
settings.yaml

↓

Pydantic

↓

Singleton
```

Never read YAML repeatedly.

---

# Logging

Every module receives

```
logger
```

Never create independent loggers.

---

# Error Handling

Custom exceptions

```
ConfigurationError

GraphError

PacketError

TriggerError

ValidationError
```

Never raise generic Exception.

---

# Coding Standards

Formatting

```
black
```

Imports

```
isort
```

Typing

```
mypy
```

Lint

```
ruff
```

Documentation

```
Google Style Docstrings
```

---

# Testing

Every module has

```
Unit Test

Integration Test
```

Target

```
>90% coverage
```

Business logic

```
100%
```

---

# Design Rules

Rule 1

One module

↓

One responsibility.

---

Rule 2

Business logic never lives in FastAPI.

---

Rule 3

Graph logic never calls LLM.

---

Rule 4

Everything typed.

---

Rule 5

Configuration external.

---

# ADR

ADR-034

Python Project Layout

Decision

Organize the Python implementation into domain-oriented packages with strict dependency direction.

Reason

Improves maintainability, testing and future scalability.

---

# End of Part 1

Next

```
10_PYTHON_PROJECT_PART_2.md
```

Topics

- Canonical Builders
- Trigger Runner
- Queue Consumer
- Graph Builder Classes
- Feature Classes
- Packet Builder Classes
- Dependency Injection
