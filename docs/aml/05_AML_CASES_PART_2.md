# AML Cases
## Part 2 — Cash Movement Cases

---
Version: 1.0

Status: Draft

Depends on:

- 04_GRAPH_MODEL
- 05_AML_CASES_PART_1

---

# Purpose

This document specifies all AML cases based on cash movements.

These cases represent the first implementation phase because only the following datasets are required:

- Clients
- Contracts
- KYC
- Deposits
- Withdrawals

No trading information is required.

---

# Case 1

## AML_CASH_IN_OUT_WINDOW

### Objective

Detect situations where money enters a contract and exits shortly afterwards.

This is the most important Phase 1 AML scenario.

---

## Business Motivation

Typical behavior:

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

The concern is not the movement itself.

The concern is the **speed** and **amount**.

---

## Business Question

```
Did money leave almost immediately after entering?
```

---

## Detection Inputs

Required

```
client_id

contract_id

top_contract

deposit_date

withdrawal_date

deposit_amount

withdrawal_amount
```

---

## Detection Rule

Conditions

```
Same client

AND

Same contract

OR

Same top_contract

AND

Withdrawal after deposit

AND

Days <= configured window

AND

Withdrawal >= configured ratio
```

---

## Default Configuration

```yaml
cash_in_out_window_days: 7

withdrawal_ratio: 0.70
```

---

## Features

Generated

```
days_between

deposit_amount

withdrawal_amount

withdrawal_ratio

contract_count

top_contract
```

---

## Graph

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

---

## Evidence

Graph Path

Timeline

Movement IDs

Amounts

Contract

Customer

---

## Severity

LOW

```
Amount < 50,000
```

MEDIUM

```
50,000 <= Amount < 500,000
```

HIGH

```
>= 500,000
```

---

# Case 2

## AML_REPEATED_ROUND_TRIP

### Objective

Detect repeated cash-in cash-out cycles.

Unlike the previous case, repetition matters.

---

## Business Question

```
Does this customer repeatedly move money in and out?
```

---

## Graph

```
Deposit 1

↓

Withdrawal 1

↓

Deposit 2

↓

Withdrawal 2

↓

Deposit 3

↓

Withdrawal 3
```

---

## Detection Rule

```
Same Client

AND

Same Contract

OR

Same Top Contract

AND

Cycles >= minimum

AND

Window <= configured

AND

Withdrawal Ratio satisfied
```

---

## Configuration

```yaml
round_trip_window_days: 30

pair_window_days: 7

minimum_cycles: 2

minimum_ratio: 0.70
```

---

## Features

```
cycle_count

total_deposit

total_withdrawal

average_cycle_days

deposit_accounts

withdrawal_accounts

same_origin_destination

multi_origin_accounts
```

---

## Severity

LOW

```
2 cycles
```

MEDIUM

```
2 cycles

+

100K
```

HIGH

```
>=3 cycles

OR

>=500K
```

CRITICAL

```
>=3 cycles

AND

>=1M

OR

Combined Risk
```

---

## Combined Risk

Increase severity if combined with

```
KYC Mismatch

Shared Account

New Destination

Future

High Risk Country

Device Risk

Watchlist
```

---

# Explanation Packet

Example

```json
{
  "case_type":"AML_REPEATED_ROUND_TRIP",
  "summary":"Repeated deposit-withdrawal cycles detected.",
  "cycle_count":3,
  "total_amount":1280000,
  "window_days":26,
  "severity":"HIGH"
}
```

---

# Case 3

## AML_SHARED_EXTERNAL_ACCOUNT

### Objective

Detect external accounts linked to multiple customers.

---

## Business Question

```
Is this account shared?
```

---

## Detection Rule

```
External Account

↓

More than one Client
```

---

## Important

Exclude

```
HouseConcentrationAccount

Internal Accounts
```

Never treat infrastructure as counterparties.

---

## Graph

```
Client A

↓

Contract

↓

External Account

↑

Contract

↑

Client B
```

---

## Features

```
client_count

contract_count

movement_count

first_seen

last_seen

institutions
```

---

## Severity

LOW

```
2 clients
```

MEDIUM

```
3 clients
```

HIGH

```
>=4 clients
```

---

# Evidence

```
Clients

Contracts

Cash Movements

Timeline
```

---

# Validation

Reject if

```
Node Type == HouseConcentrationAccount
```

---

# End of Part 2

Next

```
05_AML_CASES_PART_3.md
```

Topics

- AML_NEW_EXTERNAL_ACCOUNT
- AML_KYC_INFLOW_MISMATCH
- AML_TOP_CONTRACT_FLOW
- Compound AML Cases
- Trigger Mapping
