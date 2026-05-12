# Harness Competitive Analysis: OpenClaw, Hermes Agent, and Boring Agent

**Date:** 2026-05-12  
**Goal:** Design Boring Agent as a self-hosted, self-improving harness that can outperform OpenClaw and Nous Research's Hermes Agent by making research-for-action, autonomous capability forging, staged self-modification, memory evaluation, and external runtime bridging native to the core.

## Executive Takeaway

OpenClaw and Hermes are both strong self-hosted agent systems, but their center of gravity differs:

- **OpenClaw** is primarily a multi-channel gateway and runtime-router for AI agents. Its strongest ideas are typed gateway ownership, channel breadth, agent runtime/harness separation, plugin packaging, configurable tool policy, cron, mobile/device nodes, and explicit transcript/session ownership.
- **Hermes Agent** is primarily a batteries-included autonomous agent runtime. Its strongest ideas are a unified `AIAgent`, broad tool surface, many terminal and browser backends, platform gateway, persistent memory, skill creation, skill hub integration, session search, cron, ACP/editor integration, and research/training export.
- **Boring Agent** should not compete by merely adding more tools. It should compete by making the missing control plane explicit: mission semantics, append-only traces, memory typed by purpose, forge experiments, block provenance, staged self-update, rollback, benchmarked learning, and fractal bridges into other runtimes.

The win condition is a harness that treats every capability change as a researched, tested, policy-gated, traceable software supply-chain event.

## Core Thesis: Benchmark-Driven Self-Improvement Without Training

Boring Agent's core bet is that an agent system can improve over time without changing model weights if the harness itself behaves like a lab.

The loop is:

1. Observe failures, bottlenecks, repeated tasks, and missed opportunities.
2. Convert them into concrete benchmarks or eval cases.
3. Form hypotheses about what would improve performance.
4. Generate one or more candidate solutions: prompts, tools, Boring Blocks, memory policies, retrieval strategies, planning policies, UI flows, or orchestration changes.
5. Run candidates against baseline and regression suites.
6. Promote the winner only if it beats the current system under policy gates.
7. Preserve failed attempts as artifacts and reflective memory.
8. Re-run benchmarks over time as models, tools, data, and user needs change.

This is different from ordinary "agent memory" or "skill creation." The harness must own an experimental method:

- Every improvement proposal has a hypothesis.
- Every hypothesis has a measurable eval.
- Every eval has fixtures, scoring, and regression protection.
- Every promoted change has provenance, trace, rollback, and postmortem.

The agent does not need to train a model to get better. It can improve the surrounding system: context selection, memory promotion, tool choice, block implementation, planning strategy, retry policy, sandbox policy, and runtime routing. Over many cycles, that makes the whole agent more competent even if the underlying model is unchanged.

This also gives Boring Agent a stronger claim than OpenClaw or Hermes. OpenClaw is excellent at routing runtimes through a gateway. Hermes is excellent at broad autonomous execution and skill use. Boring Agent should be the harness that continuously turns experience into benchmarks, benchmarks into experiments, and experiments into safer, tested system changes.

## Sources Checked

Primary/current sources:

- OpenClaw overview: `https://docs.openclaw.ai/`
- OpenClaw architecture: `https://docs.openclaw.ai/concepts/architecture`
- OpenClaw agent runtimes: `https://docs.openclaw.ai/concepts/agent-runtimes`
- OpenClaw agent harness SDK: `https://docs.openclaw.ai/plugins/sdk-agent-harness`
- OpenClaw tools/plugins: `https://docs.openclaw.ai/tools`
- OpenClaw memory: `https://docs.openclaw.ai/concepts/memory`
- OpenClaw skills: `https://docs.openclaw.ai/tools/skills`
- OpenClaw cron: `https://docs.openclaw.ai/automation/cron-jobs`
- Hermes docs: `https://hermes-agent.nousresearch.com/docs/`
- Hermes GitHub repo: `https://github.com/NousResearch/hermes-agent`
- Hermes architecture: `https://hermes-agent.nousresearch.com/docs/developer-guide/architecture`
- Hermes memory: `https://hermes-agent.nousresearch.com/docs/user-guide/features/memory/`
- Hermes skills: `https://hermes-agent.nousresearch.com/docs/skills/`
- Hermes skill creation: `https://hermes-agent.nousresearch.com/docs/developer-guide/creating-skills`
- Hermes security policy: `https://github.com/NousResearch/hermes-agent/security`
- Evokoa: `https://evokoa.com/`
- Evokoa manifesto: `https://evokoa.com/manifesto/`
- Evokoa architecture: `https://evokoa.com/blog/the-architecture-behind-evokoa/`

