# docs/graph/04_GRAPH_MODEL_PART_2.md

---
title: Graph Model
subtitle: Part 2 — Edge Taxonomy, Temporal Model and Economic Flows
version: 1.0
status: Draft
depends_on:
  - 03_DATA_MODEL.md
  - 03_5_CANONICAL_BUSINESS_MODEL.md
  - 04_GRAPH_MODEL_PART_1.md
---

# Purpose

This document defines every relationship of the graph.

Unlike nodes, **edges represent business meaning**.

An edge is never a foreign key.

It represents:

- ownership
- movement
- responsibility
- economic flow
- operational flow
- evidence
- investigation

---

# Edge Philosophy

Nodes answer

```
What exists?
```

Edges answer

```
How are they related?
```

---

# Edge Categories

Every edge belongs to one category.

```
Ownership

Economic Flow

Operational Flow

Reference

AML

Behavior (future)

Evidence (future)
```

---

# Ownership Edges

Represent stable relationships.

Low change frequency.

## HAS_KYC_PROFILE

```
Client

↓

HAS_KYC_PROFILE

↓

KYCProfile
```

Purpose

Associate the client with a specific KYC snapshot.

Properties

```
observed_at

valid_from

valid_to
```

---

## HAS_CONTRACT

```
Client

↓

HAS_CONTRACT

↓

Contract
```

Purpose

Defines customer ownership.

---

## ROLLS_UP_TO

```
Contract

↓

ROLLS_UP_TO

↓

ContractGroup
```

Purpose

Groups contracts into one economic relationship.

---

# Economic Flow Edges

These are the most important AML relationships.

---

## ORIGINATES_DEPOSIT

```
ExternalBankAccount

↓

CashMovement(DEPOSIT)
```

Meaning

The money originates from this external account.

Properties

```
amount

operation_date

currency

source_table
```

---

## CREDITED_TO_CONTRACT

```
CashMovement(DEPOSIT)

↓

Contract
```

Meaning

Deposit increases contract cash balance.

---

## INITIATES_WITHDRAWAL

```
Contract

↓

CashMovement(WITHDRAWAL)
```

Meaning

Contract is the economic owner initiating the withdrawal.

NOT

```
House Account
```

---

## PAID_TO_EXTERNAL_ACCOUNT

```
CashMovement(WITHDRAWAL)

↓

ExternalBankAccount
```

Meaning

Economic destination.

Used by AML.

---

# Operational Flow Edges

These edges exist only because brokerage operations require settlement.

They must NOT influence AML reasoning.

---

## SETTLED_THROUGH

```
CashMovement

↓

HouseConcentrationAccount
```

Purpose

Operational settlement.

Properties

```
operational_only = true

exclude_from_reasoning = true
```

---

## HELD_AT

```
ExternalBankAccount

↓

FinancialInstitution
```

Purpose

Associate external account with bank.

---

# AML Edges

Represent investigations.

---

## INVOLVES_CLIENT

```
AMLCase

↓

Client
```

---

## INVOLVES_CONTRACT

```
AMLCase

↓

Contract
```

---

## SUPPORTED_BY_MOVEMENT

```
AMLCase

↓

CashMovement
```

One case can reference many movements.

---

## INVOLVES_EXTERNAL_ACCOUNT

```
AMLCase

↓

ExternalBankAccount
```

---

## MATCHES_TYPOLOGY

```
AMLCase

↓

AMLTypology
```

---

# Future AML Edges

```
SUPPORTED_BY_DEVICE

SUPPORTED_BY_SESSION

SUPPORTED_BY_LOGIN

SUPPORTED_BY_WATCHLIST

SUPPORTED_BY_EVIDENCE
```

---

# Temporal Model

Every edge is temporal.

Minimum properties

```
observed_at

first_seen

last_seen
```

Future

```
valid_from

valid_to
```

---

# Economic Flow

Deposit

```
ExternalBankAccount

↓

ORIGINATES_DEPOSIT

↓

CashMovement

↓

CREDITED_TO_CONTRACT

↓

Contract

↓

HAS_CONTRACT

↓

Client
```

Withdrawal

```
Client

↓

HAS_CONTRACT

↓

Contract

↓

INITIATES_WITHDRAWAL

↓

CashMovement

↓

PAID_TO_EXTERNAL_ACCOUNT

↓

ExternalBankAccount
```

AML reasoning follows only this path.

---

# Operational Flow

Deposit

```
CashMovement

↓

SETTLED_THROUGH

↓

HouseConcentrationAccount
```

Withdrawal

```
CashMovement

↓

SETTLED_THROUGH

↓

HouseConcentrationAccount
```

Operational only.

---

# HouseConcentrationAccount Rules

Rule 1

Never create

```
Client

↓

HouseAccount
```

relationships.

---

Rule 2

Never create

```
HouseAccount

↓

Client
```

relationships.

---

Rule 3

HouseAccount is excluded from

- shared account detection
- graph centrality
- AML scoring
- counterparty clustering

---

Rule 4

HouseAccount appears only for audit.

---

# Edge Properties

Every edge has

```
edge_id

edge_type

from_node

to_node

observed_at

amount

operation_date

properties_json

is_active
```

Future

```
confidence

weight

risk_contribution
```

---

# Edge Direction Rules

Ownership

```
Owner

↓

Owned
```

Economic Flow

```
Origin

↓

Movement

↓

Destination
```

Operational

```
Movement

↓

Infrastructure
```

AML

```
Case

↓

Entity
```

Never reverse directions.

---

# Canonical Graph

```
Client

↓

HAS_CONTRACT

↓

Contract

↓

INITIATES_WITHDRAWAL

↓

CashMovement

↓

PAID_TO_EXTERNAL_ACCOUNT

↓

ExternalBankAccount

↓

HELD_AT

↓

FinancialInstitution
```

Deposit

```
ExternalBankAccount

↓

ORIGINATES_DEPOSIT

↓

CashMovement

↓

CREDITED_TO_CONTRACT

↓

Contract
```

Operational

```
CashMovement

↓

SETTLED_THROUGH

↓

HouseConcentrationAccount
```

---

# Edge Constraints

Exactly one

```
ORIGINATES_DEPOSIT
```

per deposit.

Exactly one

```
CREDITED_TO_CONTRACT
```

per deposit.

Exactly one

```
INITIATES_WITHDRAWAL
```

per withdrawal.

Exactly one

```
PAID_TO_EXTERNAL_ACCOUNT
```

per withdrawal.

Zero or one

```
SETTLED_THROUGH
```

per movement.

---

# Validation Rules

Reject graph if

- Deposit has no contract
- Withdrawal has no contract
- Client has no contract
- Contract has no client
- Movement has no direction
- HouseAccount appears as ExternalBankAccount

---

# ADR References

ADR-002

Graph-as-compute

ADR-004

HouseConcentrationAccount

ADR-005

Economic vs Operational Flows

---

# End of Part 2

Next

```
04_GRAPH_MODEL_PART_3.md
```

Topics

- AML projections
- Graph projections
- Subgraphs
- NetworkX implementation
- Feature extraction
- Explanation Packets
- Visualization rules
- Migration to GraphDB
