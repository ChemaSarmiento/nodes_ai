# AI AML Layer
## AI Handoff Document

Version: 1.0

Repository

nodes_ai

---

# Purpose

This document is written for another AI model.

Its purpose is to transfer architectural intent.

It should be read before reading any source code.

This file is the architectural memory of the project.

---

# What is this project?

This project is NOT an AML model.

It is NOT a Graph Database project.

It is NOT an LLM project.

It is an AI-native AML platform whose objective is to transform operational financial data into explainable investigations.

The platform is composed of multiple independent layers.

Each layer has exactly one responsibility.

---

# High-Level Architecture

```
Business Data

↓

Canonical Model

↓

Trigger Engine

↓

Temporary Graph

↓

Graph Features

↓

Explanation Packet

↓

LLM

↓

Workbench

↓

Analyst

↓

Feedback
```

Every component exists for one reason only.

Never merge responsibilities.

---

# Design Philosophy

The project follows five principles.

## 1

Business first.

Technology second.

The graph represents business relationships.

Never database relationships.

---

## 2

Storage is not Graph.

The graph is computed.

Not stored.

---

## 3

Explanation before AI.

The AI never discovers evidence.

It explains evidence already computed.

---

## 4

Humans decide.

AI explains.

---

## 5

Everything must be reproducible.

If the same canonical data enters,

the same graph,

features,

packet,

and AML case

must be generated.

---

# Most Important Concepts

The project revolves around seven concepts.

```
Client

↓

Contract

↓

CashMovement

↓

Graph

↓

Features

↓

Explanation Packet

↓

LLM
```

Nothing should bypass these.

---

# Canonical Layer

This is the most important layer.

Everything enters here first.

Every source system must eventually become

```
Nodes

Edges

Events
```

Nothing should consume raw source systems.

Everything consumes canonical entities.

---

# CashMovement

CashMovement is the center of the financial graph.

Not Client.

Not Contract.

Not ExternalAccount.

Every movement of value must become

```
CashMovement
```

This decision is fundamental.

Do not change it.

---

# HouseConcentrationAccount

This node exists only because the brokerage needs it operationally.

It is NOT an economic entity.

Never:

- calculate AML risk from it;
- detect shared accounts from it;
- calculate centrality from it;
- treat it as destination.

It exists only to explain operational settlement.

---

# Graph Philosophy

There is no Graph Database.

NetworkX builds temporary investigation graphs.

```
SQL

↓

Nodes

↓

Edges

↓

NetworkX

↓

Graph

↓

Features

↓

Destroy Graph
```

The graph disappears.

Only the packet survives.

---

# Why no GraphDB?

Reasons

- MVP simplicity
- Existing Redshift investment
- Easier deployment
- Lower cost
- Easier replay

Future migration is expected.

Business model must not change.

---

# Trigger Engine

The Trigger Engine is the orchestrator.

It never:

- computes graphs;
- calls LLM;
- builds packets.

It only decides whether work should begin.

Everything else is downstream.

---

# Explanation Packet

This is the most important artifact.

The packet is the ONLY thing the LLM consumes.

Nothing else.

Never let the LLM read

- SQL;
- Redshift;
- Athena;
- Graph;
- NetworkX.

The packet is the contract.

---

# Why?

Because packets are

- deterministic;
- reproducible;
- auditable.

Prompts are not.

---

# LLM Philosophy

LLMs explain.

They never investigate.

Allowed

- summarize;
- explain;
- organize evidence;
- identify missing evidence.

Forbidden

- AML decisions;
- SQL;
- graph traversal;
- inventing evidence.

---

# Current AML Cases

Current MVP

```
Cash In Out

Repeated Round Trip

Shared External Account

New Destination

KYC Mismatch

Top Contract Flow
```

Future

```
Orders

Executions

Funds

UBO

Behavior

Fraud
```

---

# Future Direction

Everything eventually converges toward

```
Financial Intelligence Platform
```

Supporting

AML

Fraud

KYC

KYB

Compliance

using the same architecture.

---

# Never Break These

The following decisions are frozen.

## Graph

Graph is temporary.

---

## Storage

Redshift is source of truth.

---

## Canonical Layer

Everything enters here first.

---

## Packet

LLM only consumes packets.

---

## Trigger

Everything starts from Trigger Engine.

---

## Human

Human always decides.

---

## Business Keys

Never replace

```
client_id

contract_id

movement_id
```

with warehouse IDs.

---

# Anti-Patterns

Never do these.

❌

LLM queries SQL.

---

❌

LLM queries Graph.

---

❌

Graph stored permanently during MVP.

---

❌

Business logic inside prompts.

---

❌

Business logic inside SQL.

---

❌

Business logic inside FastAPI.

---

❌

One service owning multiple responsibilities.

---

# Architecture Decisions

The following ADRs are frozen.

```
Graph-as-compute

Canonical Layer

Trigger Engine

Explanation Packet

Temporary Graph

Human-in-the-loop
```

Changing any of these requires

a new ADR.

---

# Coding Philosophy

One module.

One responsibility.

One owner.

Examples

Builder

↓

Builds objects.

Never SQL.

Repository

↓

Reads data.

Never Graph.

GraphBuilder

↓

Builds Graph.

Never Trigger.

PacketBuilder

↓

Builds Packet.

Never LLM.

LLMWorker

↓

Explains Packet.

Never SQL.

---

# Current Repository Status

Architecture

100%

Documentation

100%

Implementation

Design Complete

Code

Not started.

Current phase

Repository bootstrap.

---

# Next Development Steps

Priority

1

Canonical Layer

↓

Python

---

2

Trigger Engine

↓

Python

---

3

Graph Builder

↓

NetworkX

---

4

Packet Builder

↓

Pydantic

---

5

LLM Worker

↓

OpenAI

---

6

Workbench

↓

Streamlit

---

# Final Advice to the Next AI

Do not redesign.

Do not simplify.

Do not optimize prematurely.

Preserve the architecture.

Implement incrementally.

Everything important has already been decided.

Your responsibility is implementation, not redesign.
