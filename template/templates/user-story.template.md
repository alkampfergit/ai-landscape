# User Story Template

> **For agents and humans:** Use this template to capture a documentation need
> tied to a specific user journey phase. Each story makes one journey phase
> concrete and testable — it is satisfied when the documentation it describes
> is reachable at the right moment in the journey.
>
> Stories are typically authored during journey mapping sessions or when a
> journey gap is discovered (e.g., an agent failed because context was missing
> at a specific phase).

## Story Format

```
As a [role],
I want [goal],
so that [benefit].
```

### Fields

| Field   | Description |
|---------|-------------|
| **Role**    | Who needs this — e.g., "new contributor," "agent executing a bug fix," "engineer reviewing a PR," "agent onboarding to the codebase." Use concrete roles, not generic "user." |
| **Goal**    | What they need at this journey phase — e.g., "find the dependency rules for domain X," "see the quality grade before modifying area Y," "understand why pattern Z was chosen." |
| **Benefit** | Why it matters — the pain point avoided or confidence gained. Ties the story to the journey experience: what goes wrong without this? |

## Mapping to Journeys

Each story belongs to a journey and phase. Record both:

| Journey               | Phase       | Story |
|-----------------------|-------------|-------|
| [Journey name]        | [Phase]     | As a [role], I want [goal], so that [benefit]. |

## Acceptance Criteria

A story is satisfied when:

1. **Reachable** — The document or skill that answers the story is discoverable
   at the right journey phase (linked from the right place, not buried).
2. **Sufficient** — The content actually answers the user's goal. Existing but
   incomplete docs do not satisfy the story.
3. **Timely** — The information appears at the phase where it is needed, not
   earlier (noise) or later (too late).

## Example Stories

### Journey: Onboard to the codebase

| Phase                    | Story |
|--------------------------|-------|
| Orient                   | As a new contributor, I want a single entry point that maps the repository structure, so that I know where to look without reading every file. |
| Explore architecture     | As a new contributor, I want to see the domain map and layer structure, so that I understand the system's shape before touching code. |
| Understand conventions   | As a new contributor, I want to find the glossary and coding standards, so that I use the right terminology and style from my first commit. |
| First task               | As a new contributor, I want the task lifecycle documented step by step, so that I follow the expected workflow without guessing. |

### Journey: Deliver a feature

| Phase       | Story |
|-------------|-------|
| Understand  | As an agent starting a feature task, I want to find the relevant domain docs from AGENTS.md, so that I understand the boundaries before planning. |
| Plan        | As an engineer planning a cross-domain feature, I want to see dependency rules and domain contracts, so that I know which boundaries I must respect. |
| Implement   | As an agent writing code, I want the preferred patterns for this codebase loaded in context, so that I produce consistent, reviewable code. |
| Validate    | As an agent verifying a change, I want a checklist of what "done" means, so that I do not declare success prematurely. |
| Ship        | As an engineer reviewing a PR, I want to see which ADR applies and whether the quality grade changed, so that I can assess architectural impact. |

### Journey: Diagnose and fix a bug

| Phase         | Story |
|---------------|-------|
| Reproduce     | As an agent starting a bug fix, I want a structured reproduction workflow, so that I confirm the bug exists before attempting a fix. |
| Locate        | As an agent tracing a root cause, I want the quality grade and known issues for the affected area, so that I focus on likely sources of failure. |
| Fix           | As an agent implementing a fix, I want the anti-patterns list, so that I avoid introducing a different class of bug. |
| Verify        | As an agent verifying a fix, I want to know which tests to run and what "fixed" looks like, so that I validate against the original report. |

## When to Write New Stories

- A journey phase has no documentation serving it (gap discovered during doc-gardening or after an agent failure).
- A new journey is identified (e.g., new team role, new type of task).
- An existing story's acceptance criteria are no longer met (doc was removed, renamed, or went stale).
- A retrospective reveals a pain point at a specific journey phase.