## OpenClaw: What It Actually Is

OpenClaw presents itself as a self-hosted gateway connecting chat and device surfaces to AI coding agents. It runs one long-lived gateway across channels such as Discord, Google Chat, iMessage, Matrix, Microsoft Teams, Signal, Slack, Telegram, WhatsApp, Zalo, WebChat, and mobile nodes. Its docs describe the gateway as the single source of truth for sessions, routing, and channel connections.

### Core Architecture

OpenClaw's gateway daemon:

- Maintains provider/channel connections.
- Exposes a typed WebSocket API.
- Validates inbound frames against JSON Schema.
- Emits events such as `agent`, `chat`, `presence`, `health`, `heartbeat`, and `cron`.
- Uses pairing and auth flows for clients/nodes.
- Owns channel delivery, session routing, and control-plane access.

This is a strong architecture for communication surfaces. It is less obviously a deep self-improvement substrate.

### Runtime and Harness Model

OpenClaw distinguishes:

- **Provider:** auth/model discovery/model naming.
- **Model:** selected model.
- **Agent runtime:** low-level execution loop/backend.
- **Channel:** where messages enter and leave.

The docs call the implementation of an agent runtime a **harness**. A harness executes one prepared turn; it does not own provider selection, channel delivery, tool registry, or model fallback policy. OpenClaw core resolves workspace, sandbox, tool policy, callbacks, transcript/session, thinking level, context budget, model fallback, and live switching before the harness runs.

Important implication: OpenClaw's harness model is intentionally narrow. It is a runtime adapter, not a full mission engine.

### Tools, Skills, Plugins

OpenClaw has a clean three-layer model:

- **Tools:** typed functions such as `exec`, `browser`, `web_search`, `message`, `cron`, `gateway`, `memory_search`, `apply_patch`.
- **Skills:** `SKILL.md` prompt documents that teach tool use.
- **Plugins:** packages that can add channels, providers, tools, skills, media, speech, web search/fetch, realtime, and harnesses.

This separation is worth copying. Boring Agent should preserve it, but add stronger provenance, testing, and permission metadata.

### Memory

OpenClaw memory is local and legible:

- `MEMORY.md` for durable long-term memory.
- `memory/YYYY-MM-DD.md` for daily notes.
- `DREAMS.md` for reviewable consolidation.
- Optional Memory Wiki layer with claims, evidence, contradiction/freshness tracking, dashboards, and wiki tools.
- SQLite/hybrid search backends are available.

This is pragmatic and inspectable. Its weakness for Boring Agent's goal is that memory is still mostly file/wiki oriented, not a first-class typed control plane with benchmarked retrieval, conflict handling, procedural promotion, and mission-linked provenance baked into the runtime.

### Scheduling and Background Work

OpenClaw cron runs inside the gateway, persists job definitions, tracks background task records, supports one-shot/interval/cron expressions, can use isolated sessions, and delivers to chat channels or webhooks. This is solid, especially the distinction between main, current, custom, and isolated sessions.

### Strengths to Steal

- Gateway is the single owner of channels and sessions.
- Runtime/harness separation is explicit.
- Tool/skill/plugin layering is clear.
- Typed WebSocket protocol and schema validation.
- Configurable tool profiles, allow/deny lists, and grouped tools.
- Cron is runtime-owned and persistent.
- Memory is inspectable and local-first.
- Session transcript mirror protects compatibility when native runtimes own threads.

### Gaps Boring Agent Can Exploit

- Harnesses execute prepared turns; they do not own long-horizon missions.
- Self-improvement through skills exists, but full module forging/testing/registration is not the core abstraction.
- Memory is not typed deeply enough for benchmark-oriented lifelong learning.
- Native runtime bridges are adapters, not bidirectional artifact/memory merge protocols.
- Gateway/control UI is not a full operator lab for traces, experiments, policies, block lineage, canary updates, and memory evaluation.
- Self-update appears as gateway update tooling, not an autonomous staged software evolution pipeline.

## Hermes Agent: What It Actually Is

Hermes is a full autonomous agent runtime by Nous Research. Its docs emphasize a closed learning loop: persistent memory, self-created skills, skill improvement, session search, user modeling, and cross-platform availability.

