# Knowledge Sources

This file is the **master index of external sources** that inform the harness engineering template.

Each entry records the source URL, a summary of key insights extracted from it, and which template
documents were updated as a result of ingesting it.

> **Do not edit manually.**
> Use the `ingest-link` skill to add or update entries.
> Run `ingest-link <url>` to analyse a new source and propagate its insights into the template.

---

## Format Reference

Each entry follows this structure:

```
### [Title](URL)
- **Added**: YYYY-MM-DD
- **Category**: one of: methodology | pattern | tool | case-study | reference
- **Summary**: One-paragraph synthesis of the source's key argument or content.
- **Key insights**: Bullet list of actionable points relevant to harness engineering.
- **Template documents updated**: Which files under template/ were touched and why.
```

---

## Entries

<!-- ingest-link appends entries below this line -->

### [User Journeys vs. User Flows](https://www.nngroup.com/articles/user-journeys-vs-user-flows/)
- **Added**: 2026-04-12
- **Category**: methodology
- **Summary**: Nielsen Norman Group defines user journeys as scenario-based sequences of steps a user takes to accomplish a high-level goal, usually across channels and over time, contextualized with thoughts, emotions, and pain points. User flows, by contrast, describe the specific discrete interactions within a product for a common task. Journeys are the holistic macro view (why and what the user experiences); flows are the micro view (how they interact step by step). Both are useful and complementary — journeys identify which documentation needs to exist and why, flows describe how documents link to each other.
- **Key insights**:
  - A user journey is the end-to-end experience over time — not just the steps taken, but the thoughts, emotions, pain points, and confidence shifts at each phase
  - User flows are the specific discrete interactions within a single phase — the micro-level complement to the macro-level journey
  - Journey mapping starts with a timeline of user goals and actions, then adds thoughts and emotions to create a narrative
  - Journeys answer "what is this person trying to accomplish and what do they experience?"; flows answer "what specific steps do they take?"
  - Both views should be captured: journeys tell you which documents need to exist; flows tell you how they link together
  - Journey phases can be made concrete and testable through user stories ("As a [role], I want [goal], so that [benefit]")
  - Documentation earns its place by serving identified journey phases — docs that no journey phase reaches for are candidates for pruning
  - Progressive disclosure is the mechanism that maps to journey phases: the right information surfaces at the right moment in the experience
- **Template documents updated**: `template/docs/design/DESIGN-PRINCIPLES.md`, `template/meta/hareness-foundation.md`, `template/docs/context/GLOSSARY.md`, `template/AGENTS.md`, `template/.claude/skills/doc-gardening/SKILL.md`, `template/templates/user-story.template.md` (new)

### [Skill Issue: Harness Engineering for Coding Agents](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents)
- **Added**: 2026-04-05
- **Updated**: 2026-04-06
- **Category**: methodology
- **Summary**: A practical HumanLayer case study on harness engineering for coding agents. The central thesis is that poor agent performance is almost always a harness problem, not a model problem — "it's just a skill issue." The article documents what failed and what worked in real-world usage: over-engineering the harness upfront (installing MCP servers and skills "just in case," running full test suites after every session, micro-optimizing sub-agent tool access) consistently backfires and produces tool thrash. What actually works is starting simple, adding harness configuration only after real failures, distributing battle-tested configs at the repository level, and using sub-agents as context firewalls to isolate execution context and preserve coherence over long sessions. The harness levers that provide most leverage are: Agentfiles, MCP servers, skills, sub-agents, hooks, and back-pressure.
- **Key insights**:
  - Sub-agents function as "context firewalls" — isolating execution context so that long sessions remain coherent by preventing context pollution between unrelated tasks
  - Over-engineering the harness upfront (before hitting real failures) is a primary anti-pattern — resist adding capability until the agent demonstrably needs it
  - Installing MCP servers and skills "just in case" adds noise to the model's available-tool context and degrades performance
  - Running the full test suite after every agent session is wasteful; run targeted subsets instead
  - Micro-optimizing sub-agent tool access causes tool thrash and worse outcomes — most coding agents lack a robust configuration surface for it
  - Start simple: add harness configuration only when the agent actually fails at something specific
  - Distribute battle-tested configurations at the repository level so the whole team benefits automatically
  - Optimize for iteration speed (design → test → iterate), not for the probability of one-shotting on the first attempt
  - Give the agent a broad capability set first, then pare down what is exposed based on observed need
  - Harness levers: Agentfiles, MCP servers, skills, sub-agents, hooks, back-pressure
  - Frontier LLMs can follow ~150–200 instructions with reasonable consistency; the agent's system prompt already consumes ~50 of that budget — keep top-level instruction files minimal
  - Back-pressure from automated feedback loops (type systems, linters, build tools) enables agents to work on longer-horizon tasks without constant human intervention
  - Progressive disclosure applies to skills specifically: bundle context-specific instructions inside skill files rather than in the top-level agent file, so they load only when relevant
  - Focus human review on research and planning artifacts, not just code — a flawed plan cascades into many bad lines of code
