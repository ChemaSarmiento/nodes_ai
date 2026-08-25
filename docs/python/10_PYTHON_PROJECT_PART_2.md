# Python Project
## Part 2 — Canonical Builders, Trigger Runner and Dependency Injection

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Python Project Part 1
- Canonical Data Model
- Trigger Engine

---

# Purpose

This document defines the core implementation classes.

Every business object in the AI AML Layer is constructed through dedicated builders.

No class should perform more than one business responsibility.

---

# High-Level Object Flow

```
CSV / Redshift

↓

Canonical Builders

↓

Canonical Objects

↓

Trigger Runner

↓

Graph Builder

↓

Feature Builder

↓

Packet Builder

↓

LLM Worker
```

---

# Canonical Builder Pattern

Every entity follows the same lifecycle.

```
Raw Record

↓

Normalize

↓

Validate

↓

Canonical Object

↓

Persist
```

No builder should access NetworkX.

---

# Base Builder

Every builder inherits from

```python
CanonicalBuilder
```

Responsibilities

```
Normalize

Validate

Generate Business Key

Generate Canonical ID

Return Pydantic Model
```

---

# ClientBuilder

Input

```
gold_clients
```

Output

```
ClientNode
```

Methods

```python
build()

validate()

normalize()

canonical_id()
```

---

# ContractBuilder

Input

```
gold_contracts
```

Output

```
ContractNode
```

Additional Responsibilities

```
Resolve top_contract

Create ContractGroup if required
```

---

# KYCBuilder

Produces

```
KYCProfileNode
```

Version

```
client

↓

review_date

↓

profile_version
```

History preserved.

---

# CashMovementBuilder

Input

```
Deposits

Withdrawals
```

Output

```
CashMovementNode
```

Responsibilities

```
Generate deterministic movement_id

Normalize accounts

Normalize institutions
```

---

# ExternalAccountBuilder

Produces

```
ExternalBankAccount
```

Deduplicates

```
account_hash
```

Never creates duplicates.

---

# InstitutionBuilder

Creates

```
FinancialInstitution
```

Referenced by

```
ExternalBankAccount
```

---

# EdgeBuilder

Input

```
Canonical Nodes
```

Output

```
Canonical Edges
```

No graph.

Only relationships.

---

# Trigger Runner

Purpose

Coordinates trigger execution.

Pipeline

```
Load Trigger

↓

Load Configuration

↓

Validate

↓

Execute SQL

↓

Build Graph

↓

Build Packet

↓

Publish
```

---

# TriggerRunner Class

Suggested methods

```python
run()

validate()

load_configuration()

execute()

publish()
```

---

# Queue Consumer

Consumes

```
Trigger Message
```

Produces

```
Trigger Execution
```

No business logic.

---

# Trigger Execution Context

Every execution contains

```
trigger_run_id

trigger_id

configuration

entity

timestamps
```

Immutable.

---

# Graph Builder Interface

```python
GraphBuilder
```

Methods

```python
build()

validate()

build_subgraph()

serialize()
```

GraphBuilder never loads SQL.

---

# Feature Builder Interface

```python
FeatureBuilder
```

Methods

```python
financial()

relationship()

behavior()

aml()

structural()
```

Output

```
dict
```

Only.

---

# Packet Builder Interface

```python
PacketBuilder
```

Methods

```python
build()

validate()

serialize()
```

Returns

```
ExplanationPacket
```

---

# Dependency Injection

Use constructor injection.

Example

```python
TriggerRunner(

graph_builder,

feature_builder,

packet_builder,

sql_repository
)
```

Never instantiate dependencies inside business classes.

---

# Repository Pattern

SQL access goes through repositories.

Examples

```python
ClientRepository

ContractRepository

MovementRepository

CaseRepository
```

Repositories hide SQL implementation.

---

# Configuration Injection

Every service receives

```python
Settings
```

No global variables.

No singleton business objects.

---

# Validation Layer

Validation occurs

```
Before Build

Before Persist

Before Publish
```

Never after packet generation.

---

# Exception Strategy

Business Exceptions

```
CanonicalValidationError

TriggerConfigurationError

GraphConstructionError

PacketValidationError
```

Infrastructure Exceptions

```
DatabaseError

QueueError

IOError
```

Keep separate.

---

# Unit Testing Strategy

Every Builder

↓

One Test File

Example

```
test_client_builder.py

test_trigger_runner.py

test_graph_builder.py
```

One business rule per test.

---

# Object Lifetime

```
Builder

Short-lived

↓

Graph

Ephemeral

↓

Packet

Persistent
```

No long-lived graph objects.

---

# Design Rules

Rule 1

Builders create objects.

Never graphs.

---

Rule 2

Repositories access data.

Never business logic.

---

Rule 3

TriggerRunner orchestrates.

Never computes features.

---

Rule 4

PacketBuilder never calls LLM.

---

Rule 5

Dependency Injection everywhere.

---

# ADR

ADR-035

Builder Pattern

Decision

Every canonical entity is created through dedicated builders and injected into higher-level services.

Reason

Improves modularity, testing and future maintainability.

---

# End of Part 2

Next

```
10_PYTHON_PROJECT_PART_3.md
```

Topics

- GraphBuilder implementation
- NetworkX wrappers
- Timeline Builder
- Path Builder
- Feature Extractors
- Packet Builder implementation
- Serialization
