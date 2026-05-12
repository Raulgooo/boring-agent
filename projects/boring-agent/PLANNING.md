# Boring Agent

> A self-hosted, self-improving agent home server that researches for action, forges shareable capabilities, operates through friendly channels, and exposes its inner state through a visual lab and operator dashboard.

**Created:** 2026-05-10  
**Type:** Application  
**Stack:** Go core, polyglot worker blocks, SQLite-first storage adapter, Docker Compose on VPS, official WhatsApp channel, web dashboard  
**Skill Loadout:** PAUL, AEGIS/security-auditor, ui-ux-pro-max/frontend-design, openai-docs, Superpowers planning/TDD/verification  
**Quality Gates:** tests, sandbox gates, memory eval harness, security audit, traceability, rollback, accessibility/performance for UI

---

## Problem Statement

Current agent harnesses are still constrained by the user's own knowledge, prompting skill, workspace, and available tools. They can search the web, use tools, and sometimes create skills, but they are not designed as long-lived systems that methodically research for execution, discover missing capabilities, build/test new modules, and reintegrate what they learn.

Boring Agent is for two initial users:

- **Raul / power user:** wants a VPS-native agent that can run continuously, self-improve, build modules, attach to other runtimes, inspect traces, and operate aggressively.
- **Mom / normal user:** wants a dead-simple assistant reachable through WhatsApp and a friendly visual interface, without needing prompt engineering or technical setup.

The core thesis is that the current bottleneck with agents is the user's capability, knowledge, and experience. Boring Agent should break that ceiling by making research-for-action, memory, self-extension, and reflection native runtime abilities.

---

## Tech Stack

Boring Agent should use a boring, durable core with flexible capability execution around it.

| Layer | Choice | Rationale |
|-------|--------|-----------|
| Frontend | Web dashboard, likely React/TypeScript | Best fit for a rich Normal View, Boring View, trace UI, filesystem browser, and graph visualization. |
| Backend | Go core orchestrator | Strong performance, concurrency, long-running service reliability, and a good fit for scheduling, eventing, workers, policies, and trace pipelines. |
| Workers | Polyglot process/container blocks | Lets the agent create capabilities in Go, Python, TypeScript, or other languages without coupling them to the core process. |
| Database | SQLite-first storage adapter | Durable state, relational metadata, trace indexing, memory records, FTS/search, easy snapshots, and simpler self-hosted operation on a single VPS. Keep storage interfaces abstract so Postgres/vector backends can be added later if needed. |
| Object/Artifact Storage | Filesystem first; S3-compatible later | The VPS is the agent's home. Files, generated code, screenshots, logs, and artifacts should be browsable and portable. |
| Event/Streaming | Internal append-only event log; WebSocket/SSE/NATS-style streams | Missions, traces, fractals, workers, and UI need live events and replayable history. |
| Deployment | Docker Compose on a single VPS | Phase 1 target is one self-hosted server where the agent lives, dreams, experiments, and stores identity/memory. |
| Integrations | Official/managed WhatsApp API first | Safer and more stable normal-user channel than WhatsApp Web automation. |

### Research Needed

- Best official/managed WhatsApp provider/API path for self-hosting and personal household usage.
- Current best memory benchmark harnesses and import formats: MemoryAgentBench, MemBench, LoCoMo, StructMemEval-like tasks.
- Sandboxing strategy for internet-enabled research/build containers.
- Whether WASM should become the preferred portable block runtime after process/container blocks.
- Practical adapters for Claude Code, Hermes, OpenClaw, ClickThrough, and Codex-style runtimes.

---

## Data Model

The central entity is the **Agent/Home Orchestrator**. Everything else hangs off the agent as its home server, body, memory, lab, and extension system.

### Entities