- **Template documents updated**: `template/docs/design/PATTERNS.md`, `template/docs/design/DESIGN-PRINCIPLES.md`, `template/docs/context/GLOSSARY.md`, `template/docs/workflows/TASK-LIFECYCLE.md`

### [Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)
- **Added**: 2026-04-05
- **Category**: methodology
- **Summary**: LangChain details how harness engineering — optimizing the system prompt, tools, and middleware around a fixed model — improved their deepagents-cli coding agent from 52.8% to 66.5% on Terminal Bench 2.0, moving it from Top 30 to Top 5 with no model change. The core thesis is that a harness "molds the inherently spiky intelligence of a model for tasks you care about," and the three levers are: an enriched system prompt with explicit problem-solving strategy, a LocalContextMiddleware that injects environment-discovery context at agent start, and self-verification loops that counteract model bias toward the first plausible answer. A "Reasoning Sandwich" pattern — extended reasoning at plan and verify, standard reasoning at execution — balances quality against timeout risk.
- **Key insights**:
  - Harness-only changes (no model change) can yield large benchmark improvements; 13.7-point gain on Terminal Bench 2.0 demonstrates this.
  - Three harness optimization levers: System Prompt, Tools, and Middleware (hooks around model and tool calls).
  - **Reasoning Sandwich**: use extended/high-effort reasoning for planning and verification phases, standard reasoning for execution — concentrates compute where mistakes are most costly and avoids timeouts.
  - **Context Middleware / environment onboarding**: inject working directory layout and tool discovery at agent start; eliminates failed tool-discovery attempts during task execution.
  - **Self-verification loops**: explicitly prompt agents to run tests and verify against the task spec; models are biased toward their first plausible answer and will not self-correct without explicit instruction.
  - **Loop detection middleware**: detect and break repetitive agent cycles before they exhaust budget.
  - **Trace-based improvement loop**: capture all agent actions (inputs, outputs, latency, token counts) in observability tooling; analyze failure modes from traces to drive harness improvements.
  - System prompt should embed an explicit problem-solving scaffold: read task → scan/discover environment → build plan → implement → verify.
- **Template documents updated**: `template/docs/design/PATTERNS.md`, `template/docs/workflows/TASK-LIFECYCLE.md`, `template/docs/context/GLOSSARY.md`

### [Natural-Language Agent Harnesses](https://arxiv.org/html/2603.25723v1)
- **Added**: 2026-04-05
- **Category**: methodology
- **Summary**: This paper argues that agent performance increasingly depends on harness engineering, yet harness design is typically buried in controller code and runtime-specific conventions, making it hard to transfer, compare, or study. The authors introduce Natural-Language Agent Harnesses (NLAHs) — structured natural-language files that express harness behavior via explicit contracts, roles, adapters/scripts, state semantics, and a failure taxonomy — and the Intelligent Harness Runtime (IHR), a shared runtime with an in-loop LLM that executes NLAHs while isolating shared runtime policy (the runtime charter) from task-family harness logic. Empirical evaluations across coding and computer-use benchmarks demonstrate operational viability, interpretable module-level effects, and improved reliability through file-backed durable state.
- **Key insights**:
  - Harness logic embedded only in code or prompts is non-transferable and non-comparable — externalize it as a portable, versioned artifact (document or skill file)
  - A complete harness specification includes: contracts (inputs, outputs, format constraints, validation gates, permission boundaries, retry/stop rules), roles (non-overlapping responsibilities), named adapters/scripts (hooks for deterministic actions), state semantics (what persists and how it is reopened), and a failure taxonomy (named failure modes driving recovery)
  - Separate the **runtime charter** (global policy, coordination semantics, budget constraints — akin to AGENTS.md default rules) from **task-family harness logic** (stage ordering, artifact contracts, verifiers — akin to skill files); this separation enables ablation, portability, and independent improvement
  - Durable, file-backed state artifacts improve agent reliability under context truncation; in-memory or GUI-dependent state leads to brittle repair loops
  - Naming failure modes explicitly (missing artifact, verifier failure, tool error, timeout) enables structured recovery instead of blind retry
  - 90% of tokens/calls in full IHR execution occurred in delegated child agents — behavioral delegation to sub-agents is the dominant execution pattern, not monolithic single-agent execution
  - Explicit artifact contracts at skill/step boundaries enable verifier-backed checking and structured error recovery without reading implementation internals
- **Template documents updated**:
  - `template/docs/context/GLOSSARY.md` — added "Harness Engineering Terms" section with definitions for Harness, Runtime Charter, Artifact Contract, Failure Taxonomy, and State Semantics
  - `template/docs/design/PATTERNS.md` — added preferred pattern "Externalized Harness Contracts" and anti-pattern "Harness Logic Buried in Code"
  - `template/docs/workflows/TASK-LIFECYCLE.md` — added two bullets to "When Things Go Wrong" on naming failure modes and preferring durable file-backed artifacts

