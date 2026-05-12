# Boring Agent

Boring Agent is a self-hosted, self-improving agent home server. It is designed to live on a VPS, build and test its own capabilities, remember across time, communicate through normal-user channels like WhatsApp, and expose its inner state through a visual lab and operator dashboard.

The project thesis is simple: current agents are still bounded by the user's prompting skill, knowledge, tools, and workspace. Boring Agent tries to break that ceiling by making research-for-action, advanced memory, self-extension, and reflection native parts of the runtime.

## Current Status

This repository currently contains the SEED planning work for Boring Agent.

- Planning document: [`projects/boring-agent/PLANNING.md`](projects/boring-agent/PLANNING.md)
- Project type: Application
- Target deployment: single self-hosted VPS first
- Core runtime direction: Go orchestrator with polyglot sandboxed workers

## Core Ideas

- **Home server:** the VPS is the agent's persistent home, with identity, memory, files, traces, modules, policies, and dashboard state.
- **Research-for-action:** the agent should research docs, repos, issues, specs, examples, and prior memory to produce executable plans, tests, modules, and durable lessons.
- **First-class memory:** memory is not just RAG. It includes episodic, semantic, procedural, reflective, identity, and endocrine/control state.
- **Forgery Lab:** a sandbox where the agent can research, write, test, and register new capabilities.
- **Boring Blocks:** shareable capability packages with manifests, code, tests, policies, examples, and procedural memory.
- **Fractals:** bidirectional bridges into other runtimes such as ClickThrough, Claude Code, Hermes, OpenClaw, or Codex-style coding agents.
- **Endocrine Context Engine:** an experimental control layer that modulates research depth, caution, persistence, risk behavior, personality, UI mood, memory promotion, and self-improvement.
- **Normal View and Boring View:** a cute room/lab for normal users and a dense operator surface for traces, files, memory, blocks, sandbox runs, policies, and fractals.

## Intended Users

Boring Agent starts as dual dogfood:

- **Power user:** a technical owner who wants the agent to run continuously, build modules, inspect traces, attach to other runtimes, and self-improve aggressively.
- **Normal user:** a non-technical household user who can interact through WhatsApp and a friendly visual interface without prompt-engineering expertise.

## Planned Architecture

The intended architecture is a Go core with specialized subsystems around it:

- Go home orchestrator
- SQLite-first storage for durable state and memory search, behind an adapter so Postgres/vector backends can be added later
- Append-only event/trace log
- Web dashboard
- Official/managed WhatsApp adapter
- Sandboxed worker runner
- Forgery Lab for self-extension
- Boring Block package/runtime system
- MCP adapter layer for tool/context interoperability
- Native Fractal Session Protocol for richer runtime bridges

MCP is treated as an integration edge, not the internal nervous system. Missions, memory, capabilities, policies, and fractals need richer native semantics than plain tool calls.

## Phase Plan

### Phase 1: Home Server, Memory, Forge, and Normal Channel

Build the VPS home server, dashboard shell, event/trace runtime, first-class memory, WhatsApp channel, Research-for-Action pipeline, Forgery Lab, Boring Blocks v0, endocrine context, memory eval harness, and skeletal fractal protocol.

### Phase 2: Fractals and Stronger Self-Improvement

Add real adapters for external runtimes, richer artifact/memory merge flows, block import/export, registry/signature model, and stronger sandbox policies.

### Phase 3: Benchmark-Oriented Memory and Autonomy

Push memory evals beyond baseline RAG, deepen structured memory, improve conflict resolution and temporal reasoning, and harden autonomous self-update with canary and rollback.

### Phase 4: Computer-Use and Broader Body

Extend into ClickThrough/browser/computer-use surfaces so the agent can act beyond APIs and chat channels while preserving traceability.

## Security Posture

Phase 1 is personal but hardened, with no multi-tenancy.

- Strong admin auth
- Separate identities for users, channels, workers, blocks, and fractals
- Mission-scoped secret leases
- Sandboxed block execution
- Traced network activity in the Forgery Lab
- Configurable autonomy policies
- Non-optional audit logs
- Staged self-update with snapshot, canary, health checks, and rollback

## Next Steps

- Decide whether to run `/seed graduate boring-agent` or `/seed launch boring-agent`.
- Research official/managed WhatsApp integration options.
- Specify the Boring Block manifest and process protocol.
- Specify the Forgery Lab sandbox threat model.
- Define the Phase 1 memory schema and eval harness.
- Define the Fractal Session Protocol v0.
- Choose the first normal-user workflow and first power-user forge demo.
