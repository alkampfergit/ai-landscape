# Skill Issue: Harness Engineering for Coding Agents

- **URL**: https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents
- **Added**: 2026-04-05
- **Updated**: 2026-04-06
- **Category**: methodology

## Summary

A practical HumanLayer case study on harness engineering for coding agents. The central thesis is that poor agent performance is almost always a harness problem, not a model problem — "it's just a skill issue." The article documents what failed and what worked in real-world usage: over-engineering the harness upfront (installing MCP servers and skills "just in case," running full test suites after every session, micro-optimizing sub-agent tool access) consistently backfires and produces tool thrash. What actually works is starting simple, adding harness configuration only after real failures, distributing battle-tested configs at the repository level, and using sub-agents as context firewalls to isolate execution context and preserve coherence over long sessions. The harness levers that provide most leverage are: Agentfiles, MCP servers, skills, sub-agents, hooks, and back-pressure.

## Key Insights

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

## Template Documents Updated

- `template/docs/design/PATTERNS.md` — added sub-agents-as-context-firewalls pattern and capability-bloat / sub-agent-tool-micro-optimization anti-patterns
- `template/docs/design/DESIGN-PRINCIPLES.md` — added P9 (iterate the harness, don't pre-optimize it) and instruction-budget guidance to P5
- `template/docs/context/GLOSSARY.md` — added Instruction Budget and Back-Pressure definitions
- `template/docs/workflows/TASK-LIFECYCLE.md` — added high-leverage review guidance (review plans before code)
