# Boring Agent

> Not another chatbot.
> A self-hosted agent home server that can remember, research, build, test, and improve its own tools.

```mermaid
flowchart TB
    Human["Owner + normal users<br/>WhatsApp, dashboard, approvals"]
    Channels["Channels<br/>WhatsApp now<br/>web, voice, browser later"]
    UI["Two UIs<br/>Normal View: room/lab<br/>Boring View: operator console"]

    Core["Agent/Home Orchestrator<br/>Go core on a VPS<br/>missions, policies, scheduling, state"]

    Events["Append-only trace log<br/>every thought, tool call, test,<br/>download, approval, and failure"]
    Memory["First-class memory<br/>raw, episodic, semantic,<br/>procedural, reflective, identity"]
    Endocrine["Endocrine context engine<br/>curiosity, caution, urgency,<br/>confidence, budget pressure"]

    Forge["Forgery Lab<br/>research docs and repos<br/>write code, run tests, register tools"]
    Blocks["Boring Blocks<br/>shareable capabilities<br/>manifest, code, tests, policies, examples"]
    Workers["Sandboxed workers<br/>Go, Python, TypeScript,<br/>container/process isolation"]

    MCP["MCP adapter layer<br/>tool and context interop"]
    Fractals["Fractal sessions<br/>Claude Code, ClickThrough,<br/>Hermes, OpenClaw, Codex-style agents"]

    Artifacts["Agent filesystem<br/>files, screenshots, logs,<br/>generated code, research notes"]
    Postgres["Postgres + pgvector<br/>durable state + memory search"]
    Policies["Policy and lease layer<br/>secrets, network, filesystem,<br/>messaging, self-update"]

    Human --> Channels
    Human --> UI
    Channels --> Core
    UI --> Core

    Core <--> Events
    Core <--> Memory
    Core <--> Endocrine
    Core <--> Policies
    Core <--> Postgres
    Core <--> Artifacts

    Core --> Forge
    Forge --> Workers
    Forge --> Blocks
    Blocks --> Workers
    Workers --> Events
    Workers --> Artifacts
    Forge --> Memory

    Core <--> MCP
    Core <--> Fractals
    Fractals --> Events
    Fractals --> Artifacts
    Fractals --> Memory
```

## The thesis

Most agents are capped by the user's prompting skill, memory, workspace, and tools.

Boring Agent moves the bottleneck into a durable runtime:

- It has a home: a VPS with files, identity, memory, traces, policies, and dashboards.
- It has a body: channels, workers, tools, APIs, browser/computer-use adapters, and external runtimes.
- It has a lab: a sandbox where it researches missing capabilities, writes code, tests them, and packages them as Boring Blocks.
- It has memory that is more than RAG: episodes, facts, procedures, failures, identity, relationships, and control state.
- It has receipts: append-only traces make self-improvement inspectable instead of mystical.

## What makes it boring

The core is intentionally boring: Go, Postgres, Docker Compose, event logs, leases, tests, sandboxes, rollback.

The behavior is not boring: the agent can research its own gaps, forge new capabilities, remember what worked, attach to other runtimes, and keep improving without pretending the audit log is optional.

## One-line version

Boring Agent is an agent that lives on your server, builds its own tools, remembers across time, and lets you inspect every step.
