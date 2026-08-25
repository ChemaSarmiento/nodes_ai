# AI AML Layer — LLM Context Pack

Version: 1.0

---

# Purpose

This directory contains the minimum context required for any Large Language Model to understand the AI AML Layer project.

It is **not** intended for humans.

It is optimized for AI assistants.

Supported assistants

- ChatGPT
- Claude Code
- Codex
- Gemini
- Cursor
- Windsurf
- Continue.dev

---

# Load Order

Always load files in this order.

```
SYSTEM_PROMPT.md

↓

PROJECT_CONTEXT.md

↓

ARCHITECTURE_SUMMARY.md

↓

BUSINESS_MODEL.md

↓

GRAPH_MODEL.md

↓

RULES.md

↓

DECISIONS.md

↓

CURRENT_STATE.md

↓

LLM_CONTEXT.yaml
```

---

# Philosophy

The goal is minimizing prompt size while maximizing architectural understanding.

Large documents inside `/docs` should not be loaded unless deeper detail is required.

---

# Repository

```
docs/

↓

Reference Documentation

llm/

↓

Compressed AI Context
```

---

# Usage

Claude Code

```
Read every file under /llm before writing code.
```

Cursor

```
Index only /llm for initial context.
```

Codex

```
Use PROJECT_CONTEXT.md as system prompt.
```

---

# Contents

SYSTEM_PROMPT.md

General AI behavior.

PROJECT_CONTEXT.md

High-level project.

ARCHITECTURE_SUMMARY.md

Architecture.

BUSINESS_MODEL.md

Business entities.

GRAPH_MODEL.md

Graph.

RULES.md

Architecture rules.

DECISIONS.md

Architecture decisions.

CURRENT_STATE.md

Current implementation.

LLM_CONTEXT.yaml

Machine-readable summary.
