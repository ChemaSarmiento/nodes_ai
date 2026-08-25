# Canonical Business Model

**Version:** 1.0  
**Status:** Draft  
**Owner:** AI AML Layer  
**Repository:** nodes_ai

---

# Purpose

This document defines the **canonical business model** for the AI AML Layer.

It intentionally separates:

- Business entities
- Business events
- Economic relationships
- Operational relationships
- AML concepts
- Infrastructure concepts

This model is **technology independent**.

It is valid whether the implementation uses:

- NetworkX
- Neo4j
- Neptune
- TigerGraph
- Relational tables

The implementation changes.

The business model does not.

---

# Philosophy

The AI AML Layer models **business reality**.

Not databases.

Not APIs.

Not source systems.

The graph represents:

- who participates,
- what happened,
- how value moved,
- why AML cares.

---

# Core Principle

Every node belongs to exactly one semantic layer.

```
Identity

Financial

Infrastructure

AML

Behavior
```

Relationships between layers are explicit.

---

# Semantic Layers

```
Identity Layer

Financial Layer

Infrastructure Layer

AML Layer

Behavior Layer
```

---

# Identity Layer

Represents people and legal identities.

Current nodes

```
Client

KYCProfile
```

Future

```
UBO

Representative

Corporate Entity

Beneficial Owner
```

Purpose

Answer

```
Who is this customer?
```

---

# Financial Layer

Represents movement of value.

Current nodes

```
Contract

ContractGroup

CashMovement
```

Future

```
Order

Execution

Instrument

Fund

Position
```

Purpose

Answer

```
How did money move?
```

---

# Infrastructure Layer

Represents operational objects.

Current nodes

```
ExternalBankAccount

HouseConcentrationAccount

FinancialInstitution
```

Future

```
InternalAccount

Advisor

Custodian
```

Purpose

Infrastructure supports operations.

Infrastructure is NOT economic ownership.

---

# AML Layer

Represents investigations.

Current nodes

```
AMLCase

AMLTypology
```

Future

```
Alert

Evidence

WatchlistHit

Decision

Disposition
```

Purpose

Explain why a case exists.

---

# Behavior Layer

Current

None

Future

```
Login

Session

Device

GeoLocation

IPAddress
```

Purpose

Model customer behavior.

---

# Business Entity Definitions

## Client

Represents a customer.

Business Key

```
client_id
```

Properties

- client_type
- segment
- onboarding_date
- status
- risk_level
- is_moral_entity

Responsibilities

Client owns:

- contracts
- KYC history

Never:

- deposits
- withdrawals

Those belong to Contracts.

---

## KYCProfile

Represents a version of KYC.

Business Key

```
(client,last_review)
```

A client may have multiple KYC profiles.

KYC is temporal.

Never overwrite history.

---

## Contract

Represents an investment relationship.

Business Key

```
contract_id
```

Responsibilities

Receives deposits.

Initiates withdrawals.

Owns positions.

---

## ContractGroup

Logical aggregation.

Business Key

```
top_contract
```

Purpose

Aggregate all product contracts belonging to one relationship.

Example

```
Trading

↓

Smart Cash

↓

Future products
```

---

## CashMovement

Represents movement of value.

Never represents a bank account.

Types

```
Deposit

Withdrawal
```

Properties

- amount
- operation_date
- client
- contract

CashMovement is the central event of the model.

---

## ExternalBankAccount

Represents an external economic counterparty.

Examples

Checking account

Corporate account

Third-party account

Used for:

- shared account detection
- new destination detection
- money flow analysis

---

## HouseConcentrationAccount

Represents brokerage operational settlement.

NOT an AML counterparty.

Responsibilities

Settlement only.

Never participates in:

- AML risk
- shared account detection
- graph centrality
- economic ownership

---

## FinancialInstitution

Represents a bank.

Examples

BBVA

Banorte

HSBC

Santander

---

## AMLCase

Represents an investigation.

Contains

- client
- contract
- movements
- typology
- evidence

Lifecycle

OPEN

↓

UNDER_REVIEW

↓

CONFIRMED

↓

DISMISSED

↓

CLOSED

---

## AMLTypology

Business classification.

Current

```
Cash In Out

Repeated Round Trip

Shared External Account

KYC Mismatch

New External Account

Top Contract Concentration
```

Future

```
Layering

Mirror Trading

Wash Trades

Low Liquidity

Round Tripping

UBO Risk
```

---

# Business Events

Current

Deposit

Withdrawal

Future

Login

Order

Execution

Position Change

Beneficiary Change

KYC Update

---

# Economic Flow

Economic ownership.

Deposit

```
ExternalBankAccount

↓

Deposit

↓

Contract

↓

Client
```

Withdrawal

```
Client

↓

Contract

↓

Withdrawal

↓

ExternalBankAccount
```

AML reasoning follows economic flow.

---

# Operational Flow

Operational settlement.

Deposit

```
Deposit

↓

HouseConcentrationAccount
```

Withdrawal

```
Withdrawal

↓

HouseConcentrationAccount
```

These relationships exist only for operational audit.

They never define ownership.

---

# Canonical Relationships

```
Client

↓

HAS_KYC_PROFILE

↓

KYCProfile
```

```
Client

↓

HAS_CONTRACT

↓

Contract
```

```
Contract

↓

ROLLS_UP_TO

↓

ContractGroup
```

```
ExternalBankAccount

↓

ORIGINATES_DEPOSIT

↓

CashMovement
```

```
CashMovement

↓

CREDITED_TO_CONTRACT

↓

Contract
```

```
Contract

↓

INITIATES_WITHDRAWAL

↓

CashMovement
```

```
CashMovement

↓

PAID_TO_EXTERNAL_ACCOUNT

↓

ExternalBankAccount
```

```
CashMovement

↓

SETTLED_THROUGH

↓

HouseConcentrationAccount
```

```
ExternalBankAccount

↓

HELD_AT

↓

FinancialInstitution
```

---

# Business Invariants

Always true.

```
Client owns Contracts.
```

```
Contract owns movements.
```

```
CashMovement always has direction.
```

```
Deposits move:

External

↓

Contract
```

```
Withdrawals move:

Contract

↓

External
```

HouseConcentrationAccount never changes economic ownership.

---

# Design Rules

Rule 1

Model business.

Never model database joins.

---

Rule 2

CashMovement is always the center of money movement.

---

Rule 3

Infrastructure never drives AML reasoning.

---

Rule 4

Economic ownership always dominates operational ownership.

---

Rule 5

Every node belongs to one semantic layer.

---

Rule 6

Every AML case references business entities, never operational artifacts only.

---

# Future Extensions

Identity

- UBO
- Corporate Structures

Financial

- Orders
- Executions
- Positions
- Instruments

Behavior

- Sessions
- Devices
- Geo
- Login History

AML

- Alerts
- Evidence
- Watchlists
- Decisions
- Regulatory References

Fraud

- Impossible Travel
- Behavioral Sequences
- Device Sharing
- Velocity Detection

---

# Architecture Decision Records

Associated ADRs

- ADR-001 AI AML Layer
- ADR-002 Graph-as-Compute
- ADR-003 Trigger Engine
- ADR-004 House Concentration Account
- ADR-005 Explanation Packet
- ADR-006 Human in the Loop

---

# End of Document
