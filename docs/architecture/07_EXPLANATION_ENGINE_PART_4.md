# Explanation Engine
## Part 4 — Regulatory Layer, Human Feedback and Multi-Agent Evolution

---
Version: 1.0

Status: Draft

Owner: AI AML Layer

Depends on

- 07_EXPLANATION_ENGINE_PART_1
- 07_EXPLANATION_ENGINE_PART_2
- 07_EXPLANATION_ENGINE_PART_3

---

# Purpose

This document defines the final layer of the Explanation Engine.

Topics

- Regulatory References
- Human Feedback
- Explainability Metrics
- Prompt Evaluation
- Continuous Learning
- Future Multi-Agent Architecture

---

# Regulatory Reference Layer

The AI AML Layer does NOT expose entire regulations to the LLM.

Instead it exposes curated reference objects.

---

# Why?

Regulations are

- large
- changing
- difficult to version
- difficult to audit

Instead, AI receives only approved references.

---

# Regulatory Object

Every reference becomes a structured object.

Example

```json
{
  "reference_id":"AML-MX-001",
  "title":"Customer Due Diligence",
  "version":"2026-01",
  "owner":"Compliance",
  "status":"Approved"
}
```

The packet references

```
reference_id
```

never raw regulation.

---

# Regulatory Workflow

```
Compliance

↓

Reference Objects

↓

Packet Builder

↓

LLM
```

LLM never searches regulations.

---

# Human Feedback

Every explanation receives analyst feedback.

Stored fields

```
Accepted

Rejected

Edited

Commented
```

---

# Feedback Categories

```
Narrative Quality

Evidence Completeness

Explanation Clarity

Recommendation Quality

Missing Evidence
```

---

# Feedback Lifecycle

```
LLM

↓

Workbench

↓

Analyst

↓

Feedback

↓

Metrics
```

Future

↓

Prompt Improvement

---

# Explainability Metrics

Current

```
Narrative Length

Packet Completeness

JSON Validity

Missing Evidence Count
```

Future

```
Analyst Score

Acceptance Rate

Editing Distance

Time Saved
```

---

# Quality Metrics

Track

```
Packets Generated

Packets Accepted

Packets Edited

Packets Rejected

Average Review Time

Average Reading Time
```

---

# Prompt Evaluation

Every prompt is evaluated.

Metrics

```
Acceptance

Latency

Token Count

Cost

Consistency
```

---

# Prompt Evolution

Never edit prompts directly.

Create

```
PRM_RRT_V1

↓

PRM_RRT_V2

↓

PRM_RRT_V3
```

Historical packets remain reproducible.

---

# A/B Testing

Future

```
Packet

↓

Prompt A

Prompt B

↓

Compare

↓

Analyst Preference
```

---

# Confidence

Current

Only report

```
Narrative Confidence
```

Never

```
AML Probability
```

AML scoring belongs to analytics.

---

# Human-in-the-loop

The analyst remains responsible for

```
Disposition

SAR Decision

Escalation

Regulatory Reporting
```

AI never performs those actions.

---

# Future Multi-Agent Architecture

Current

```
Packet

↓

LLM

↓

Narrative
```

Future

```
Graph Agent

↓

Explanation Agent

↓

Regulatory Agent

↓

Case Builder

↓

Narrative Agent
```

Each agent has a single responsibility.

---

# Agent Responsibilities

Graph Agent

```
Graph

Paths

Communities
```

---

Explanation Agent

```
Business Narrative
```

---

Regulatory Agent

```
Reference Objects

Regulatory Mapping
```

---

Case Builder

```
Timeline

Evidence

Features
```

---

Narrative Agent

```
Final Report

Executive Summary

Analyst Summary
```

---

# Explainability Principles

Every response must answer

```
Why?

How?

Based on what?

What is missing?

What should happen next?
```

---

# Governance

Every explanation stores

```
packet_version

prompt_version

model_version

reference_version
```

Everything is reproducible.

---

# Future Enhancements

```
GraphRAG

Semantic Similarity

Related Historical Cases

Cross-case Retrieval

Investigation Recommendations

Case Clustering

Knowledge Graph

Voice Explanation
```

---

# Design Rules

Rule 1

AI never invents evidence.

---

Rule 2

AI only consumes approved packets.

---

Rule 3

Regulatory references are curated.

---

Rule 4

Feedback improves prompts.

---

Rule 5

Business logic never migrates into prompts.

---

# ADR

ADR-023

AI Explanation Engine

Decision

Generative AI is isolated behind immutable Explanation Packets and curated regulatory references.

Reason

Maintain auditability, reproducibility and regulatory compliance.

---

# Explanation Engine Complete

The Explanation Engine specification consists of

```
Part 1

AI Philosophy

--------------

Part 2

Packets

Prompts

--------------

Part 3

LLM Worker

Routing

Guardrails

--------------

Part 4

Regulatory Layer

Feedback

Future Agents
```

Together they define the complete AI layer of the AI AML Platform.

---

# Next Document

```
docs/implementation/08_IMPLEMENTATION_PART_1.md
```

Topics

- Repository Structure
- Development Standards
- Coding Conventions
- Project Layout
- Technology Stack
- Local Development
- Build Pipeline
