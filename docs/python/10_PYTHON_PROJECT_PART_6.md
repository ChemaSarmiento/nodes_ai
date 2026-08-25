# Python Project
## Part 6 — Packaging, Deployment, Release Management and Project Summary

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Python Project Parts 1–5

---

# Purpose

This document closes the Python implementation specification.

It defines:

- Packaging
- Docker
- Development Environment
- Releases
- Versioning
- Repository Standards
- Future Evolution

---

# Repository Structure

```
python/

src/

config/

tests/

examples/

requirements.txt

pyproject.toml

Dockerfile

docker-compose.yml

Makefile

README.md
```

---

# Python Package

Recommended

```
src/

ai_aml_layer/

    canonical/

    graph/

    features/

    triggers/

    packets/

    llm/

    api/

    workers/

    repositories/

    utils/
```

Everything importable as

```python
from ai_aml_layer.graph import GraphBuilder
```

---

# pyproject.toml

Package metadata

```
name

version

dependencies

python_version
```

Preferred build system

```
setuptools
```

or

```
poetry
```

---

# Requirements

Separate

```
requirements.txt

requirements-dev.txt

requirements-test.txt
```

Avoid one large dependency list.

---

# Makefile

Recommended commands

```
make install

make test

make lint

make format

make run

make docker

make clean
```

Developers should never remember long commands.

---

# Docker

Every service has one image.

Examples

```
graph-service

trigger-worker

packet-worker

llm-worker

api
```

Each image has one responsibility.

---

# Dockerfile

Stages

```
Builder

↓

Runtime
```

Use multi-stage builds.

Never include

```
tests

examples

git history
```

inside runtime image.

---

# docker-compose

Local environment

```
API

↓

Workers

↓

Redis (future)

↓

Mock Queue

↓

Local Storage
```

Should allow

```
docker compose up
```

to start the entire platform.

---

# Dev Containers

Recommended

```
.devcontainer/

devcontainer.json
```

Allows identical development environments.

---

# Environment Profiles

```
local

development

qa

production
```

Every profile has

```
settings

logging

LLM

storage

queue
```

---

# Release Process

```
Develop

↓

Pull Request

↓

Tests

↓

Merge

↓

Tag

↓

Release

↓

Deploy
```

Never deploy from feature branches.

---

# Semantic Versioning

Current

```
Major.Minor.Patch
```

Example

```
1.0.0
```

Major

```
Breaking architecture
```

Minor

```
New capability
```

Patch

```
Bug fix
```

---

# Component Versioning

Track independently

```
Canonical Model

Trigger Engine

Graph

Packets

Prompts

LLM

API
```

Example

```
graph_version

packet_version

prompt_version

trigger_version
```

---

# Release Checklist

Before release

- Unit Tests
- Integration Tests
- Replay Tests
- Documentation
- ADR Updated
- Configuration Reviewed
- Security Scan
- Docker Build

---

# Documentation

Every module requires

```
README

Examples

Docstrings
```

Every public class documented.

---

# Coding Standards

Mandatory

```
Type Hints

Pydantic

Dataclasses

Google Docstrings
```

No undocumented public methods.

---

# Package Boundaries

Allowed

```
API

↓

Services

↓

Repositories

↓

Canonical

↓

Graph
```

Forbidden

```
Graph

↓

FastAPI
```

```
LLM

↓

Canonical
```

```
Workers

↓

UI
```

---

# Performance Targets

```
Graph

<2 sec
```

```
Packet

<500 ms
```

```
Trigger

<2 sec
```

```
API

<300 ms
```

LLM excluded.

---

# Local Dataset

Repository contains

```
examples/

golden_dataset/
```

Every developer can execute

```
python run.py
```

without Redshift.

---

# Cookiecutter Template

Future

Generate projects using

```
cookiecutter

↓

AI AML Layer Template
```

Provides

- folders
- configs
- Docker
- CI
- tests

---

# CI/CD

Preferred

```
GitHub Actions
```

Stages

```
Lint

↓

Tests

↓

Docker

↓

Security

↓

Deploy
```

---

# Observability

Every service exposes

```
Health

Metrics

Version

Build

Configuration
```

---

# Future Evolution

Current

```
NetworkX

FastAPI

Streamlit
```

Future

```
Neo4j

React

Graphistry

LangGraph

Feature Store

Streaming
```

Business architecture remains unchanged.

---

# Python Project Summary

The Python implementation consists of

```
Part 1

Architecture

--------------

Part 2

Builders

Trigger Runner

--------------

Part 3

Graph Engine

--------------

Part 4

API

Workers

--------------

Part 5

Testing

--------------

Part 6

Packaging

Deployment

Release
```

Together these documents define the complete Python implementation of the AI AML Layer.

---

# ADR

ADR-039

Python Reference Architecture

Decision

Implement the AI AML Layer as a modular Python platform with strict separation between canonical processing, graph computation, AI explanation and service exposure.

Reason

Improves maintainability, scalability and future migration to distributed architectures.

---

# Python Project Complete

Next document

```
docs/roadmap/11_ROADMAP_PART_1.md
```

Topics

- 3-Month MVP
- Weekly Deliverables
- Milestones
- Owners
- Dependencies
- Critical Path
- Success Criteria