| Entity | Key Fields | Relationships |
|--------|-----------|---------------|
| Agent | id, name, identity profile, config, current state, endocrine state | Owns missions, memories, capabilities, files, policies, fractals, users. |
| User/Contact | id, name, role, channel identities, preferences, trust level | Starts missions, receives messages, has relationship memories. |
| Mission | id, source, objective, status, risk level, budget, active plan | Belongs to agent/user; has traces, artifacts, experiments, memory writes, block calls. |
| Trace Event | id, mission_id, type, timestamp, actor, payload, visibility | Append-only timeline for reasoning, tool calls, tests, errors, downloads, approvals. |
| Memory | id, type, content, source, confidence, recency, expiry, embedding, graph links | First-class memory across episodic, semantic, procedural, reflective, identity, and endocrine contexts. |
| Capability / Boring Block | id, name, version, manifest, permissions, runtime, trust level, test status | Invoked by missions; created by experiments; shareable/exportable. |
| Experiment | id, hypothesis, research notes, sandbox workspace, test results, outcome | Produces artifacts, blocks, memories, and postmortems. |
| Artifact/File | id, path, mime type, provenance, checksum, tags, retention | Generated or collected by missions, fractals, research, and blocks. |
| Fractal Session | id, target runtime, lease, mission scope, status, heartbeat, context package | Bidirectional bridge into another runtime/environment; returns traces/artifacts/results. |
| Policy/Lease | id, scope, permissions, expiry, risk class, approval mode | Controls secrets, network, filesystem, messaging, block registration, self-update. |
| Room/Lab Object | id, visual type, linked entity, position/state, mood | Visual representation of files, memories, tools, active missions, experiments. |
| Self-Update | id, patch/ref, tests, health checks, canary status, rollback snapshot | Tracks autonomous core changes and preserved failed implementations. |

### Memory Architecture

Memory is first-class and benchmark-oriented from day one. It includes:

- **Raw memory:** traces, files, messages, screenshots, logs.
- **Episodic memory:** what happened across missions and days.
- **Semantic memory:** durable facts, preferences, concepts, and user models.
- **Procedural memory:** skills, playbooks, Boring Blocks, workflows, module lessons.
- **Reflective memory:** failures, postmortems, contradictions, lessons learned.
- **Identity/persona memory:** agent self-model, user relationships, role boundaries.
- **Endocrine context:** functional behavioral state such as curiosity, caution, urgency, confidence, frustration, budget pressure, novelty pressure, and attachment to user intent.

Benchmark targets include MemoryAgentBench-style accurate retrieval, test-time learning, long-range understanding, and conflict resolution; MemBench-style factual/reflective memory and participation/observation scenarios; LoCoMo-style long-term temporal consistency; and structured memory organization.

---

## API Surface

Boring Agent uses a hybrid API architecture with an event bus at the center.

### Auth Strategy

Personal but hardened. No multi-tenancy in Phase 1.

- Strong admin auth for the dashboard.
- Separate identities for owner, normal users, channels, workers, blocks, and fractals.
- Mission-scoped leases for secrets and permissions.
- API tokens or mTLS-style credentials for internal workers/fractals where appropriate.
- Audit logs and traces are non-optional.

### Route Groups

| Group | Methods | Auth | Purpose |
|-------|---------|------|---------|
| `/api/missions` | GET, POST, PATCH | Required | Create, inspect, steer, pause, resume, and cancel missions. |
| `/api/traces` | GET | Required | Query trace events and timelines. |
| `/api/memory` | GET, POST, PATCH | Required | Browse, promote, correct, decay, link, and search first-class memories. |
| `/api/files` | GET, POST, DELETE | Required + policy | Browse and manage the agent filesystem/artifacts. |
| `/api/capabilities` | GET, POST, PATCH | Required | Register, test, enable, disable, export, and import Boring Blocks. |
| `/api/forge` | POST, GET | Required | Launch and inspect Forgery Lab experiments. |
| `/api/fractals` | POST, GET, PATCH | Required | Create, monitor, and terminate fractal sessions. |
| `/api/channels` | GET, POST, PATCH | Required | Configure WhatsApp and future communication channels. |
| `/api/policies` | GET, PATCH | Admin | Manage autonomy, approvals, secrets, block registration, self-update. |
| `/api/room` | GET, PATCH | Required | Normal View room/lab object state. |
| `/api/self-update` | POST, GET | Admin + policy | Run autonomous/staged core update attempts and rollback. |

### Internal vs External

- **Public endpoints:** ideally none except channel/webhook receivers protected by provider verification and rate limits.
- **Internal/admin endpoints:** dashboard, mission control, forge, files, memory, policies, self-update.
- **Streaming:** WebSocket/SSE/NATS-style streams for mission events, traces, block logs, fractal communication, and dashboard updates.
- **MCP integration points:** Boring Agent should both consume MCP servers and expose selected memories, files, skills, research tools, mission controls, and capability forge functions as MCP servers.

