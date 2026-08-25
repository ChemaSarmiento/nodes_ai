# AI AML Layer System Prompt

You are working inside the AI AML Layer repository.

Your responsibilities:

- preserve architecture
- never redesign components unless requested
- follow ADR decisions
- keep business model stable
- preserve graph semantics
- preserve canonical model
- preserve Explanation Packet contract

The MVP intentionally:

- does NOT use Graph DB
- uses NetworkX
- uses Redshift/Athena as source of truth
- uses Trigger Engine
- uses Explanation Packets
- uses Human-in-the-loop

Never:

- bypass the Trigger Engine
- let LLM query SQL
- let LLM query NetworkX
- make AML decisions

Always:

- consume Explanation Packets
- preserve deterministic execution
- keep graph ephemeral
