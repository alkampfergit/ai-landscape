# Architecture Decision Records (ADRs)

> Every significant architectural decision is recorded here.
> An ADR captures WHAT was decided, WHY it was decided, and WHAT alternatives were rejected.
> This prevents future agents (and humans) from re-litigating settled decisions.

## When to Write an ADR

Write an ADR when:
- A new technology, library, or framework is adopted.
- A structural pattern is chosen over alternatives.
- A domain boundary is created, split, or merged.
- A dependency rule exception is granted.
- A convention is established that future code must follow.

Do NOT write an ADR for:
- Routine implementation choices within established patterns.
- Bug fixes that don't change architecture.
- Minor refactors that don't alter boundaries or dependencies.

## ADR Template

```markdown
## ADR-[NNN]: [Title]

**Date**: [YYYY-MM-DD]
**Status**: [Proposed | Accepted | Deprecated | Superseded by ADR-XXX]
**Deciders**: [who made this decision]

### Context

[What situation prompted this decision? What problem needed solving?]

### Decision

[What was decided? Be specific.]

### Consequences

[What are the positive and negative consequences?]
[What trade-offs were accepted?]

### Alternatives Considered

[What other options were evaluated and why were they rejected?]
```

## Active ADRs

*(Add new ADRs below, in chronological order)*

---

## ADR-001: Repository-as-Source-of-Truth Policy

**Date**: 2026-03-12
**Status**: Accepted
**Deciders**: [founding team]

### Context

AI agents can only reason about information available in their context.
Knowledge stored in external tools (Slack, Google Docs, email) is invisible
to the agent and leads to misaligned output.

### Decision

All project knowledge — design decisions, domain terminology, architectural
constraints, team conventions — must live in the repository as versioned
markdown files or code artifacts.

### Consequences

- (+) Agents always have access to the full knowledge base.
- (+) Knowledge is versioned and reviewable.
- (-) Requires discipline to capture decisions immediately.
- (-) Some knowledge (e.g., business context) is harder to formalize.

### Alternatives Considered

- **Wiki or external docs**: Rejected because agents cannot access them in-context.
- **Large AGENTS.md file**: Rejected because it overwhelms the context window.

---

## ADR-002: Progressive Disclosure via Structured docs/ Directory

**Date**: 2026-03-12
**Status**: Accepted
**Deciders**: [founding team]

### Context

A monolithic instruction file degrades agent performance: it crowds out task
context, causes pattern-matching instead of intentional navigation, and rots
quickly as rules go stale.

### Decision

Use a short AGENTS.md (~100 lines) as a table of contents, with detailed
knowledge organized in `docs/` subdirectories. Agents load deeper context
on demand based on the task at hand.

### Consequences

- (+) Agents start with focused context and expand as needed.
- (+) Each document can be independently verified and maintained.
- (-) Agents must know how to navigate the documentation structure.
- (-) Cross-references between docs must be kept up to date.

### Alternatives Considered

- **Single large file**: Tested and abandoned — agents missed key constraints.
- **No structured docs**: Too much implicit knowledge, high error rate.