### Core Architecture

Hermes centers on `AIAgent` in `run_agent.py`, used by multiple entry points:

- CLI.
- Messaging gateway.
- ACP adapter.
- Batch runner.
- API server.
- Python library.

Major subsystems:

- Prompt builder.
- Provider resolution.
- Tool dispatch.
- Context compression and prompt caching.
- SQLite + FTS5 session storage.
- Tool backends for terminal, browser, web, MCP, file, vision.
- Gateway with many platform adapters.
- Cron.
- Plugins.
- RL/data-generation environments.

Hermes' architecture is more agent-runtime-centric than OpenClaw's gateway-centric design.

### Tool and Backend Surface

Hermes advertises 70+ tools, multiple terminal backends, browser backends, web backends, dynamic MCP, file tools, vision, image/TTS, subagent delegation, and code execution. Terminal backends include local, Docker, SSH, Daytona, Modal, Singularity, and Vercel Sandbox. This is a strong execution portfolio.

### Memory

Built-in Hermes memory is bounded and curated:

- `MEMORY.md`: personal/environment/project notes, around 2,200 chars.
- `USER.md`: user profile/preferences, around 1,375 chars.
- Both injected as frozen prompt snapshots at session start for cache stability.
- Agent manages entries through a `memory` tool.
- Memory entries are scanned for injection/exfiltration patterns.
- SQLite session search with FTS5 provides broader recall beyond active memory.
- External memory providers include Honcho, OpenViking, Mem0, Hindsight, Holographic, RetainDB, ByteRover, and Supermemory.

Hermes optimizes for bounded prompt memory plus searchable session history. Boring Agent should go further: memory records must be entities with types, source links, confidence, decay, contradiction state, embeddings, graph edges, mission provenance, eval labels, and promotion policies.

### Skills and Self-Improvement

Hermes treats skills as preferred capability extensions when the capability can be expressed as instructions plus shell/API usage. Skills are easier than tools and shareable via a hub. Tool implementations are reserved for precise integrations, auth, binary/streaming/realtime, or complex processing.

Hermes also has a large built-in and community skill ecosystem. Its docs list hundreds of skills across built-in, optional, and community registries.

This is powerful, but skill creation is still mostly procedural memory. Boring Blocks should be a stronger unit:

- Manifest.
- Code.
- Tests.
- Permissions.
- Runtime.
- Examples.
- Provenance.
- Threat model.
- Evaluation results.
- Memory/procedure notes.
- Upgrade/migration instructions.
- Signature/trust metadata.

### Plugins

Hermes plugins are discovered from user, project, and pip-entry-point locations. Plugins register tools, hooks, CLI commands, memory providers, and context engines. Memory provider and context engine plugins are single-select.

This is mature enough to learn from, but Boring Agent should avoid letting arbitrary plugin code become the default self-extension path. Generated/imported blocks should run outside the core process unless elevated through explicit promotion.

### Security

Hermes' security policy states a personal single-operator trust model. It protects the operator from LLM actions, not from malicious co-tenants. Local terminal execution is default; container isolation is opt-in. Dangerous command approval is a core boundary. Skills guard and MCP package checks help with supply chain.

For Boring Agent's stated ambition, this is too permissive as a default. Boring Agent needs personal deployment ergonomics, but its core should assume generated code, third-party skills, web content, package registries, and external runtimes are adversarial until policy grants scoped leases.

### Research/Training

Hermes includes batch trajectory generation, Atropos RL integration, ShareGPT trajectory export, and tool-call parsers. This is a major advantage for model-training loops. Boring Agent should not ignore this; it should make evaluation and trajectory export first-class for harness behavior, memory, and forge outcomes.

### Strengths to Steal

- One core agent loop serving CLI, gateway, ACP, batch, and API.
- Broad terminal/browser/web/MCP backend support.
- Strong skill ecosystem and progressive skill disclosure.
- Built-in skill creation path.
- Session search with FTS5.
- Provider-agnostic model/runtime resolution.
- Cron as first-class agent tasks.
- ACP/editor integration.
- RL trajectory export and Atropos integration.
- Profile isolation.

### Gaps Boring Agent Can Exploit