### Fractal Session Protocol

MCP is an integration edge, not the internal fractal protocol. Fractals need richer semantics:

- identity and target runtime
- mission scope and leases
- context package
- heartbeat
- bidirectional messages
- tool/context requests back to home
- trace streaming
- artifact return
- memory merge proposal
- interruption/cancel semantics
- sandbox and risk metadata

---

## Deployment Strategy

### Local Development

Development should mirror the VPS stack with Docker Compose.

| Service | Image/Runtime | Port | Purpose |
|---------|--------------|------|---------|
| core | Go | internal | Home orchestrator, event log, mission runtime, policy engine. |
| dashboard | Node/React | 3000 behind proxy | Normal View and Boring View. |
| storage | SQLite database file/volume | internal | Durable state, memory metadata, trace indexes, FTS/search, and snapshot/backup target. |
| object-store | filesystem first; MinIO later | internal / 9000 if MinIO | Artifacts, generated code, screenshots, logs. |
| worker-runner | container runtime | internal | Runs Boring Blocks and Forgery Lab jobs. |
| whatsapp-adapter | managed provider adapter | internal/webhook | Official/managed WhatsApp communication channel. |
| reverse-proxy | Caddy/Traefik/Nginx | 80/443 | TLS, routing, auth boundary. |

### Staging / Production

Phase 1 production target is a single VPS with Docker Compose:

- TLS and reverse proxy.
- Private internal network for services.
- Backups for SQLite database snapshots and artifacts.
- Health checks for core, dashboard, workers, adapters.
- Upgrade protocol with canary and rollback.
- Logs/traces visible in Boring View.

Future phases can add multi-node workers, remote fractal endpoints, dedicated vector/graph storage, and distributed execution.

---

## Security Considerations

Boring Agent can write code, run sandboxed processes, access channels, handle personal data, use secrets, and eventually update itself. Security is core product behavior, not a checklist.

- **Auth/Authz model:** single-owner household deployment with strong admin auth and scoped identities for users/channels/workers/fractals.
- **Secrets management:** secrets are never raw in prompts or traces. Blocks and fractals receive mission-scoped leases only.
- **Sandboxing:** Forgery Lab and Boring Blocks run isolated from the host, with no default host secret access or unrestricted host filesystem mutation.
- **Network posture:** Forgery Lab has internet access for research and dependency acquisition. All downloads, domains, commands, and dependency additions are traced.
- **Block registration:** configurable. Recommended default auto-registers low-risk blocks after tests/policy gates and requires approval for dangerous permissions. Raul's power-user preset allows full autonomous registration after gates pass.
- **Non-negotiable rails:** no hidden trace deletion, no silent audit disabling, no unscoped secret access, no raw host mutation outside assigned workspaces, no core replacement outside self-update protocol.
- **External messaging:** official WhatsApp channel should respect channel policy. Dangerous or identity-sensitive outbound messages can require approval depending on preset.
- **Self-update:** autonomous in power-user mode, but staged with tests, snapshot, canary, health checks, promotion, and rollback.
- **OWASP concerns:** auth/session hardening, CSRF where relevant, SSRF from research/browser tooling, prompt injection, supply-chain attacks, unsafe generated code, secrets leakage, command injection, path traversal, webhook spoofing.

---

## UI/UX Needs

Boring Agent has two primary UI modes.

### Design System

Use a polished web dashboard with a practical design system. The interface should serve both normal users and power users without making the normal experience feel like a server admin panel.

### Key Views / Pages

| View | Purpose | Complexity |
|------|---------|------------|
| Normal View / Room Lab | Cute visual room where users see the agent, active mission, memories, files, tools, experiments, and approvals as objects. | High |
| Boring View | Operator dashboard for traces, events, files, memory, blocks, sandbox runs, policies, logs, and fractals. | High |
| Mission Timeline | Inspect live and historical mission execution. | High |
| Memory Browser | Search, correct, promote, link, decay, and inspect memories. | High |
| Capability Forge | Watch research, code generation, tests, registration, and reflection for Boring Blocks. | High |
| Filesystem/Artifacts | Browse the agent's home filesystem and generated artifacts. | Medium |
| WhatsApp/Channels | Configure communication channels and inspect conversations. | Medium |
| Fractal Sessions | Monitor bridges into other runtimes and returned artifacts/traces. | Medium |
| Policies/Autonomy | Configure approval modes, risk gates, secrets, and self-update policy. | High |
| Brain Graph View | Obsidian-like graph of memories, capabilities, extremities/fractals, missions, files, and endocrine state. | Add-on |

