# AML Cases
## Part 5 — Corporate AML, UBO, Watchlists and Composite Risk

---
Version: 1.0

Status: Draft

Depends on

- 04_GRAPH_MODEL
- 05_AML_CASES_PART_1
- 05_AML_CASES_PART_2
- 05_AML_CASES_PART_3
- 05_AML_CASES_PART_4

---

# Purpose

This section defines AML cases that require
corporate relationships, beneficial ownership,
watchlists and relationship analysis.

These cases are planned for Phase 2 of the AI AML Layer.

---

# Corporate AML Philosophy

Traditional AML usually evaluates only the customer.

AI AML Layer evaluates

```
Customer

↓

Corporate Structure

↓

Beneficial Owners

↓

Relationships

↓

Counterparties
```

The investigation extends beyond the legal entity.

---

# Future Nodes

```
CorporateEntity

BeneficialOwner

Representative

OwnershipEdge

CorporateDocument

Watchlist

PEP

AdverseMedia
```

---

# Case 15

## AML_UBO_DISCOVERY

### Objective

Identify natural persons exercising effective control.

---

## Business Question

```
Who ultimately controls this legal entity?
```

---

## Graph

```
Legal Entity

↓

Holding

↓

Holding

↓

Natural Person
```

---

## Detection

Compute

```
Effective Ownership
```

through ownership paths.

Default threshold

```
25%
```

Risk-adjusted threshold

```
10%
```

for high-risk entities.

---

## Features

```
ownership_percentage

ownership_depth

control_type

document_count

confidence
```

---

## Evidence

```
Corporate Documents

Shareholder Registry

Articles of Incorporation

Powers of Attorney
```

---

## Explanation

Every UBO must include

```
Ownership Path

Ownership %

Supporting Documents

Confidence
```

---

# Case 16

## AML_HIGH_RISK_UBO

### Objective

Detect UBOs belonging to high-risk jurisdictions or categories.

---

## Future Inputs

```
Country

Watchlists

PEP

Sanctions

Adverse Media
```

---

## Graph

```
UBO

↓

Corporate Entity

↓

Customer
```

---

# Case 17

## AML_WATCHLIST_MATCH

### Objective

Detect direct or indirect watchlist matches.

---

## Future Graph

```
Customer

↓

Watchlist
```

or

```
UBO

↓

Watchlist
```

---

## Severity

Depends on

```
List

Confidence

Relationship

Recency
```

---

# Case 18

## AML_PEP_MATCH

### Objective

Identify Politically Exposed Persons.

---

Graph

```
Customer

↓

PEP
```

Future

```
UBO

↓

PEP

Representative

↓

PEP

Family

↓

PEP
```

---

# Case 19

## AML_ADVERSE_MEDIA

### Objective

Detect relevant adverse media.

---

Future Inputs

```
News

Court Records

Regulatory Actions

Open Source Intelligence
```

---

## Graph

```
Customer

↓

Adverse Media
```

---

## Features

```
severity

recency

source_count

confidence
```

---

# Relationship AML

Relationship AML evaluates

```
Customer

↓

Relationship

↓

Customer
```

instead of

```
Customer

↓

Transaction
```

alone.

---

# Case 20

## AML_RELATIONSHIP_CLUSTER

### Objective

Detect customers connected through multiple relationships.

---

Possible Connections

```
Shared External Account

Shared Device

Shared IP

Shared UBO

Shared Representative

Shared Geo
```

---

## Graph

```
Customer A

↓

Relationship

↑

Customer B

↓

Relationship

↑

Customer C
```

---

# Community Detection

Future

```
Louvain

Leiden

GraphFrames
```

Goal

Identify communities instead of isolated alerts.

---

# Composite Risk Engine

Every AML Case contributes risk.

Final investigation score

```
Financial

+

Behavior

+

Relationship

+

Corporate

+

Watchlists
```

---

# Composite Examples

Example 1

```
Cash In Out

+

New Destination

↓

MEDIUM
```

Example 2

```
Repeated Round Trip

+

Shared Account

↓

HIGH
```

Example 3

```
Repeated Round Trip

+

New Country Login

+

New Device

↓

CRITICAL
```

Example 4

```
UBO

+

Watchlist

↓

CRITICAL
```

---

# Composite Risk Matrix

| Financial | Behavior | Relationship | Corporate | Result |
|-----------|-----------|--------------|-----------|--------|
| Low | Low | Low | Low | Low |
| High | Low | Low | Low | Medium |
| High | Medium | Medium | Low | High |
| High | High | High | High | Critical |

---

# Future Risk Contributors

```
Behavior Score

Corporate Score

Relationship Score

Trading Score

Watchlist Score

KYC Score
```

---

# Case Dependencies

Some cases require others.

Example

```
AML_SHARED_ACCOUNT

↓

Relationship Graph

↓

Community Detection
```

---

Another

```
UBO Discovery

↓

Watchlist Screening

↓

Corporate Risk
```

---

# Design Rules

Rule 1

Relationship cases never replace financial evidence.

---

Rule 2

Corporate evidence is cumulative.

---

Rule 3

Community detection is explanatory.

Not deterministic.

---

Rule 4

UBO paths are versioned.

---

Rule 5

Composite Risk never overrides analyst judgment.

---

# ADR

ADR-013

Relationship-Based AML

Decision

AML investigations expand from individual events to relationship networks.

Reason

Money laundering is often collaborative rather than isolated.

---

# End of Part 5

Next

```
05_AML_CASES_PART_6.md
```

Topics

- Case Scoring Framework
- Human Review Workflow
- Case Prioritization
- SLA
- KPIs
- Analyst Feedback
- Continuous Learning
- AML Case API
- Final AML Architecture