### [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- **Added**: 2026-04-05
- **Category**: case-study
- **Summary**: OpenAI's Codex team built and shipped a production beta product containing roughly one million lines of code in five months, with zero lines written by human hands, driven by three engineers using Codex agents across ~1,500 merged pull requests. The article defines "harness engineering" as the discipline of designing the environment — scaffolding, tooling, abstractions, and feedback loops — that makes reliable agent-generated code possible. Key practices include a short AGENTS.md (~100 lines) as a navigation map (not a comprehensive manual), a structured `docs/` directory as machine-readable ground truth, mechanically enforced architectural invariants via custom linters in CI, a recurring doc-gardening agent for keeping documentation current, and the principle that each failure-driven correction should be encoded permanently rather than handled once.
- **Key insights**:
  - Context is a scarce resource: give agents a map (~100-line AGENTS.md), not a 1000-page manual — a giant instruction file crowds out the task, the code, and the relevant docs.
  - Enforce architectural invariants mechanically via custom linters and CI gates rather than micromanaging individual implementations; linters themselves can be generated by the agent.
  - Work depth-first: break large goals into the smallest building block that, once delivered, unlocks the next step — avoids context overload and keeps each unit verifiable.
  - Encode corrections permanently: when a failure reveals a missing constraint, turn the fix into a lint rule, CI check, or skill update so the same class of mistake cannot recur — this is the harness improvement flywheel.
  - Machine-readable documentation is the agent's ground truth: vague AGENTS.md → vague output; precise, well-structured docs directly determine agent output quality.
  - A recurring doc-gardening agent (scans for stale/obsolete docs, opens fix-up PRs) is essential at scale to keep documentation in sync with the actual codebase.
  - Quality grading per domain and architectural layer (tracked over time) provides an objective signal of where agent work is safe and where extra caution is needed.
  - The epistemic shift: the engineer's primary job becomes designing the environment in which an agent reliably produces correct code, not producing correct code directly.
  - A mature harness enables full end-to-end agent autonomy: spec → code → tests → review response → merge, with no human in the loop.
- **Template documents updated**: `template/docs/design/PATTERNS.md`, `template/docs/design/DESIGN-PRINCIPLES.md`, `template/docs/workflows/TASK-LIFECYCLE.md`, `template/docs/context/GLOSSARY.md`

### [Architecture Decision Records — Comprehensive Guide](https://www.perplexity.ai/search/search-info-about-adr-document-fCZ3UaxrS3qoEPY_gzh4_w)
- **Added**: 2026-04-05
- **Canonical sources fetched**: https://github.com/joelparkerhenderson/architecture-decision-record (README)
- **Category**: methodology
- **Summary**: Architecture Decision Records (ADRs) are documents that capture significant architectural decisions along with their context, the options considered, the rationale for the choice made, and the resulting consequences. Collected together they form an Architecture Decision Log (ADL). Multiple formats exist — Nygard (simple), MADR (options-and-trade-offs focused), Tyree-Akerman (sophisticated), and others — each suited to different team contexts. The core discipline is preserving the "why" behind decisions so future developers and agents can understand constraints rather than re-litigate them. Records should be immutable once accepted; changes are captured as new superseding ADRs.
- **Key insights**:
  - ADRs must capture **rationale** (the decisive pros/cons and reasoning), not just the decision — the "why" is more valuable than the "what"
  - One decision per record; keep them specific and focused
  - Records are **immutable** once accepted — amend by creating a new ADR that supersedes the old one; never rewrite history
  - MADR format explicitly surfaces **options considered and trade-offs**, which is more useful for agents than a flat decision statement
  - File naming convention: lowercase, dash-separated, present-tense imperative verb (e.g. `choose-database.md`)
  - Status lifecycle: `Proposed → Accepted → Deprecated | Superseded by ADR-NNN`
  - Schedule an **after-action review** ~1 month after acceptance to capture real consequences
  - Governance: define who can propose, who accepts, when to skip — prevents ADR fatigue
  - **Fitness functions** (ArchUnit, structural tests, linter rules) can mechanically enforce ADR decisions in code
  - **Decision Guardian** tooling can surface relevant ADRs during PR review, embedding decision context in the workflow
  - AI/LLM fitness functions can evaluate compliance with ADRs using extended-context reasoning
- **Template documents updated**:
  - `template/templates/adr.template.md` — added `Options Considered` section (MADR-style), renamed `Alternatives Considered` → `Options Considered`, added `Rationale` section separate from `Decision`, added immutability note, expanded `Context` guidance, added file naming convention
  - `template/docs/context/DECISIONS.md` — added ADL concept, immutability rule, after-action review, governance section, skip criteria, status lifecycle diagram, updated inline template to reference standalone template
  - `template/docs/context/GLOSSARY.md` — added Documentation & Decision Terms section with ADR, ADL, ASR, Fitness Function, Superseded ADR definitions
  - `template/docs/workflows/TASK-LIFECYCLE.md` — expanded ADR trigger criteria in Phase 2 Plan with explicit write/skip conditions