### Real-Time Requirements

The dashboard needs real-time streams for:

- active mission traces
- worker/block logs
- sandbox test output
- fractal communication
- memory promotion proposals
- approvals
- room/lab visual state
- self-update/canary health

### Responsive Needs

Normal View should work on mobile and desktop. Boring View can be desktop-first but should degrade responsibly on tablets/mobile for monitoring.

---

## Integration Points

| Integration | Type | Purpose | Auth |
|------------|------|---------|------|
| Official/managed WhatsApp | Channel adapter | First normal-user communication surface for mom and household use. | Provider credentials/webhook verification |
| MCP servers | Tool/context protocol | Consume external tools/resources and expose Boring Agent capabilities to MCP-capable hosts. | Per-server config/tokens |
| Claude Code/Codex/Hermes/OpenClaw | Fractal/harness adapters | Attach to other runtimes as execution surfaces and return traces/artifacts/context. | Runtime-specific credentials/leases |
| ClickThrough/browser/computer-use | Fractal/body adapter | Browser/UI/computer-use extremity for future action surfaces. | Session-scoped |
| Web/docs/repos/package registries | Research-for-action | Gather best practices, working implementations, specs, dependencies. | Public or scoped tokens |
| Git/GitHub | Source/artifact integration | Store generated blocks, share packages, inspect repos, create patches. | Scoped token |

---

## Phase Breakdown

### Phase 1: Home Server, Memory, Forge, and Normal Channel

- **Build:** Go core orchestrator, Docker Compose VPS stack, SQLite-backed storage adapter schema, event/trace log, dashboard shell, Normal View + Boring View skeleton, official WhatsApp adapter, first-class memory system, Research-for-Action pipeline, Forgery Lab, Boring Block package format, process/container block runner, configurable autonomy policy, endocrine context engine, memory eval harness, and skeletal fractal protocol/bridge.
- **Testable:** create a mission from WhatsApp; trace it in Boring View; store/promote memories; research an unknown; generate/test/register a simple Boring Block; invoke it; show artifacts in filesystem; show endocrine state; run memory baseline evals; demonstrate rollback-safe self-update simulation.
- **Outcome:** Boring Agent lives on the VPS, receives a normal-user request, researches for action, forges or uses a capability, completes the workflow, and leaves an inspectable trace.

### Phase 2: Fractals and Stronger Self-Improvement

- **Build:** adapters for one or more hijackable runtimes such as Claude Code, ClickThrough, Hermes, OpenClaw, or Codex-style agents; richer Fractal Session Protocol; artifact/memory merge workflows; stronger sandbox policies; block import/export; registry/signature model; expanded Research-for-Action.
- **Testable:** home server opens a bidirectional bridge to an external runtime, delegates a scoped task, receives traces/files/results, and merges useful knowledge into memory/procedural memory.
- **Outcome:** Boring Agent gains remote/extensible "extremities" while the VPS remains the source of truth.

### Phase 3: Benchmark-Oriented Memory and Autonomy

- **Build:** deeper benchmark runners, structured memory graph, conflict resolution, temporal reasoning, observation mode, reflective memory scoring, endocrine ablation tests, autonomous self-update with canary promotion, block marketplace/export UX.
- **Testable:** memory eval scores improve over baseline RAG; repeated tasks produce better capabilities; autonomous updates can roll back safely; blocks are shareable between agents.
- **Outcome:** Boring Agent begins competing with or exceeding existing memory systems and becomes meaningfully self-improving.

### Phase 4: Computer-Use and Broader Body

- **Build:** ClickThrough/browser/computer-use extensions, visual perception/action loops, desktop/browser adapters, stronger policy controls for external actions.
- **Testable:** agent completes a UI/browser task through a fractal/body adapter and returns traces/artifacts/memory.
- **Outcome:** Boring Agent can act beyond APIs and chat channels while preserving traceability and user trust.

