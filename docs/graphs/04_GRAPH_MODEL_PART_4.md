# docs/graph/04_GRAPH_MODEL_PART_4.md

---
title: Graph Model
subtitle: Part 4 — AML Investigation Graphs
version: 1.0
status: Draft
owner: AI AML Layer
depends_on:
  - 04_GRAPH_MODEL_PART_1.md
  - 04_GRAPH_MODEL_PART_2.md
  - 04_GRAPH_MODEL_PART_3.md
---

# Purpose

This document defines how AML cases are represented inside the graph.

Unlike the canonical graph, an AML Case is **not a business entity**.

It is an investigation assembled from a projection of the graph.

The AML Case is ephemeral.

The business graph is permanent.

---

# Investigation Philosophy

The graph is permanent.

```
Client

↓

Contract

↓

CashMovement

↓

ExternalBankAccount
```

The investigation is temporary.

```
AML Case

↓

Evidence

↓

Decision
```

The graph answers

```
"What happened?"
```

The AML Case answers

```
"Why should an analyst care?"
```

---

# AML Case Structure

Every AML Case contains

```
Case

↓

Entities

↓

Evidence

↓

Timeline

↓

Features

↓

Explanation
```

Represented as

```
AMLCase

↓

Client

↓

Contracts

↓

Movements

↓

External Accounts

↓

Typology
```

---

# Case Types

Current

```
AML_CASH_IN_OUT_WINDOW

AML_REPEATED_ROUND_TRIP

AML_SHARED_EXTERNAL_ACCOUNT

AML_NEW_EXTERNAL_ACCOUNT

AML_KYC_INFLOW_MISMATCH

AML_TOP_CONTRACT_FLOW
```

Future

```
AML_LAYERING

AML_LOW_LIQUIDITY

AML_MIRROR_TRADING

AML_WATCHLIST

AML_UBO

AML_FRAUD_SEQUENCE
```

---

# Case Graph

Every case follows the same structure.

```
AML Case

↓

Client

↓

Contract

↓

Cash Movements

↓

External Accounts

↓

Typology
```

---

# AML_CASH_IN_OUT_WINDOW

Purpose

Detect rapid cash in / cash out.

Graph

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

Minimum conditions

```
Deposit

↓

Withdrawal

≤ 7 days

Withdrawal >= 70% Deposit
```

Features

```
days_between

deposit_amount

withdrawal_amount

withdrawal_ratio

top_contract

contracts_count
```

Evidence

```
CashMovement IDs

Timeline

Graph Path
```

---

# AML_REPEATED_ROUND_TRIP

Purpose

Detect repeated deposit / withdrawal cycles.

Graph

```
External

↓

Deposit

↓

Contract

↓

Withdrawal

↓

External
```

Repeated

```
Cycle 1

Cycle 2

Cycle 3
```

Features

```
cycle_count

total_amount

deposit_accounts

withdrawal_accounts

same_account

days_between

top_contract
```

Severity

```
LOW

MEDIUM

HIGH

CRITICAL
```

---

# AML_SHARED_EXTERNAL_ACCOUNT

Purpose

Detect one bank account shared across clients.

Graph

```
Client A

↓

Contract

↓

Movement

↓

External Account

↑

Movement

↑

Contract

↑

Client B
```

Features

```
client_count

contracts

banks

movement_count

first_seen

last_seen
```

HouseConcentrationAccount excluded.

---

# AML_NEW_EXTERNAL_ACCOUNT

Purpose

Detect withdrawal toward previously unseen account.

Graph

```
Client

↓

Contract

↓

Withdrawal

↓

External Account

(first appearance)
```

Features

```
account_age

first_seen

withdrawal_amount

kyc_bucket

country (future)
```

---

# AML_KYC_INFLOW_MISMATCH

Purpose

Detect deposits exceeding declared profile.

Graph

```
Client

↓

KYC

↓

Expected Bucket

↓

Deposits
```

Comparison

```
Observed

vs

Expected
```

Features

```
monthly_total

expected_bucket

ratio

industry

activity
```

---

# AML_TOP_CONTRACT_FLOW

Purpose

Aggregate movements across contracts.

Graph

```
Client

↓

Contract A

↓

ContractGroup

↑

Contract B

↓

Movements
```

Useful when

```
Trading

+

Smart Cash

+

Other products
```

must be analyzed together.

---

# Evidence Model

Every AML Case references evidence.

Current evidence

```
Cash Movements

Contracts

External Accounts

KYC
```

Future

```
Orders

Executions

Watchlists

Sessions

Devices

Documents
```

---

# Timeline

Every case has a timeline.

Example

```
09:00

Deposit

↓

09:45

Withdrawal

↓

13:10

Deposit

↓

15:00

Withdrawal
```

Timeline becomes input to the Explanation Packet.

---

# Explanation Packet

Every case generates one packet.

Contains

```
Summary

Reason

Evidence

Timeline

Features

Graph Paths

Missing Evidence

Next Action
```

No LLM reasoning occurs outside the packet.

---

# Case Severity

Severity is computed.

Inputs

```
Amounts

Cycles

Velocity

Shared Accounts

KYC

Future

Watchlists

Geo

Device
```

Current thresholds

```
LOW

MEDIUM

HIGH

CRITICAL
```

---

# NetworkX Projection

The graph builder creates

```
get_case_subgraph(case_id)
```

Returns

```
NetworkX DiGraph
```

Containing

```
Case

Client

Contracts

Movements

Accounts

Typology
```

No unrelated nodes.

---

# Visualization

Each AML Case becomes one HTML.

Example

```
case_001.html

case_002.html

case_003.html
```

Displayed

```
Timeline

Graph

Evidence

Features

Explanation
```

---

# Future Cases

Future graph projections

```
UBO Discovery

↓

Corporate Structure

↓

Control Chain
```

```
Fraud Sequence

↓

Login

↓

Device

↓

Geo

↓

Withdrawal
```

```
Behavior Drift

↓

Login

↓

Movement

↓

KYC
```

---

# Design Rules

Rule 1

AML Cases never become source data.

Rule 2

AML Cases never modify the graph.

Rule 3

Cases are projections.

Rule 4

Evidence references immutable business events.

Rule 5

Every case must be reproducible.

---

# ADR

ADR-008

AML Cases are graph projections.

Reason

Allows rebuilding every investigation from canonical data.

Cases remain reproducible.

---

# End of Part 4

Next

```
04_GRAPH_MODEL_PART_5.md
```

Topics

- Feature Engineering
- Graph Algorithms
- Community Detection
- Centrality
- Path Algorithms
- Temporal Algorithms
- NetworkX Implementation Details
- Graph Optimization
- Future Graph Database Migration
