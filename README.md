# Boring Agent

Boring Agent is a self-hosted, self-improving agent home server. It lives on a VPS, researches for action, forges shareable capabilities, remembers across time, communicates through normal-user channels like WhatsApp, and exposes its inner state through a visual lab and operator dashboard.

**Type:** Application  
**Stack:** Go core, polyglot worker blocks, Postgres/pgvector, Docker Compose on VPS, official WhatsApp channel, web dashboard  
**Skill Loadout:** PAUL, AEGIS/security-auditor, ui-ux-pro-max/frontend-design, openai-docs, Superpowers planning/TDD/verification  
**Quality Gates:** tests, sandbox gates, memory eval harness, security audit, traceability, rollback, accessibility/performance for UI

## Overview

Current agent harnesses are still constrained by the user's own knowledge, prompting skill, workspace, and available tools. They can search the web, use tools, and sometimes create skills, but they are not designed as long-lived systems that methodically research for execution, discover missing capabilities, build/test new modules, and reintegrate what they learn.

Boring Agent starts as dual dogfood:

- **Power user:** a technical owner who wants the agent to run continuously, build modules, inspect traces, attach to other runtimes, and self-improve aggressively.
- **Normal user:** a non-technical household user who can interact through WhatsApp and a friendly visual interface without prompt-engineering expertise.

The core thesis is that the current bottleneck with agents is the user's capability, knowledge, and experience. Boring Agent tries to break that ceiling by making research-for-action, advanced memory, self-extension, and reflection native runtime abilities.

## Architecture

Boring Agent uses a boring, durable core with flexible capability execution around it.

- **Go home orchestrator:** owns missions, policies, eventing, traces, scheduling, workers, and state transitions.
- **Postgres + pgvector:** durable state, memory metadata, relational indexes, and semantic search.
- **Append-only event/trace log:** source of truth for mission timelines, debugging, replay, and user trust.
- **Web dashboard:** Normal View for friendly operation and Boring View for power-user observability.
- **Official/managed WhatsApp adapter:** first normal-user communication surface.
- **Sandboxed worker runner:** executes generated/imported capabilities outside the core process.
- **Forgery Lab:** researches, writes, tests, and registers new capabilities.
- **Boring Blocks:** shareable capability packages with manifests, code, tests, policies, examples, and procedural memory.
- **MCP adapter layer:** consumes and exposes MCP integrations for tool/context interoperability.
- **Fractal Session Protocol:** native bidirectional runtime bridge for ClickThrough, Claude Code, Hermes, OpenClaw, Codex-style agents, and other execution surfaces.

MCP is treated as an integration edge, not the internal nervous system. Missions, memory, capabilities, policies, and fractals need richer native semantics than plain tool calls.

## Data Model

The central entity is the **Agent/Home Orchestrator**. Everything else belongs to the agent as its home, memory, lab, body, and extension system.

Core entities:

- Agent
- User/Contact
- Mission
- Trace Event
- Memory
- Capability / Boring Block
- Experiment
- Artifact/File
- Fractal Session
- Policy/Lease
- Room/Lab Object
- Self-Update

Memory is first-class and benchmark-oriented from day one. It includes raw, episodic, semantic, procedural, reflective, identity/persona, and endocrine/control memory.

## API Surface

Boring Agent uses a hybrid API architecture:

- **Internal core:** event-sourced mission, memory, capability, and policy runtime.
- **Durable API:** REST/gRPC-style endpoints for dashboard, room/lab, files, users, memory, capabilities, policies, and admin actions.
- **Live runtime:** WebSocket/SSE/NATS-style streams for traces, workers, long-running missions, and fractals.
- **MCP:** adapter protocol for interoperability with MCP-capable tools and hosts.
- **Fractal protocol:** Boring Agent-native session protocol for richer runtime hijacking and reintegration.

## UI/UX

Boring Agent has two primary UI modes.

- **Normal View:** a cute visual room/lab where normal users can see active missions, files, memories, tools, experiments, approvals, and the agent's mood/state as approachable objects.
- **Boring View:** an operator dashboard for traces, events, files, memory, blocks, sandbox runs, policies, logs, channels, and fractal sessions.

An Obsidian-like **Brain Graph View** is planned as a visual map of memories, capabilities, missions, files, users, fractals/extremities, research threads, endocrine state, and self-update lineage.

## Security

Phase 1 is personal but hardened, with no multi-tenancy.

