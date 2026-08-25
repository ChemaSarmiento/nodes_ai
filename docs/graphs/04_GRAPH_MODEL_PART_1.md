# Graph Model
## Part 1 — Philosophy, Semantic Model and Node Taxonomy

---
Version: 1.0

Status: Draft

Repository: nodes_ai

Depends on:

- 03_DATA_MODEL.md
- 03_5_CANONICAL_BUSINESS_MODEL.md

---

# Purpose

This document defines the logical graph used by the AI AML Layer.

Although Phase 1 does **not** use a Graph Database, every entity and relationship is modeled as a graph.

Storage remains relational.

Graph is computed dynamically using NetworkX.

---

# Design Philosophy

The graph represents **business reality**, not tables.

A graph answers questions that relational models answer poorly.

Examples:

Instead of

```
Which deposits belong to Contract X?
```

We ask

```
How did value move?

Who participated?

Which relationships explain the AML case?
```

---

# First Principle

Storage is NOT the graph.

```
Redshift

Athena

S3
```

are storage.

```
NetworkX
```

computes the graph.

---

# Second Principle

CashMovement is the center of the financial graph.

Everything that represents money movement is attached to a CashMovement node.

Never connect deposits directly to clients.

Never connect withdrawals directly to bank accounts.

Always use:

```
CashMovement
```

---

# Third Principle

Separate Economic Relationships from Operational Relationships.

Economic

```
Client

↓

Contract

↓

CashMovement

↓

External Account
```

Operational

```
CashMovement

↓

House Concentration Account
```

Operational relationships never affect AML reasoning.

---

# Fourth Principle

The graph is temporal.

Every edge should be considered valid only during its observed time.

Eventually every relationship becomes

```
(valid_from, valid_to)
```

---

# Fifth Principle

Cases are graph projections.

The graph is permanent.

Cases are temporary views.

```
Graph

↓

AML Case

↓

Explanation Packet
```

---

# Semantic Layers

Every node belongs to exactly one semantic layer.

---

## Identity Layer

Purpose

Represents legal identities.

Nodes

```
Client

KYCProfile

Future

UBO

CorporateEntity

Representative
```

---

## Financial Layer

Purpose

Represents movement of value.

Nodes

```
Contract

ContractGroup

CashMovement

Future

Order

Execution

Instrument

Fund

Position
```

---

## Infrastructure Layer

Purpose

Supports execution.

Nodes

```
ExternalBankAccount

HouseConcentrationAccount

FinancialInstitution

Advisor
```

Infrastructure does not imply ownership.

---

## AML Layer

Purpose

Represents investigations.

Nodes

```
AMLCase

AMLTypology

Future

Alert

Evidence

Watchlist

Decision
```

---

## Behavior Layer

Future

```
Login

Session

GeoLocation

Device

IPAddress
```

These nodes support Fraud and Behavioral AML.

---

# Node Taxonomy

The graph is divided into:

```
Master Nodes

Reference Nodes

Event Nodes

Case Nodes
```

---

# Master Nodes

Represent business entities.

Stable.

Low change frequency.

```
Client

Contract

ContractGroup

ExternalBankAccount

FinancialInstitution
```

---

# Reference Nodes

Represent classifications.

```
AMLTypology

Industry

BusinessSector

Future

Country

GeoRegion

Watchlist
```

---

# Event Nodes

Represent facts.

High volume.

Immutable.

```
CashMovement

Future

Order

Execution

Login

Session

PositionMovement
```

---

# Case Nodes

Represent investigation artifacts.

```
AMLCase

Future

Alert

Evidence

Decision
```

---

# Node Definitions

---

## Client

Represents the canonical customer.

Business Key

```
client_id
```

Properties

| Property | Description |
|-----------|-------------|
| client_id | Canonical identifier |
| client_type | LIGHT / BLACK |
| segment | Business segment |
| onboarding_date | Customer since |
| status | ACTIVE / CLOSED |
| risk_level | Business risk |
| is_moral_entity | Physical / Legal |

Allowed outgoing edges

```
HAS_KYC_PROFILE

HAS_CONTRACT
```

Incoming edges

```
INVOLVES_CLIENT
```

---

## KYCProfile

Represents one version of KYC.

Business Key

```
(client,last_review_date)
```

Properties

```
risk_score

risk_level

declared_activity

expected_monthly_inflow_bucket

expected_monthly_outflow

industry_type

business_sector
```

Outgoing edges

None.

Incoming

```
HAS_KYC_PROFILE
```

---

## Contract

Business Key

```
contract_id
```

Properties

```
contract_type

advisor

currency

opening_date

contract_age

top_contract
```

Outgoing

```
ROLLS_UP_TO

INITIATES_WITHDRAWAL
```

Incoming

```
HAS_CONTRACT

CREDITED_TO_CONTRACT
```

---

## ContractGroup

Logical aggregation.

Business Key

```
top_contract
```

Purpose

Represents one economic relationship.

Example

```
Trading

↓

Smart Cash

↓

Future Product
```

---

## CashMovement

Business Event.

Business Key

```
movement_id
```

Properties

```
movement_type

amount

operation_date

client

contract
```

CashMovement is immutable.

Never update.

Create new events instead.

---

## ExternalBankAccount

Business Key

```
external_account_hash
```

Purpose

Represents economic counterparty.

Used for:

```
Shared account detection

New destination detection

Round trips

Behavior analysis
```

---

## HouseConcentrationAccount

Business Key

```
house_account_hash
```

Purpose

Operational settlement only.

Properties

```
is_aml_hub

exclude_from_shared_account_detection

exclude_from_counterparty_risk

exclude_from_graph_centrality
```

Never participates in AML logic.

---

## FinancialInstitution

Represents a bank.

Business Key

```
institution_hash
```

Properties

```
bank

country

type
```

---

## AMLCase

Business Key

```
case_id
```

Properties

```
severity

status

created_at

case_type
```

Outgoing

```
INVOLVES_CLIENT

INVOLVES_CONTRACT

SUPPORTED_BY_MOVEMENT

MATCHES_TYPOLOGY
```

---

## AMLTypology

Business Key

```
typology_id
```

Current

```
Cash In Out

Repeated Round Trip

Shared External Account

New External Account

Top Contract Concentration

KYC Mismatch
```

Future

```
Layering

Mirror Trading

Wash Trading

Low Liquidity

UBO Risk
```

---

# Node Colors (Visualization)

| Node | Color |
|--------|--------|
| Client | Blue |
| Contract | Purple |
| ContractGroup | Indigo |
| CashMovement | Red |
| ExternalBankAccount | Orange |
| HouseConcentrationAccount | Gray |
| FinancialInstitution | Slate |
| AMLCase | Dark Red |
| AMLTypology | Violet |
| KYCProfile | Green |

---

# Graph Layers

```
Identity

↓

Financial

↓

Infrastructure

↓

AML

↓

Behavior
```

Never connect Infrastructure directly to AML unless explicitly required for audit.

---

# End of Part 1

Next:

**04_GRAPH_MODEL_PART_2.md**

Topics

- Edge taxonomy
- Relationship rules
- Temporal model
- Economic flows
- Operational flows
- HouseConcentrationAccount rules
