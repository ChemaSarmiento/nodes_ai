# AML Cases
## Part 6 — Case Scoring, Human Review and Continuous Learning

---
Version: 1.0

Status: Draft

Depends on

- 05_AML_CASES_PART_1
- 05_AML_CASES_PART_2
- 05_AML_CASES_PART_3
- 05_AML_CASES_PART_4
- 05_AML_CASES_PART_5

---

# Purpose

This document closes the AML Case specification.

It defines:

- Case Scoring
- Prioritization
- Human Review
- Feedback Loop
- Continuous Learning
- APIs
- KPIs

Cases become operational objects from this point.

---

# Case Scoring

Every AML Case produces a normalized score.

The score is NOT a probability of money laundering.

It is an investigation priority.

---

# Current Score Components

```
Financial Score

+

Behavior Score

+

Relationship Score

+

Profile Score
```

Future

```
Corporate Score

Watchlist Score

Fraud Score

Model Score
```

---

# Example

```
Financial

75

Behavior

20

Relationship

10

Profile

15

Total

120
```

---

# Priority Bands

LOW

```
0-39
```

MEDIUM

```
40-79
```

HIGH

```
80-119
```

CRITICAL

```
>=120
```

Configuration driven.

---

# Prioritization

Cases are ordered by

```
Priority

↓

Creation Time

↓

Business SLA
```

Not by arrival order.

---

# Human Review

Every AML Case requires human review.

No automated disposition.

Workflow

```
Open

↓

Assigned

↓

Reviewed

↓

Disposition

↓

Closed
```

---

# Analyst Actions

Current

```
Confirm

Dismiss

Escalate

Request Evidence
```

Future

```
EDD

SAR Candidate

Watch

Freeze

Legal Review
```

---

# Feedback

Every analyst decision generates feedback.

Stored fields

```
Disposition

Reason

Usefulness

False Positive

Comments
```

---

# Continuous Learning

Feedback updates

```
Trigger Quality

Thresholds

Feature Importance

Future Models
```

Never modify historical evidence.

---

# Trigger Metrics

Every trigger stores

```
Execution Count

Case Count

Precision

False Positive Rate

Average Severity

Analyst Acceptance
```

---

# AML KPIs

Operational

```
Cases per Day

Average Review Time

Backlog

Open Cases
```

Quality

```
Precision

Analyst Acceptance

False Positive Rate

Escalation Rate
```

Business

```
Time to Understand

Time to Decision

Analyst Productivity
```

---

# SLA

Suggested

LOW

```
5 business days
```

MEDIUM

```
2 business days
```

HIGH

```
24 hours
```

CRITICAL

```
Immediate
```

Configurable.

---

# Explanation Packet Lifecycle

```
Case Created

↓

Graph Projection

↓

Features

↓

Packet

↓

LLM

↓

Workbench

↓

Analyst

↓

Feedback
```

Packet is immutable.

If evidence changes,

generate a new packet.

---

# AML API

Future endpoints

```
GET /cases

GET /cases/{id}

GET /cases/{id}/graph

GET /cases/{id}/packet

POST /cases/{id}/feedback

POST /cases/{id}/assign

POST /cases/{id}/rebuild
```

---

# Data Retention

Business entities

```
Canonical tables
```

Investigation

```
AML Case

Explanation Packet

Feedback
```

must remain reproducible.

---

# Governance

Every case stores

```
graph_version

trigger_version

packet_version

model_version

rule_version
```

This guarantees auditability.

---

# Design Rules

Rule 1

Cases never overwrite evidence.

---

Rule 2

Feedback never modifies historical packets.

---

Rule 3

Packets are immutable.

---

Rule 4

Graph projections are reproducible.

---

Rule 5

Every decision is attributable.

---

# Future AI

Future versions may include

```
Case Summaries

Suggested Investigation Steps

Regulatory References

Narrative Generation

Related Historical Cases

Semantic Similarity
```

The final decision remains human.

---

# ADR

ADR-014

Human-in-the-loop AML

Decision

AI assists.

Humans decide.

Reason

Compliance responsibility cannot be delegated.

---

# AML Package Complete

The AML Case specification now consists of

```
05_AML_CASES_PART_1

Detection Philosophy

----------------------

05_AML_CASES_PART_2

Cash Movement Cases

----------------------

05_AML_CASES_PART_3

Profile & Contract Cases

----------------------

05_AML_CASES_PART_4

Behavioral AML

----------------------

05_AML_CASES_PART_5

Corporate & Relationship AML

----------------------

05_AML_CASES_PART_6

Scoring & Operations
```

Together these define

- AML detection philosophy
- Financial AML
- Behavioral AML
- Relationship AML
- Corporate AML
- Composite Risk
- Human Review
- Feedback
- Continuous Learning

---

# Next Document

```
docs/architecture/06_TRIGGER_ENGINE_PART_1.md
```

Topics

- Event-Driven Architecture
- Trigger Philosophy
- Trigger Taxonomy
- Queue Architecture
- Trigger Lifecycle
- Trigger Registry
- Trigger Configuration