---

## Skill Loadout & Quality Gates

### Skills Used During Build

| Skill | When It Fires | Purpose |
|-------|--------------|---------|
| PAUL | Build planning and milestone execution | Keep the application from becoming ad-hoc and unbounded. |
| AEGIS/security-auditor | End of each phase and before risky capability execution | Catch auth, sandbox, secrets, OWASP, and supply-chain issues. |
| ui-ux-pro-max/frontend-design | Dashboard, Normal View, Boring View, Brain Graph View | Build a usable and distinctive interface for normal and power users. |
| openai-docs | Model/API/tooling decisions | Use current official OpenAI guidance when integrating OpenAI products. |
| Superpowers planning/TDD/verification | Feature implementation | Keep changes designed, tested, and verified before completion. |

### Quality Gates

| Gate | Threshold | When |
|------|-----------|------|
| Core tests | Passing | Every core change |
| Block tests | Passing before registration | Every generated/imported block |
| Sandbox policy check | Passing | Every Forgery Lab/block run |
| Trace completeness | Mission has inspectable events/artifacts | Every mission |
| Security scan | No critical unresolved issues | Each phase |
| Memory eval harness | Runs with baseline scores | Phase 1 soft gate |
| Memory benchmark improvement | Improves over baseline RAG | Phase 2/3 target |
| Self-update rollback | Demonstrated rollback path | Before autonomous self-update |
| Accessibility | WCAG AA target | UI milestones |
| Performance | Dashboard responsive; core stable under long-running missions | Each phase |

---

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
10. **UI has Normal View and Boring View:** the same system must be approachable for mom and deeply inspectable for Raul.
11. **WhatsApp is the first normal-user channel:** use official/managed integration rather than brittle WhatsApp Web automation.
12. **Fractals are skeletal in Phase 1:** the protocol and minimal bridge are important early, with full runtime hijacking in Phase 2.
13. **Self-update can be autonomous in power-user mode:** but updates must be staged, tested, snapshotted, canaried, health-checked, and rollbackable.

---

## Open Questions

1. Which official/managed WhatsApp provider/API path is best for a self-hosted personal assistant?
2. Should the dashboard frontend be React/Next.js or a simpler Vite/React app?
3. What is the first concrete normal-user workflow for mom?
4. What is the first concrete power-user capability the agent should forge?
5. Which external runtime should be the first real fractal target: Claude Code, ClickThrough, Hermes, OpenClaw, or Codex-style runtime?
6. How should block signing, provenance, and sharing work in the first public package format?
7. Which memory benchmarks can be imported directly versus approximated with internal evals?
8. What baseline model/provider mix should Phase 1 assume?

---

## Next Actions

- [ ] Run `/seed graduate boring-agent` or `/seed launch boring-agent` when ready to turn this plan into a buildable app.
- [ ] Research official WhatsApp integration options.
- [ ] Specify the Boring Block manifest and process protocol.
- [ ] Specify the Forgery Lab sandbox threat model.
- [ ] Define the Phase 1 memory schema and eval harness.
- [ ] Define the Fractal Session Protocol v0.
- [ ] Choose the first mom workflow and first power-user forge demo.

---

## References

- Hermes Agent: https://nousresearch.com/hermes-agent/
- OpenClaw overview: https://openclawdoc.com/docs/getting-started/what-is-openclaw/
- OpenClaw runtime docs: https://docs.openclaw.ai/concepts/agent
- Model Context Protocol: https://modelcontextprotocol.io/docs/protocol
- MemoryAgentBench: https://huggingface.co/papers/2507.05257
- MemBench: https://huggingface.co/papers/2506.21605
- LoCoMo: https://huggingface.co/papers/2402.17753
- Emotion concepts / functional emotions: https://papers.cool/arxiv/2604.07729
- EmotionPrompt: https://huggingface.co/papers/2307.11760
- Emotional framing study: https://papers.cool/arxiv/2604.02236
- ClickThrough inspiration: https://github.com/raulgooo/clickthrough

---

*Last updated: 2026-05-10*

---

**Graduated:** 2026-05-11  
**Location:** `apps/boring-agent/`  
**README:** `apps/boring-agent/README.md`
