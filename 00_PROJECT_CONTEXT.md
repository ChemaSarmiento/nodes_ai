# 00_PROJECT_CONTEXT

Version: 1.0

# AI AML Layer

## Objetivo

Construir una capa de inteligencia AML basada en eventos que transforme datos operativos en explicaciones accionables para analistas AML.

El sistema NO reemplaza al analista.
El sistema explica, prioriza y documenta.

---

# Problema

Los motores AML tradicionales generan muchas alertas con poco contexto.

Problemas principales:

- Alto porcentaje de falsos positivos.
- Investigación manual.
- Información distribuida.
- Baja explicabilidad.
- Escasa reutilización del conocimiento.

---

# Solución

AI AML Layer

Arquitectura basada en:

- Datos canónicos
- Trigger Engine
- Queue
- SQL Templates
- NetworkX (Phase 1)
- Explanation Packets
- LLM Explanation Worker
- Human-in-the-loop

---

# Arquitectura

Fuentes

↓

Redshift / Athena / S3

↓

Modelo Canónico

↓

Trigger Engine

↓

Queue

↓

Trigger Worker

↓

SQL Templates

↓

Subgraph Builder (NetworkX)

↓

Explanation Packet

↓

LLM Worker

↓

Workbench AML

↓

Feedback

---

# Alcance MVP

- Modelo canónico
- Grafo lógico en NetworkX
- Triggers AML
- Casos AML
- Explanation Packets
- Visualización HTML
- Feedback

---

# Casos AML MVP

- AML_CASH_IN_OUT_WINDOW
- AML_REPEATED_ROUND_TRIP
- AML_SHARED_EXTERNAL_ACCOUNT
- AML_NEW_EXTERNAL_ACCOUNT_WITHDRAWAL
- AML_KYC_INFLOW_MISMATCH
- AML_TOP_CONTRACT_FLOW_CONCENTRATION

---

# Principios

1. Redshift/Athena son la fuente de verdad.
2. NetworkX sólo calcula subgrafos temporales.
3. El LLM nunca consulta libremente los datos.
4. Todo pasa por Trigger Engine.
5. Toda explicación proviene de un Explanation Packet.
6. La decisión final siempre es humana.

---

# Equipos

Gobernanza de Datos
- Cliente
- Contratos
- KYC
- Movimientos

Global Task
- Ingesta
- Transformación

AML/TI
- Alertas
- Casos
- Watchlists

AI Data Science
- AI AML Layer
- Triggers
- Features
- Explanation Engine

AML Negocio
- Validación
- Feedback
- Operación

---

# Roadmap

Fase 1
- Datos
- NetworkX
- Triggers
- Casos
- LLM

Fase 2
- Órdenes
- Ejecuciones
- Fondos
- UBO

Fase 3
- Producción
- Tiempo real
- Optimización

---

# ADRs

ADR-001
AI AML Layer como arquitectura central.

ADR-002
Graph-as-compute con NetworkX.

ADR-003
Trigger Engine desacoplado mediante Queue.

ADR-004
Explanation Packet como contrato con el LLM.

ADR-005
HouseConcentrationAccount no participa como contraparte económica.

ADR-006
Human-in-the-loop obligatorio.
