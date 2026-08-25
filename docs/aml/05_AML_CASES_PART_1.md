# AML Cases
## Part 1 — Detection Philosophy, Taxonomy and Case Lifecycle

---
Version: 1.0

Status: Draft

Depends on:

- 04_GRAPH_MODEL

---

# Purpose

This document defines the AML detection philosophy.

Unlike traditional AML systems, cases are not only rule matches.

They are graph-supported investigations.

The purpose of an AML Case is:

- prioritize analyst work;
- aggregate evidence;
- provide explainability;
- support regulatory decision making.

---

# AML Detection Philosophy

The AI AML Layer does not generate alerts directly.

Instead it produces:

```
Signals

↓

Triggers

↓

AML Cases

↓

Explanation

↓

Human Decision
```

Cases are business artifacts.

Alerts are implementation artifacts.

---

# Principles

## Principle 1

One event does not necessarily create one case.

Multiple events may belong to the same investigation.

---

## Principle 2

A case explains a business situation.

Not a database event.

---

## Principle 3

Evidence is immutable.

Cases evolve.

Evidence never changes.

---

## Principle 4

A case never owns business data.

It references business entities.

---

## Principle 5

Every case must be reproducible.

Given canonical tables and trigger configuration,

the same case should always be regenerated.

---

# AML Detection Pipeline

```
Business Event

↓

Trigger

↓

Candidate

↓

Rule Evaluation

↓

Graph Projection

↓

Feature Extraction

↓

Severity

↓

AML Case

↓

Explanation Packet
```

---

# AML Typology Taxonomy

Current

```
Cash Movement

Behavior

Relationship

Profile

Aggregation
```

Future

```
Trading

Corporate

UBO

Behavioral

Fraud

Watchlist
```

---

# Current Case Types

```
AML_CASH_IN_OUT_WINDOW

AML_REPEATED_ROUND_TRIP

AML_SHARED_EXTERNAL_ACCOUNT

AML_NEW_EXTERNAL_ACCOUNT

AML_KYC_INFLOW_MISMATCH

AML_TOP_CONTRACT_FLOW
```

Each case belongs to exactly one primary typology.

Compound cases may reference multiple typologies.

---

# AML Case Lifecycle

```
Candidate

↓

Open

↓

Under Review

↓

Confirmed

↓

Dismissed

↓

Closed
```

---

# Candidate

Created automatically by Trigger Engine.

Contains

- Trigger
- Initial Evidence
- Root Entity

No analyst yet.

---

# Open

Case is accepted.

Graph projection is generated.

Explanation Packet is created.

---

# Under Review

Analyst investigates.

May request:

- more evidence
- additional graph
- historical context

---

# Confirmed

Case becomes valid AML investigation.

Disposition depends on compliance process.

---

# Dismissed

False positive.

Feedback is stored.

Trigger quality updated.

---

# Closed

Case is archived.

Graph remains reproducible.

---

# Case Ownership

Every case has

```
Business Owner

Technical Owner

Current Assignee
```

Business Owner

AML Operations.

Technical Owner

AI AML Layer.

---

# Evidence Categories

Current

```
Cash Movement

Contract

KYC

External Account
```

Future

```
Order

Execution

Position

UBO

Device

Session

Watchlist

Document
```

---

# Evidence Quality

Every evidence item receives

```
Confidence

Freshness

Source

Timestamp
```

Future

```
Trust Score
```

---

# End of Part 1

Next

```
05_AML_CASES_PART_2.md
```

Topics

- Cash In Out
- Repeated Round Trip
- Shared External Account
- New External Account
- KYC Mismatch
- Top Contract Flow
- Detection Rules