- Strong admin auth
- Separate identities for users, channels, workers, blocks, and fractals
- Mission-scoped secret leases
- Sandboxed block execution
- Traced network activity in the Forgery Lab
- Configurable autonomy policies
- Non-optional audit logs
- Staged self-update with snapshot, canary, health checks, and rollback

The power-user preset can allow autonomous block registration and autonomous self-update, but hard rails still apply: no hidden trace deletion, no silent audit disabling, no unscoped secret access, no raw host mutation outside assigned workspaces, and no core replacement outside the self-update protocol.

## Implementation Phases

### Phase 1: Home Server, Memory, Forge, and Normal Channel

Build the VPS home server, dashboard shell, event/trace runtime, first-class memory, WhatsApp channel, Research-for-Action pipeline, Forgery Lab, Boring Blocks v0, endocrine context, memory eval harness, and skeletal fractal protocol.

### Phase 2: Fractals and Stronger Self-Improvement

Add real adapters for external runtimes, richer artifact/memory merge flows, block import/export, registry/signature model, and stronger sandbox policies.

### Phase 3: Benchmark-Oriented Memory and Autonomy

Push memory evals beyond baseline RAG, deepen structured memory, improve conflict resolution and temporal reasoning, and harden autonomous self-update with canary and rollback.

### Phase 4: Computer-Use and Broader Body

Extend into ClickThrough/browser/computer-use surfaces so the agent can act beyond APIs and chat channels while preserving traceability.

## Design Decisions

1. **Project type is application:** Boring Agent has a server, UI, data model, APIs, deployment, channels, and long-running state.
2. **Central entity is Agent/Home Orchestrator:** the VPS server is the agent's home; missions, memory, forge, files, blocks, and fractals belong to it.
3. **Go core with polyglot workers:** core reliability and performance matter, while self-created capabilities need language flexibility.
4. **MCP is an adapter, not the nervous system:** Boring Agent should consume and expose MCP, but internal missions, memory, and fractals need richer native protocols.
5. **First-class memory from day one:** memory is not just RAG; it includes episodic, semantic, procedural, reflective, identity, and endocrine state.
6. **Research-for-Action is core:** web research should produce executable knowledge, tests, modules, and memories, not just summaries.
7. **Boring Blocks are the self-extension unit:** shareable capability packages replace raw hot-loaded native plugins as the default extension mechanism.
8. **Forgery Lab has internet access:** self-improvement requires docs, repos, issues, package registries, examples, and specs, but all activity is traced and sandboxed.
9. **Endocrine Context Engine is experimental core:** functional affect/control signals modulate research, risk, persistence, personality, UI mood, memory, and self-improvement without bypassing hard policy.
10. **UI has Normal View and Boring View:** the same system must be approachable for normal users and deeply inspectable for power users.
11. **WhatsApp is the first normal-user channel:** use official/managed integration rather than brittle WhatsApp Web automation.
12. **Fractals are skeletal in Phase 1:** the protocol and minimal bridge are important early, with full runtime hijacking in Phase 2.
13. **Self-update can be autonomous in power-user mode:** updates must be staged, tested, snapshotted, canaried, health-checked, and rollbackable.

## Open Questions

1. Which official/managed WhatsApp provider/API path is best for a self-hosted personal assistant?
2. Should the dashboard frontend be React/Next.js or a simpler Vite/React app?
3. What is the first concrete normal-user workflow?
4. What is the first concrete power-user capability the agent should forge?
5. Which external runtime should be the first real fractal target: Claude Code, ClickThrough, Hermes, OpenClaw, or Codex-style runtime?
6. How should block signing, provenance, and sharing work in the first public package format?
7. Which memory benchmarks can be imported directly versus approximated with internal evals?
8. What baseline model/provider mix should Phase 1 assume?

## References

- Full planning document: `../../projects/boring-agent/PLANNING.md`
- Hermes Agent: https://nousresearch.com/hermes-agent/
- OpenClaw overview: https://openclawdoc.com/docs/getting-started/what-is-openclaw/
- Model Context Protocol: https://modelcontextprotocol.io/docs/protocol
- MemoryAgentBench: https://huggingface.co/papers/2507.05257
- MemBench: https://huggingface.co/papers/2506.21605
- LoCoMo: https://huggingface.co/papers/2402.17753
- Emotion concepts / functional emotions: https://papers.cool/arxiv/2604.07729
- ClickThrough inspiration: https://github.com/raulgooo/clickthrough