- Agent loop is large and centralized; harder to reason about as a durable home-server control plane.
- Memory is bounded prompt memory plus session search, not a typed memory/event/missions database.
- Skills are useful but lighter than tested, permissioned, versioned modules.
- Local execution default is convenient but risky for autonomous self-modification.
- Self-improvement focuses on skills and prompts more than staged core/module mutation with canary, health checks, and rollback.
- Operator UI is not framed as a full lab for mission replay, policy review, block provenance, memory contradiction, and autonomous update lineage.

## Direct Comparison

| Dimension | OpenClaw | Hermes Agent | Boring Agent Target |
|---|---|---|---|
| Center of gravity | Gateway and runtime routing | Full autonomous agent runtime | Durable home orchestrator and self-improvement lab |
| Core runtime | Gateway owns sessions/channels; harness runs prepared turn | `AIAgent` owns prompt/provider/tool loop | Mission engine owns objective, policy, trace, workers, memory, forge, rollback |
| Extension unit | Plugins + tools + skills | Tools + plugins + skills | Boring Blocks with manifest/code/tests/policy/provenance/evals |
| Self-improvement | Skill Workshop / memory / update tooling | Skill creation and skill improvement | Research-for-action forge, module generation, tests, canary, rollback, memory promotion |
| Memory | Markdown, daily notes, dreams, optional wiki/search | Bounded memory files, USER profile, FTS5 sessions, external providers | Typed episodic/semantic/procedural/reflective/identity/endocrine memory with evals |
| Scheduling | Gateway cron with isolated/custom sessions | Agent cron with platform delivery | Mission scheduler with persistent objectives, leases, artifacts, postmortems |
| Sandbox | Tool policy and optional sandboxes | Local default; Docker/SSH/Modal/etc available | Sandbox-first workers for generated/imported code; host mutation only through update protocol |
| Runtime bridges | Harnesses and CLI backends | ACP, subagents, terminal backends, MCP | Fractal Session Protocol with bidirectional trace/artifact/memory merge |
| Observability | Typed events, transcripts, dashboard | Visible tool callbacks, sessions | Append-only event log, trace replay, lab dashboards, block lineage, memory lineage |
| Research/training | Not the apparent focus | Strong trajectory/RL story | Use trajectories and evals to improve harness, memory, planning, and block quality |

## Boring Agent Architecture Implications

### 1. Mission Engine Must Be the Core Abstraction

Do not make "chat turn" the core unit. A chat turn is just one way to create or steer a mission.

Minimum mission fields:

- Objective.
- Source channel/user.
- Current plan.
- Risk class.
- Budget.
- Model policy.
- Memory context.
- Capability inventory.
- Leases.
- Trace stream.
- Artifacts.
- Experiments.
- Postmortem.

This lets Boring Agent outperform both systems on long-horizon work, because it can keep reasoning, research, tests, artifacts, and memories tied to the objective instead of scattered across chat sessions.

### 2. Separate Core, Workers, Blocks, and Fractals

The Go core should own:

- Mission state machine.
- Policy decisions.
- Leases.
- Event/trace writes.
- Memory writes/promotions.
- Block registration status.
- Self-update lifecycle.
- Fractal session contracts.

Workers should own:

- Running code.
- Research/build/test sandboxes.
- Tool-specific subprocesses.
- Block execution.

Blocks should be data/code packages, not arbitrary core plugins by default.

Fractals should bridge into external runtimes such as Hermes, OpenClaw, Codex, Claude Code, or ClickThrough, with explicit context packages and returned artifacts/traces.

### 3. Boring Blocks Should Be Stronger Than Skills

OpenClaw and Hermes both use AgentSkills-style `SKILL.md`. Boring Agent should consume that format but not stop there.

Proposed Boring Block v0:

```yaml
name: github-pr-triage
version: 0.1.0
kind: workflow
runtime: container
entrypoint: run.py
permissions:
  network:
    allow:
      - api.github.com
  secrets:
    - github.token:read
  filesystem:
    write:
      - artifacts/
tests:
  command: pytest
examples:
  - examples/triage-small.json
provenance:
  created_by_mission: mission_...
  sources:
    - https://docs.github.com/...
policy:
  auto_register: low-risk-after-tests
```

Required files:

- `BLOCK.md` or `SKILL.md` for procedural instructions.
- `block.yaml` manifest.
- `src/`.
- `tests/`.
- `examples/`.
- `POLICY.md`.
- `PROVENANCE.md`.
- `EVALS.md`.

### 4. Forge Pipeline Must Be Native

The Forgery Lab should be a state machine, not a prompt convention:

1. Gap detection: mission needs capability not available.
2. Research: docs/repos/issues/examples.
3. Spec: exact block interface, inputs, outputs, permissions.
4. Threat model: secrets, network, filesystem, injection, supply chain.
5. Implementation: sandbox workspace.
6. Tests: unit, integration, fixture, smoke.
7. Evaluation: task-specific success criteria.
8. Registration proposal: manifest + provenance + policy.
9. Activation: auto or approval based on risk.
10. Reflection: memory writes and procedural updates.

This is where Boring Agent can beat "self-improving skills" by shipping actual tested capability packages.

### 5. Self-Modification Must Be Staged Like Production

Autonomous core/module modification should only happen through a self-update protocol:

1. Snapshot current core, DB schema version, config hash, block registry, and health baseline.
2. Create update branch/worktree.
3. Generate or apply patch.
4. Run unit/integration/security/memory eval gates.
5. Start canary core instance against copied or migration-safe state.
6. Replay representative missions/traces.
7. Run health checks.
8. Promote or rollback.
9. Preserve failed implementation as artifact and reflective memory.

Hard rails:

- No deleting audit trails.
- No disabling policy silently.
- No host mutation outside declared workspace.
- No raw secrets in prompts/traces.
- No replacing the running core without snapshot/canary/rollback.

### 6. Memory Must Be Benchmark-Oriented

Use files for inspectability, store memory as typed records in SQLite through a storage adapter, and use Evokoa as the relationship layer over those records.

Evokoa's model fits Boring Agent better than a separate graph database. Its public architecture describes a compact in-memory relationship cache over existing source databases: traverse IDs, relationships, access rules, and metadata first, then hydrate only the relevant rows from SQLite or another source system. For Boring Agent, that means SQLite remains the Phase 1 system of record for traces, missions, memories, artifacts, users, blocks, policies, and evals, while Evokoa provides fast graph-shaped retrieval across them. The core should access SQLite through a storage adapter so Postgres, a vector store, or another backend can be added later without rewriting mission/memory logic.

Memory types:

- Raw: traces, messages, logs, files.
- Episodic: what happened.
- Semantic: durable facts.
- Procedural: workflows, blocks, commands.
- Reflective: failures, contradictions, lessons.
- Identity: agent/user profiles and boundaries.
- Endocrine/control: caution, curiosity, urgency, confidence, budget pressure.

Each memory should include:

- Source event/artifact.
- Confidence.
- Recency.
- Expiry/decay.
- Embedding.
- Graph links.
- Contradiction links.
- Promotion state.
- Eval labels where relevant.

This beats both systems if it is backed by memory tests: retrieval accuracy, temporal consistency, contradiction handling, procedural reuse, relationship traversal quality, and "learned capability actually improves future mission success."

Evokoa should make the following queries cheap enough to become normal agent behavior:

- "Find missions where this user had a similar goal, the first solution failed, and the second solution passed."
- "Find memories contradicted by newer evidence from the same source or a higher-trust source."
- "Find all blocks, policies, secrets, tests, and failures connected to this capability."
- "Find the shortest useful path from this user request to prior procedures, artifacts, and eval cases."
- "Find benchmark cases that cover this proposed self-update."

The key design point: Evokoa is not the memory itself. It is the traversal layer that makes Boring Agent's memory usable as a connected system.

### 7. Fractal Session Protocol Should Be Richer Than MCP

MCP is a tool/context edge. It is not enough for controlling whole agent runtimes.

Fractal protocol v0 should include:

- Runtime identity.
- Mission scope.
- Context package.
- Lease bundle.
- Heartbeat.
- Bidirectional messages.
- Tool/context requests back to home.
- Trace stream.
- Artifact return.
- Memory merge proposals.
- Cancellation/interruption.
- Sandbox/risk metadata.
- Native thread/session binding.

OpenClaw's harness docs are useful here: if an external runtime owns native thread state, Boring Agent should mirror and bind it, not pretend to own unsupported internals.

## Concrete Competitive Strategy

### Beat OpenClaw

OpenClaw is strongest at gateway/routing. Boring Agent can beat it by:

- Using official/managed WhatsApp first instead of brittle consumer-web automation where possible.
- Making the channel layer boring and reliable, not the product center.
- Building a stronger internal mission/trace/forge model.
- Providing OpenClaw as a fractal target, not a competitor to reimplement immediately.
- Supporting typed gateway/event APIs, but making event replay durable.

