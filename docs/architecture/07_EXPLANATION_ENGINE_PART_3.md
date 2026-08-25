# Explanation Engine
## Part 3 — LLM Worker, Model Routing and Guardrails

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 07_EXPLANATION_ENGINE_PART_1
- 07_EXPLANATION_ENGINE_PART_2

---

# Purpose

This document defines the execution architecture of the LLM Worker.

The worker is responsible for converting a validated Explanation Packet into a structured explanation.

The worker does not perform AML analysis.

It only performs controlled language generation.

---

# Worker Architecture

```
Explanation Packet

↓

Prompt Builder

↓

Prompt Template

↓

LLM

↓

JSON Response

↓

Validator

↓

Workbench
```

The worker has no access to

- SQL
- Graph
- Redshift
- Athena
- NetworkX

---

# LLM Worker Responsibilities

Allowed

- Summarize
- Explain
- Organize evidence
- Produce narratives
- Identify missing evidence
- Suggest investigation steps

Forbidden

- Execute SQL
- Traverse graphs
- Infer facts not present
- Modify packet
- Decide AML outcome

---

# Model Routing

The architecture supports multiple LLMs.

```
Packet

↓

Router

↓

Model

↓

Structured Output
```

---

# Suggested Routing

Executive Summary

↓

Small model

---

Analyst Narrative

↓

Large reasoning model

---

Future

Regulatory Draft

↓

Compliance-tuned model

---

# Model Registry

Every model has metadata.

```
model_id

provider

version

context_window

cost

status
```

Example

```
GPT-5

Claude

Gemini

Internal Model
```

---

# Prompt Routing

Prompt selection depends on

```
Case Type

↓

Prompt Version

↓

Language

↓

Output Type
```

Example

```
AML_REPEATED_ROUND_TRIP

↓

PRM_RRT_V2

↓

Spanish

↓

Analyst Narrative
```

---

# Prompt Layers

Each prompt consists of

```
System Instructions

↓

Packet

↓

Formatting Rules

↓

Output Schema
```

Business logic never appears in prompts.

---

# Output Contract

The worker must always return JSON.

Example

```json
{
    "summary":"",
    "business_reason":"",
    "timeline_summary":"",
    "key_findings":[],
    "evidence":[],
    "missing_evidence":[],
    "recommended_next_step":"",
    "limitations":[]
}
```

---

# Guardrails

The worker must reject responses that

- invent entities
- invent transactions
- invent evidence
- invent regulations
- contradict packet contents

---

# Hallucination Policy

If evidence is missing

Return

```
Insufficient evidence to support this conclusion.
```

Never guess.

---

# Regulatory References

Future

Packets may include

```
Regulatory Objects
```

Example

```json
{
    "reference_id":"MX-LFV-001",
    "title":"Customer Due Diligence"
}
```

The worker may quote

Reference IDs

Never raw regulation.

---

# Confidence

The worker reports confidence only for

Language quality

Never for AML risk.

Example

```json
{
    "response_confidence":"HIGH"
}
```

AML confidence comes from analytics.

---

# Cost Optimization

Preferred strategy

```
Small model

↓

Simple cases

Large model

↓

Complex cases
```

Examples

```
Cash In Out

↓

Small
```

```
Composite Case

↓

Large
```

---

# Token Optimization

Never send

- SQL
- Graph JSON
- Node IDs
- Edge IDs

Send only

```
Summary

Timeline

Features

Evidence

Paths
```

---

# Context Window

Maximum packet size

```
~8K tokens
```

If packet exceeds

Summarize graph before prompting.

---

# Prompt Versioning

Store

```
prompt_id

prompt_version

model_id

model_version
```

Every explanation is reproducible.

---

# Validation

Before Workbench

Validate

```
JSON

Required Keys

Length

Language

Schema
```

Reject invalid outputs.

---

# Failure Handling

Possible failures

```
Timeout

Rate Limit

Invalid JSON

Provider Failure
```

Recovery

```
Retry

↓

Fallback Model

↓

Manual Review
```

---

# Fallback Strategy

Priority

```
Primary Model

↓

Secondary Model

↓

No AI

↓

Human Investigation
```

System must continue operating.

---

# Metrics

Track

```
Latency

Tokens

Cost

Retries

Fallbacks

Validation Failures
```

Business metrics are separate.

---

# Design Rules

Rule 1

LLM only explains.

---

Rule 2

Analytics determines facts.

---

Rule 3

Packets are immutable.

---

Rule 4

Outputs are structured.

---

Rule 5

Human remains final authority.

---

# ADR

ADR-022

LLM Worker

Decision

The LLM Worker is a stateless language generation service that consumes Explanation Packets and returns validated structured narratives.

Reason

Allows replacing models without changing analytics or investigation logic.

---

# End of Part 3

Next

```
07_EXPLANATION_ENGINE_PART_4.md
```

Topics

- Regulatory Reference Layer
- Human Feedback Loop
- Explanation Quality Metrics
- Explainability Evaluation
- A/B Testing
- Prompt Evolution
- Future AI Agents
- Multi-Agent Architecture
