# Improving Deep Agents with Harness Engineering

- **URL**: https://blog.langchain.com/improving-deep-agents-with-harness-engineering/
- **Added**: 2026-04-05
- **Category**: methodology

## Summary

LangChain details how harness engineering — optimizing the system prompt, tools, and middleware around a fixed model — improved their deepagents-cli coding agent from 52.8% to 66.5% on Terminal Bench 2.0, moving it from Top 30 to Top 5 with no model change. The core thesis is that a harness "molds the inherently spiky intelligence of a model for tasks you care about," and the three levers are: an enriched system prompt with explicit problem-solving strategy, a LocalContextMiddleware that injects environment-discovery context at agent start, and self-verification loops that counteract model bias toward the first plausible answer. A "Reasoning Sandwich" pattern — extended reasoning at plan and verify, standard reasoning at execution — balances quality against timeout risk.

## Key Insights

- Harness-only changes (no model change) can yield large benchmark improvements; 13.7-point gain on Terminal Bench 2.0 demonstrates this.
- Three harness optimization levers: System Prompt, Tools, and Middleware (hooks around model and tool calls).
- **Reasoning Sandwich**: use extended/high-effort reasoning for planning and verification phases, standard reasoning for execution — concentrates compute where mistakes are most costly and avoids timeouts.
- **Context Middleware / environment onboarding**: inject working directory layout and tool discovery at agent start; eliminates failed tool-discovery attempts during task execution.
- **Self-verification loops**: explicitly prompt agents to run tests and verify against the task spec; models are biased toward their first plausible answer and will not self-correct without explicit instruction.
- **Loop detection middleware**: detect and break repetitive agent cycles before they exhaust budget.
- **Trace-based improvement loop**: capture all agent actions (inputs, outputs, latency, token counts) in observability tooling; analyze failure modes from traces to drive harness improvements.
- System prompt should embed an explicit problem-solving scaffold: read task → scan/discover environment → build plan → implement → verify.

## Template Documents Updated

- `template/docs/design/PATTERNS.md` — added Reasoning Sandwich and Self-Verification Loop patterns
- `template/docs/workflows/TASK-LIFECYCLE.md` — added environment survey step and explicit self-verification in validate phase
- `template/docs/context/GLOSSARY.md` — added Reasoning Sandwich, Self-Verification Loop, Context Middleware, and Harness Improvement Flywheel definitions
