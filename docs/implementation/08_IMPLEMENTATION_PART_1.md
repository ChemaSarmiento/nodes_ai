# Implementation Guide
## Part 1 — Repository Structure, Standards and Technology Stack

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Architecture
- Graph Model
- Trigger Engine
- Explanation Engine

---

# Purpose

This document defines the engineering implementation standards of the AI AML Layer.

Unlike previous documents, this one is implementation-oriented.

It defines:

- repository organization
- coding standards
- environments
- dependencies
- deployment strategy

---

# Repository Philosophy

The repository is divided into six major domains.

```
Documentation

Configuration

Data

Application

Infrastructure

Testing
```

No domain should depend directly on another without an explicit contract.

---

# Repository Structure

```
nodes_ai/

│
├── README.md
├── LICENSE
├── .gitignore
│
├── docs/
│
├── config/
│
├── python/
│
├── sql/
│
├── diagrams/
│
├── prompts/
│
├── notebooks/
│
├── examples/
│
├── tests/
│
└── scripts/
```

---

# Documentation

```
docs/

architecture/

business/

graph/

implementation/

adr/

roadmap/
```

Contains only documentation.

Never executable code.

---

# Python

```
python/

src/

tests/

requirements.txt

pyproject.toml
```

All production code lives here.

---

# SQL

```
sql/

ddl/

canonical/

triggers/

analytics/

views/

validation/
```

One SQL file per logical responsibility.

---

# Prompts

```
prompts/

graph/

cases/

explanations/

llm/
```

Every prompt versioned.

Never embed prompts inside Python code.

---

# Configuration

```
config/

application.yaml

triggers.yaml

graph.yaml

llm.yaml

logging.yaml
```

No hardcoded configuration.

---

# Data

```
data/

input/

output/

tmp/
```

Ignored by Git.

Only examples belong in repository.

---

# Examples

```
examples/

packets/

graphs/

html/

screenshots/
```

Contains reproducible outputs.

---

# Tests

```
tests/

unit/

integration/

regression/

performance/
```

---

# Coding Standards

Language

```
Python 3.12+
```

Style

```
PEP-8
```

Formatting

```
black
```

Imports

```
isort
```

Lint

```
ruff
```

Typing

```
mypy
```

---

# Project Layers

```
Presentation

↓

Application

↓

Domain

↓

Infrastructure

↓

Persistence
```

Never skip layers.

---

# Technology Stack

## Storage

```
Redshift

Athena

S3
```

---

## Graph

```
NetworkX
```

Future

```
Neo4j

Neptune
```

---

## Queue

Preferred

```
SQS FIFO
```

Alternative

```
Kafka

RabbitMQ
```

---

## API

```
FastAPI
```

---

## UI

Prototype

```
Streamlit
```

Future

```
React

NextJS
```

---

## LLM

Abstraction Layer

Supports

```
OpenAI

Azure OpenAI

Bedrock

Claude

Gemini
```

No provider-specific logic inside business code.

---

# Python Dependencies

Core

```
pandas

networkx

numpy

pyarrow

pydantic

pyyaml
```

Visualization

```
pyvis

plotly
```

API

```
fastapi

uvicorn
```

Future

```
langgraph

mlflow

evidently
```

---

# Development Environment

Recommended

```
VSCode

Python

Docker

Git
```

---

# Local Execution

```
python run.py
```

or

```
make run
```

Future

```
docker compose up
```

---

# Logging

Library

```
logging
```

Format

JSON

Fields

```
timestamp

service

trigger_run_id

case_id

level

message
```

---

# Environment Variables

```
REDIS_URL

REDSHIFT_URL

ATHENA_DATABASE

S3_BUCKET

OPENAI_API_KEY

AWS_REGION
```

Never commit secrets.

---

# Git Strategy

Main

```
main
```

Development

```
develop
```

Feature

```
feature/*
```

Documentation

```
docs/*
```

---

# Commit Convention

```
feat:

fix:

docs:

refactor:

test:

chore:
```

Example

```
feat(graph):
add repeated round trip detector
```

---

# Branch Policy

One issue

↓

One branch

↓

One Pull Request

↓

One merge

---

# Design Rules

Rule 1

Configuration outside code.

Rule 2

Prompts outside code.

Rule 3

SQL outside code.

Rule 4

Documentation first.

Rule 5

Everything versioned.

---

# ADR

ADR-024

Repository Organization

Decision

Documentation, SQL, prompts and application code are separated into independent domains.

Reason

Improves maintainability, onboarding and future evolution.

---

# End of Part 1

Next

```
08_IMPLEMENTATION_PART_2.md
```

Topics

- Canonical Data Pipeline
- ETL
- Bronze / Silver / Gold
- Data Contracts
- Data Quality
- Incremental Loads
- Change Detection
- Canonical Tables
