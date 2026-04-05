# Architecture Decision Records (ADRs)

> Every significant architectural decision is recorded here.
> An ADR captures WHAT was decided, WHY it was decided, and WHAT alternatives were rejected.
> This prevents future agents (and humans) from re-litigating settled decisions.
>
> The collection of all ADRs for a project is the **Architecture Decision Log (ADL)**.

## When to Write an ADR

Write an ADR when:
- A new technology, library, or framework is adopted.
- A structural pattern is chosen over alternatives.
- A domain boundary is created, split, or merged.
- A dependency rule exception is granted.
- A convention is established that future code must follow.

Skip an ADR when:
- The decision is minimal-scope, low-risk, or already covered by an existing ADR.
- The change is temporary or time-bound with no lasting architectural effect.
- Bug fixes that don't change architecture.
- Minor refactors that don't alter boundaries or dependencies.

## Governance

- **Who can propose**: Any team member or agent working on the codebase.
- **Who accepts**: The team (or a designated architecture forum). Consensus beats unilateral decisions.
- **Immutability**: Do not alter an accepted ADR. Amend with a new, dated ADR that supersedes the old one; update the old ADR's status to `Superseded by ADR-NNN`.
- **After-action review**: Schedule a review ~1 month after acceptance to capture real consequences and update the record with what actually happened.

## Status Lifecycle

```
Proposed → Accepted → (Deprecated | Superseded by ADR-NNN)
```

## ADR Template

See [`templates/adr.template.md`](../../templates/adr.template.md) for the full template with guidance notes.

Quick reference:

```markdown
## ADR-[NNN]: [Title]

**Date**: [YYYY-MM-DD]
**Status**: [Proposed | Accepted | Deprecated | Superseded by ADR-XXX]
**Deciders**: [who made this decision]

### Context
[Forces and constraints that prompted this decision]

### Options Considered
- **Option A**: [trade-offs]
- **Option B**: [trade-offs]

### Decision
[What was decided — one decision per record]

### Rationale
[Why this option — the decisive pros/cons and reasoning]

### Consequences
[Follow-on decisions, constraints imposed, after-action review date]
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
