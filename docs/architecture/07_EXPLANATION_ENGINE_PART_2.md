# Explanation Engine
## Part 2 — Explanation Packet, Prompt Builder and Structured Outputs

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 07_EXPLANATION_ENGINE_PART_1
- 04_GRAPH_MODEL
- 05_AML_CASES

---

# Purpose

This document specifies the interface between the analytics platform and the Large Language Model.

The interface is called

```
Explanation Packet
```

It is the single contract consumed by AI.

---

# Why an Explanation Packet?

Without a packet

```
LLM

↓

Graph

↓

SQL

↓

Tables

↓

Documents
```

Problems

- hallucinations
- non reproducible
- impossible auditing
- prompt instability

---

With a packet

```
Graph

↓

Packet

↓

LLM
```

Everything becomes deterministic.

---

# Packet Principles

The packet is

- immutable
- deterministic
- versioned
- structured
- machine readable

The packet is NOT

- prose
- prompt
- report

---

# Packet Lifecycle

```
Graph

↓

Feature Builder

↓

Timeline

↓

Packet Builder

↓

Packet

↓

Prompt Builder

↓

LLM
```

---

# Packet Schema

Every packet contains the following sections.

```
Metadata

Case

Graph

Timeline

Features

Evidence

Missing Evidence

Recommendation

Prompt Context
```

---

# Metadata

```json
{
    "packet_id":"",
    "packet_version":"1.0",
    "graph_version":"1.0",
    "trigger_run_id":"",
    "generated_at":"",
    "model_target":"GPT"
}
```

---

# Case Section

```json
{
    "case_id":"",
    "case_type":"",
    "severity":"",
    "status":"",
    "client_id":"",
    "contract_id":"",
    "top_contract":""
}
```

---

# Timeline Section

Ordered list

```json
[
    {
        "timestamp":"",
        "event":"Deposit",
        "amount":100000
    },
    {
        "timestamp":"",
        "event":"Withdrawal",
        "amount":98000
    }
]
```

The timeline is always chronological.

---

# Graph Section

Contains graph summaries.

Never raw graph objects.

```json
{
    "nodes":18,
    "edges":24,
    "paths":[
        "...",
        "..."
    ]
}
```

---

# Graph Paths

Stored as business paths.

Example

```
External Account

↓

Deposit

↓

Contract

↓

Withdrawal

↓

External Account
```

Never

```
Node IDs

Hashes

Internal references
```

---

# Feature Section

Example

```json
{
    "cash_in_out_cycles":3,
    "withdrawal_ratio":0.91,
    "shared_accounts":1,
    "contracts":2,
    "top_contract":"..."
}
```

Features must already be computed.

LLM never computes features.

---

# Evidence Section

Current

```
Cash Movements

Contracts

KYC

External Accounts
```

Future

```
Orders

Executions

Watchlists

Devices

Geo

Documents
```

Every evidence item has

```
source

timestamp

confidence
```

---

# Missing Evidence

Very important.

Example

```json
[
    "Destination account ownership unknown",
    "No login history available",
    "No device information available"
]
```

LLM must explicitly mention missing evidence.

---

# Recommendation

Current

```
Review

Monitor

Escalate
```

Future

```
EDD

SAR Candidate

Freeze

Manual Verification
```

Recommendation is advisory only.

---

# Prompt Context

Prompt Builder appends

```
Case Type

Prompt Version

Language

Output Schema
```

No business data is added here.

---

# Prompt Builder

Purpose

Convert packet into model prompt.

Flow

```
Packet

↓

Prompt Template

↓

Rendered Prompt

↓

LLM
```

---

# Prompt Templates

Versioned.

Examples

```
PRM_CASH_IN_OUT_V1

PRM_RRT_V1

PRM_KYC_MISMATCH_V1

PRM_SHARED_ACCOUNT_V1
```

---

# Prompt Inputs

Prompt receives

```
Packet

Prompt Configuration
```

Never

```
SQL

Graph

Database
```

---

# Structured Outputs

Every response must follow JSON.

Example

```json
{
    "summary":"",
    "why_triggered":[],
    "evidence":[],
    "missing_evidence":[],
    "recommendation":"",
    "limitations":[]
}
```

No free-form responses.

---

# Validation

Before sending to Workbench

Validate

```
Required Keys

JSON

Schema

Length

Null Fields
```

Reject malformed responses.

---

# Response Parser

Responsibilities

```
Validate

Normalize

Persist
```

Never modify meaning.

---

# Output Length

Target

Executive Summary

```
<150 words
```

Analyst Narrative

```
300-800 words
```

Configurable.

---

# Language

Supported

```
Spanish

English
```

Future

```
Portuguese
```

Language chosen by Workbench.

---

# Versioning

Every explanation stores

```
packet_version

prompt_version

model_version

response_schema_version
```

This guarantees reproducibility.

---

# Design Rules

Rule 1

LLM consumes packets only.

---

Rule 2

Packet is immutable.

---

Rule 3

Prompt templates are versioned.

---

Rule 4

Responses are structured.

---

Rule 5

Every explanation is reproducible.

---

# ADR

ADR-021

Explanation Packet Contract

Decision

The Explanation Packet is the only interface between analytics and generative AI.

Reason

Separates deterministic analytics from probabilistic language generation.

---

# End of Part 2

Next

```
07_EXPLANATION_ENGINE_PART_3.md
```

Topics

- LLM Worker
- Multi-model strategy
- Model routing
- Guardrails
- Regulatory references
- Confidence estimation
- Cost optimization
- Prompt orchestration
