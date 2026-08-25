# Python Project
## Part 5 — Testing Strategy, Quality Assurance and Development Workflow

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- Python Project Parts 1–4

---

# Purpose

This document defines the software quality strategy for the AI AML Layer.

Unlike previous sections, this document specifies how the platform is validated before reaching production.

---

# Testing Philosophy

The AI AML Layer is a deterministic platform.

The following must always produce identical outputs:

```
Same Canonical Data

↓

Same Trigger

↓

Same Graph

↓

Same Features

↓

Same Packet
```

Only the LLM narrative may change between model versions.

Everything else must remain reproducible.

---

# Testing Pyramid

```
                End-to-End
            ----------------
          Integration Testing
      --------------------------
        Unit Testing (largest)
```

Target

```
70%

Unit Tests

20%

Integration Tests

10%

End-to-End Tests
```

---

# Unit Tests

Every business module has unit tests.

Examples

```
test_client_builder.py

test_graph_builder.py

test_trigger_runner.py

test_packet_builder.py

test_feature_engine.py
```

---

# Unit Test Principles

Each unit test validates

```
One Function

One Rule

One Expected Result
```

Never test multiple business rules simultaneously.

---

# Canonical Builder Tests

Validate

```
Client Builder

↓

Correct Node

Correct Business Key

Correct Hash
```

Same for

```
Contracts

Movements

Edges

Events
```

---

# Trigger Tests

Each trigger must validate

```
Positive Case

Negative Case

Boundary Conditions
```

Example

```
Cash In Out

↓

6 days

↓

Case Created

---------

8 days

↓

No Case
```

---

# Graph Tests

Graph Builder validates

```
Correct Node Count

Correct Edge Count

Graph Connectivity

Traversal

Graph Serialization
```

---

# Feature Tests

Validate

```
Cash In Out Cycles

Repeated Round Trip

KYC Mismatch

Shared Accounts

Top Contract Flow
```

Every feature has deterministic expected values.

---

# Packet Tests

Packet validation

Checks

```
Required Fields

JSON Schema

Timeline

Graph Paths

Evidence
```

Never allow malformed packets.

---

# Integration Tests

Pipeline

```
Canonical

↓

Trigger

↓

Graph

↓

Features

↓

Packet
```

Entire flow executes.

No LLM required.

---

# Replay Tests

Historical datasets

↓

Replay Trigger

↓

Rebuild Packet

↓

Compare Packet

Packets should match.

---

# Golden Dataset

Repository should contain

```
examples/

golden_dataset/
```

Contains

```
Clients

Contracts

Deposits

Withdrawals

Expected Cases
```

Used for regression.

---

# Mock Data

Generate synthetic customers.

Examples

```
Normal Customer

↓

Cash In Out

↓

Repeated Round Trip

↓

Shared Account

↓

KYC Mismatch
```

Each dataset isolated.

---

# LLM Tests

LLM is validated separately.

Checks

```
JSON Output

Required Keys

Prompt Version

Response Length
```

Never validate AML correctness through LLM.

---

# Performance Tests

Current targets

```
Graph Build

<2 sec
```

```
Feature Extraction

<1 sec
```

```
Packet Build

<500 ms
```

LLM excluded.

---

# Load Tests

Simulate

```
100

500

1000

Concurrent Trigger Executions
```

Measure

```
Latency

Memory

Queue Growth
```

---

# Regression Tests

Every Pull Request runs

```
Golden Dataset

↓

Expected Output

↓

Comparison
```

Any change blocks merge.

---

# Static Analysis

Required

```
ruff

mypy

black

isort
```

Every PR.

---

# Security Tests

Validate

```
Secrets

Authentication

Authorization

Input Validation
```

No production secrets inside repository.

---

# CI Pipeline

```
Git Push

↓

Lint

↓

Unit Tests

↓

Integration Tests

↓

Regression

↓

Docker Build

↓

Deploy
```

Any failure

↓

Stop pipeline.

---

# Local Development Workflow

Developer

```
Pull

↓

Create Branch

↓

Implement

↓

Run Tests

↓

Commit

↓

Pull Request
```

---

# Pull Request Checklist

- Tests pass
- Documentation updated
- ADR required? (Yes/No)
- Configuration changed?
- Trigger version changed?
- Packet version changed?

---

# Coverage Goals

Business Logic

```
100%
```

Overall

```
>90%
```

Graph

```
>95%
```

Triggers

```
100%
```

---

# Benchmark Dataset

Maintain

```
10 Clients

100 Clients

1000 Clients
```

For repeatable benchmarking.

---

# Development Principles

Rule 1

Every bug gets a regression test.

---

Rule 2

Every trigger has boundary tests.

---

Rule 3

Every graph builder has deterministic outputs.

---

Rule 4

Golden datasets are immutable.

---

Rule 5

No Pull Request without tests.

---

# ADR

ADR-038

Testing Strategy

Decision

The AI AML Layer is validated through deterministic business testing rather than probabilistic AI evaluation.

Reason

Business correctness must not depend on model behavior.

---

# End of Part 5

Next

```
10_PYTHON_PROJECT_PART_6.md
```

Topics

- Complete Project Structure
- Cookiecutter
- Packaging
- Docker
- Dev Containers
- Makefile
- Release Process
- Python Project Summary
