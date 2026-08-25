# 03_DATA_MODEL

Version: 1.0

# Canonical Data Model

## Purpose

Define the logical data model for the AI AML Layer. The model is storage-agnostic and is implemented in Redshift/Athena while graph relationships are computed with NetworkX.

---

# Core Entities

- Client
- KYCProfile
- Contract
- ContractGroup
- CashMovement
- ExternalBankAccount
- HouseConcentrationAccount
- FinancialInstitution
- AMLCase
- AMLTypology

---

# Client

Business Key: client_id

Represents the canonical customer.

Relationships

- HAS_KYC_PROFILE
- HAS_CONTRACT

---

# KYCProfile

Represents a versioned KYC snapshot.

Key fields

- risk_score
- declared_activity
- deposits_number
- expected_monthly_inflow_bucket
- industry_type
- last_review_date

Relationship

Client -> HAS_KYC_PROFILE -> KYCProfile

---

# Contract

Business Key: contract_id

Relationships

- ROLLS_UP_TO ContractGroup
- INITIATES_WITHDRAWAL
- RECEIVES_DEPOSIT

---

# ContractGroup

Represents top_contract aggregation.

Purpose

Aggregate activity across multiple product contracts.

---

# CashMovement

Types

- DEPOSIT
- WITHDRAWAL

Acts as the event joining operational and economic flows.

---

# ExternalBankAccount

Economic counterparty.

Used for:

- shared account detection
- new destination detection
- repeated round trips

---

# HouseConcentrationAccount

Operational settlement only.

Never considered:

- economic counterparty
- shared account
- AML hub for reasoning

Relationship

CashMovement -> SETTLED_THROUGH -> HouseConcentrationAccount

---

# FinancialInstitution

Represents external banks.

Relationship

ExternalBankAccount -> HELD_AT -> FinancialInstitution

---

# Node Properties

Common

- node_id
- node_type
- business_key
- source_table
- properties_json
- observed_at
- valid_from
- valid_to
- is_active

---

# Edge Types

Client -> HAS_KYC_PROFILE -> KYCProfile

Client -> HAS_CONTRACT -> Contract

Contract -> ROLLS_UP_TO -> ContractGroup

ExternalBankAccount -> ORIGINATES_DEPOSIT -> CashMovement

CashMovement -> CREDITED_TO_CONTRACT -> Contract

Contract -> INITIATES_WITHDRAWAL -> CashMovement

CashMovement -> PAID_TO_EXTERNAL_ACCOUNT -> ExternalBankAccount

CashMovement -> SETTLED_THROUGH -> HouseConcentrationAccount

ExternalBankAccount -> HELD_AT -> FinancialInstitution

AMLCase -> INVOLVES_CLIENT -> Client

AMLCase -> INVOLVES_CONTRACT -> Contract

AMLCase -> SUPPORTED_BY_MOVEMENT -> CashMovement

AMLCase -> MATCHES_TYPOLOGY -> AMLTypology

---

# Canonical Tables

ai_aml_nodes

ai_aml_edges

ai_aml_events

ai_aml_cash_movements

ai_aml_cases

---

# Design Rules

1. Storage remains relational.
2. Graph is computed on demand.
3. CashMovement is the central event.
4. HouseConcentrationAccount is operational only.
5. ContractGroup consolidates product contracts.
6. Explanation Packets consume graph outputs instead of raw tables.

---

# Future Extensions

- Orders
- Executions
- Instruments
- Positions
- UBO
- Devices
- Sessions
- Geolocation
- Watchlists
- Fraud