### Beat Hermes

Hermes is strongest at autonomous runtime breadth and skills. Boring Agent can beat it by:

- Making self-improvement a tested software pipeline, not mostly skill evolution.
- Making memory structured, queryable, evaluated, and mission-linked.
- Running generated/imported code sandbox-first by default.
- Creating a visual operator lab for traces, blocks, policies, experiments, and self-update lineage.
- Treating Hermes as a callable fractal runtime for jobs it already does well.

### Avoid Losing to Both

Boring Agent must not spend Phase 1 cloning every channel/tool/backend. That race is a trap.

Phase 1 should prove:

- One normal-user channel.
- One excellent dashboard shell.
- One durable mission engine.
- One memory schema and eval harness.
- One forge path that creates a tested block.
- One self-update simulation with rollback.
- One skeletal fractal adapter.

## Proposed Phase 1 Harness Design

### Services

- `core`: Go mission orchestrator.
- `dashboard`: React/Vite or Next app.
- `storage`: SQLite database file/volume for state, memory, trace indexes, FTS/search, and snapshots.
- `worker-runner`: container/process executor.
- `whatsapp-adapter`: official/managed channel adapter.
- `artifact-store`: filesystem first.
- `reverse-proxy`: TLS and routing.

### Core Packages

- `mission`: mission state machine.
- `trace`: append-only event writer/reader.
- `memory`: typed memory records and retrieval.
- `policy`: leases, approvals, risk gates.
- `blocks`: manifest, registry, invocation.
- `forge`: experiment lifecycle.
- `workers`: job dispatch and sandbox protocol.
- `channels`: inbound/outbound normalized messages.
- `fractal`: external runtime sessions.
- `selfupdate`: snapshot/canary/rollback.

### First Demo Mission

"Mom asks over WhatsApp: remind me tomorrow morning to send Raul the grocery list, and if you do not know the list format, ask Raul."

This exercises:

- Normal channel.
- User/contact memory.
- Scheduling.
- Clarification.
- Trace UI.
- Policy for outbound message.

### First Forge Demo

"Power user asks: build a block that watches a GitHub repo for stale issues and sends a weekly triage summary."

This exercises:

- Research-for-action.
- GitHub docs.
- Generated code.
- Secret lease.
- Tests.
- Cron integration.
- Block registration.
- Procedural memory.

### First Self-Update Demo

"Improve the block test runner timeout handling."

This should:

- Create branch/worktree.
- Modify only worker/block-runner code.
- Run tests.
- Start canary worker.
- Replay a simple block invocation.
- Promote or rollback.
- Preserve trace.

## High-Risk Design Decisions

1. **Go core vs Python core:** Go is right for the orchestrator, but do not force generated capabilities into Go.
2. **Official WhatsApp vs consumer automation:** official/managed is safer but may have cost/product constraints. Abstract channel adapter early.
3. **SQLite first:** correct for a single-owner VPS. Use a storage adapter from day one so the system can later switch selected stores to Postgres/vector backends if scale or multi-user requirements justify it.
4. **Autonomous self-modification:** allowed only in power-user mode with hard rails. This is product behavior, not a hidden hack.
5. **Fractals:** keep skeletal in Phase 1. Full Hermes/OpenClaw adapters belong after the core mission model is real.
6. **MCP:** support it, but do not let MCP become the internal architecture.

## Immediate Implementation Recommendations

1. Add `apps/boring-agent/core` with a minimal Go service exposing mission creation, trace append/query, health, and event stream.
2. Add Docker Compose with core, dashboard, worker-runner, and a mounted SQLite data volume.
3. Define the trace event schema before any UI.
4. Define `block.yaml` v0 and a runner protocol.
5. Build a toy block and force it through the same registration path future generated blocks will use.
6. Add a memory schema and one tiny eval suite before adding complex recall logic.
7. Add self-update simulation as a first-class workflow early, even if it only updates a toy module.

## Bottom Line

OpenClaw is the better gateway/harness-router reference. Hermes is the better autonomous runtime/skills/research reference. Boring Agent should be the better **self-improving home-server control plane**.

The differentiator is not "more tools." It is the combination of:

- Durable missions.
- Append-only traces.
- Typed memory with evals.
- Sandboxed forge.
- Tested capability packages.
- Policy-scoped leases.
- Fractal runtime bridges.
- Staged autonomous self-update.

That combination is the credible path to beating both harnesses.
