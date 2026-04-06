# Domain Glossary

> This glossary defines terms used across the codebase.
> When you encounter an unfamiliar term in a task description or code,
> check here first. If the term is not listed, ask for clarification
> and then add it.

## How to Use This Glossary

- Terms are grouped by domain.
- Each term has a **definition** and an **in-code representation** (where applicable).
- When naming variables, classes, or functions, use the glossary term exactly.
  Consistent naming prevents confusion across domains.

## Universal Terms

| Term                | Definition                                                        | In Code           |
|---------------------|-------------------------------------------------------------------|--------------------|
| Domain              | A bounded context with clear ownership of specific business rules | Folder in `src/domains/` |
| Aggregate           | A cluster of domain objects treated as a single unit for changes  | Class with ID      |
| Domain Event        | A record that something meaningful happened in a domain           | Event class/type   |
| Value Object        | An immutable object defined by its attributes, not identity       | Immutable type     |
| Boundary            | The interface between two domains or between a domain and external world | API handler, event consumer |

## Documentation & Decision Terms

| Term                          | Definition                                                                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------------|
| ADR (Architecture Decision Record) | A document capturing a significant architectural decision: its context, the options considered, the decision made, the rationale, and the consequences. One decision per record. |
| ADL (Architecture Decision Log)    | The ordered collection of all ADRs for a project. In this template, the ADL lives in `docs/context/DECISIONS.md`. |
| ASR (Architecturally-Significant Requirement) | A requirement that has a measurable impact on the system's architecture — performance, security, scalability, or structural constraints that narrow architectural choices. |
| Fitness Function               | An objective, automated check that verifies the codebase continues to comply with a recorded architectural decision. Implemented as a structural test, linter rule, or CI gate. |
| Superseded ADR                 | An ADR whose decision has been replaced by a newer ADR. The original is kept immutable; its status is updated to "Superseded by ADR-NNN". |

## Harness Engineering Terms

| Term                | Definition                                                                                          |
|---------------------|-----------------------------------------------------------------------------------------------------|
| Harness             | The orchestration layer surrounding an AI agent — including its skill files, prompts, contracts, adapters, and runtime policy — that shapes how the agent behaves and what it is allowed to do. |
| Runtime Charter     | The global policy document (or section of AGENTS.md) that specifies coordination semantics, budget constraints, and contract-fulfillment rules shared across all agent tasks, distinct from task-family-specific skill logic. |
| Artifact Contract   | An explicit specification on a skill or agent step defining required inputs, expected outputs, format constraints, and validation gates. Enables verifier-backed checking at each stage of a workflow. |
| Failure Taxonomy    | A named catalog of failure modes that can occur in an agent workflow (e.g., missing artifact, verifier failure, tool error, timeout). Naming failures explicitly drives structured recovery rather than blind retry. |
| State Semantics     | The specification of what state persists across agent steps, how it is stored (e.g., file-backed artifacts vs. in-memory), and how it is reopened after interruption or context truncation. |
| Reasoning Sandwich  | A reasoning-budget pattern in which extended or high-effort reasoning is applied at planning and verification phases, while standard reasoning is used for execution steps. Concentrates thinking compute where mistakes are most costly. |
| Self-Verification Loop | An explicit harness instruction directing an agent to run tests, check outputs, and re-examine its solution against the original task specification before declaring the task complete. Counteracts model bias toward the first plausible answer. |
| Context Middleware  | A hook that runs at agent start to discover and inject environmental context (working directory layout, available tools, language runtimes) into the agent's prompt, reducing failed tool-discovery attempts during task execution. |
| Harness Improvement Flywheel | The compounding process by which each failure-driven correction is encoded permanently as a lint rule, CI gate, structural test, or skill update, raising the floor for all future agent runs. A mature harness enables more complex delegation, which surfaces the next gap, which gets encoded in turn. |
| Depth-First Decomposition | A task-planning strategy in which a large goal is broken into the smallest building block that, once completed, unlocks the next step. Building blocks are delivered in order rather than attempting the full goal at once, keeping each unit verifiable and context-bounded. |
| Instruction Budget | The finite number of distinct instructions a frontier LLM can follow with reasonable consistency in a single context window — approximately 150–200 for current models. The agent's own system prompt consumes a significant share (~50 instructions for Claude Code), leaving limited headroom for project rules, skills, and task-specific guidance. Exceeding the budget causes instructions to be silently ignored. |
| Back-Pressure | Automated feedback from deterministic tools — type checkers, linters, test runners, build systems — that signals errors to the agent immediately after an action, enabling self-correction without human intervention. Back-pressure is the primary mechanism that allows agents to work on longer-horizon tasks autonomously; without it, errors compound silently. |

## [Domain 1] Terms

| Term                | Definition                                                        | In Code           |
|---------------------|-------------------------------------------------------------------|--------------------|
| [Term]              | [Definition]                                                      | [Class/type name]  |

## [Domain 2] Terms

| Term                | Definition                                                        | In Code           |
|---------------------|-------------------------------------------------------------------|--------------------|
| [Term]              | [Definition]                                                      | [Class/type name]  |

## Adding New Terms

When a new concept is introduced:
1. Add it to the appropriate domain section.
2. Use the exact term in code (class names, variable names).
3. If the term is used across domains, add it to "Universal Terms."
4. Commit with message `docs: add glossary term [term]`.
