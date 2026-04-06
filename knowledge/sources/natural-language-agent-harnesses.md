# Natural-Language Agent Harnesses

- **URL**: https://arxiv.org/html/2603.25723v1
- **Added**: 2026-04-05
- **Category**: methodology

## Summary

This paper argues that agent performance increasingly depends on harness engineering, yet harness design is typically buried in controller code and runtime-specific conventions, making it hard to transfer, compare, or study. The authors introduce Natural-Language Agent Harnesses (NLAHs) — structured natural-language files that express harness behavior via explicit contracts, roles, adapters/scripts, state semantics, and a failure taxonomy — and the Intelligent Harness Runtime (IHR), a shared runtime with an in-loop LLM that executes NLAHs while isolating shared runtime policy (the runtime charter) from task-family harness logic. Empirical evaluations across coding and computer-use benchmarks demonstrate operational viability, interpretable module-level effects, and improved reliability through file-backed durable state.

## Key Insights

- Harness logic embedded only in code or prompts is non-transferable and non-comparable — externalize it as a portable, versioned artifact (document or skill file)
- A complete harness specification includes: contracts (inputs, outputs, format constraints, validation gates, permission boundaries, retry/stop rules), roles (non-overlapping responsibilities), named adapters/scripts (hooks for deterministic actions), state semantics (what persists and how it is reopened), and a failure taxonomy (named failure modes driving recovery)
- Separate the **runtime charter** (global policy, coordination semantics, budget constraints — akin to AGENTS.md default rules) from **task-family harness logic** (stage ordering, artifact contracts, verifiers — akin to skill files); this separation enables ablation, portability, and independent improvement
- Durable, file-backed state artifacts improve agent reliability under context truncation; in-memory or GUI-dependent state leads to brittle repair loops
- Naming failure modes explicitly (missing artifact, verifier failure, tool error, timeout) enables structured recovery instead of blind retry
- 90% of tokens/calls in full IHR execution occurred in delegated child agents — behavioral delegation to sub-agents is the dominant execution pattern, not monolithic single-agent execution
- Explicit artifact contracts at skill/step boundaries enable verifier-backed checking and structured error recovery without reading implementation internals

## Template Documents Updated

- `template/docs/context/GLOSSARY.md` — added "Harness Engineering Terms" section with definitions for Harness, Runtime Charter, Artifact Contract, Failure Taxonomy, and State Semantics
- `template/docs/design/PATTERNS.md` — added preferred pattern "Externalized Harness Contracts" and anti-pattern "Harness Logic Buried in Code"
- `template/docs/workflows/TASK-LIFECYCLE.md` — added two bullets to "When Things Go Wrong" on naming failure modes and preferring durable file-backed artifacts
