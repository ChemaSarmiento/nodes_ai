# Explanation Engine
## Part 1 — AI Explanation Philosophy and System Boundaries

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 04_GRAPH_MODEL
- 05_AML_CASES
- 06_TRIGGER_ENGINE

---

# Purpose

This document specifies the architecture of the AI Explanation Engine.

The Explanation Engine is responsible for transforming structured investigation data into human-readable explanations.

It never generates AML cases.

It never computes graph features.

It never evaluates business rules.

It only explains.

---

# Philosophy

The LLM is **not** the investigation engine.

The investigation engine consists of:

```
Canonical Data

↓

Graph

↓

Features

↓

Timeline

↓

Evidence

↓

Explanation Packet
```

Only after these steps is the LLM invoked.

---

# Fundamental Principle

The LLM never discovers evidence.

The LLM explains evidence.

---

# AI Boundary

The AI layer starts here:

```
Explanation Packet

↓

Prompt Builder

↓

LLM

↓

Narrative

↓

Workbench
```

The AI layer ends before analyst interaction.

---

# Why Not Graph RAG?

The AI AML Layer intentionally avoids allowing the LLM to query the graph directly.

Instead:

```
Trigger

↓

SQL

↓

Graph

↓

Packet

↓

LLM
```

Advantages

- deterministic
- reproducible
- auditable
- cheaper
- easier to validate
- less hallucination

---

# AI Responsibilities

The LLM is responsible for

- summarization
- explanation
- evidence organization
- narrative generation
- missing evidence identification
- investigation guidance

The LLM is NOT responsible for

- AML detection
- graph traversal
- SQL
- rule evaluation
- scoring
- disposition
- regulatory decisions

---

# Human Boundary

The analyst remains responsible for

```
Review

↓

Disposition

↓

Escalation

↓

SAR Decision
```

The AI never performs these actions.

---

# Explanation Levels

Level 1

Executive Summary

Example

```
Repeated deposit-withdrawal cycles detected.
```

---

Level 2

Business Explanation

```
Three deposit/withdrawal cycles
were identified within 30 days.

The total withdrawal exceeded
the customer's expected profile.
```

---

Level 3

Evidence

```
Timeline

Graph Paths

Amounts

Contracts

Accounts
```

---

Level 4

Investigation Guidance

```
Recommended next steps

Missing evidence

Related entities
```

---

# Explainability Principles

Every explanation must satisfy

```
Correct

↓

Complete

↓

Traceable

↓

Auditable
```

---

# Explanation Sources

Allowed

```
Explanation Packet

↓

Timeline

↓

Graph Paths

↓

Features
```

Forbidden

```
SQL

Redshift

Athena

NetworkX

Business Tables
```

The LLM never sees raw operational data.

---

# Prompt Philosophy

Prompts are templates.

Never constructed ad hoc.

Every prompt is versioned.

Example

```
PRM_CASH_IN_OUT_V1

PRM_RRT_V1

PRM_SHARED_ACCOUNT_V1
```

---

# Packet Contract

The Explanation Packet is the only interface between analytics and AI.

Everything required by the LLM must exist inside the packet.

Nothing outside the packet may be assumed.

---

# Hallucination Policy

If information is absent

The model must answer

```
Insufficient evidence.

Additional investigation required.
```

Never infer

- ownership
- intent
- crime
- regulatory violations

without evidence.

---

# Regulatory Boundary

Future

Regulatory references will be added as

```
Reference Objects
```

The LLM never interprets regulation independently.

It only references curated regulatory objects.

---

# Output Types

Current

```
Narrative

Summary

Evidence

Missing Evidence

Recommendation
```

Future

```
Executive Summary

SAR Draft

EDD Draft

Investigation Plan

Related Cases
```

---

# Design Rules

Rule 1

The LLM only consumes Explanation Packets.

---

Rule 2

The LLM never reads operational databases.

---

Rule 3

The LLM never decides AML outcomes.

---

Rule 4

All generated text is reproducible through

- prompt version
- model version
- packet version

---

Rule 5

Every response must cite packet evidence internally.

---

# ADR

ADR-020

Explanation Boundary

Decision

Separate graph reasoning from language generation.

Reason

Improves auditability, reduces hallucinations and enables deterministic investigations.

---

# End of Part 1

Next

```
07_EXPLANATION_ENGINE_PART_2.md
```

Topics

- Explanation Packet Schema
- Prompt Builder
- Prompt Templates
- Model Selection
- Structured Outputs
- JSON Contracts
- Validation
- Response Parsing
