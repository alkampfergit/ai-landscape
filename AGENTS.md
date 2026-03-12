# AGENTS.md — Agent Entry Point

> This file is the **only** file injected into agent context at session start.
> It is a map, not an encyclopedia. Follow pointers to load context on demand.

## Identity

- **Project**: [PROJECT_NAME]
- **Stack**: [e.g., .NET 9 / TypeScript / Python]
- **Architecture style**: [e.g., DDD + Event Sourcing, Clean Architecture, Microservices]

## Core Beliefs

1. The repository is the single source of truth. If it's not in the repo, it doesn't exist.
2. Every constraint must be mechanically enforced — documentation alone is not enough.
3. When a task fails, the fix is never "try harder." Ask: what capability or context is missing?
4. Prefer boring, well-understood technologies the agent can fully reason about.
5. Progressive disclosure: load only the context needed for the current task.

## Where to Look

| What you need                        | Where to find it                                        |
|--------------------------------------|---------------------------------------------------------|
| Architecture & layer rules           | [docs/architecture/ARCHITECTURE.md](docs/architecture/ARCHITECTURE.md) |
| Dependency direction enforcement     | [docs/architecture/DEPENDENCY-RULES.md](docs/architecture/DEPENDENCY-RULES.md) |
| Module & domain boundaries           | [docs/architecture/DOMAIN-BOUNDARIES.md](docs/architecture/DOMAIN-BOUNDARIES.md) |
| Design principles & beliefs          | [docs/design/DESIGN-PRINCIPLES.md](docs/design/DESIGN-PRINCIPLES.md) |
| Preferred patterns & anti-patterns   | [docs/design/PATTERNS.md](docs/design/PATTERNS.md) |
| Code quality grades by domain        | [docs/quality/QUALITY-GRADES.md](docs/quality/QUALITY-GRADES.md) |
| Code standards & style               | [docs/quality/CODE-STANDARDS.md](docs/quality/CODE-STANDARDS.md) |
| Task lifecycle (prompt → PR)         | [docs/workflows/TASK-LIFECYCLE.md](docs/workflows/TASK-LIFECYCLE.md) |
| Review & merge checklist             | [docs/workflows/REVIEW-CHECKLIST.md](docs/workflows/REVIEW-CHECKLIST.md) |
| Domain glossary                      | [docs/context/GLOSSARY.md](docs/context/GLOSSARY.md) |
| Architecture Decision Records        | [docs/context/DECISIONS.md](docs/context/DECISIONS.md) |
| Reusable task skills                 | [skills/](skills/)                                      |

## Before You Write Code

1. **Read** the relevant architecture and design docs for the domain you're touching.
2. **Check** the quality grades — know the current state of the area you're modifying.
3. **Plan** before implementing. Write your approach in a comment or plan file first.
4. **Validate** your changes against dependency rules and structural tests.
5. **Run** the full CI check before considering the task complete.

## Rules That Are Always Enforced

- Dependencies flow in ONE direction only. See [DEPENDENCY-RULES.md](docs/architecture/DEPENDENCY-RULES.md).
- Parse and validate all data at boundaries — never trust unvalidated shapes.
- Every public API must have at least one test covering the happy path.
- No manual overrides of linter rules without an ADR explaining why.

## How to Use Skills

Skills are reusable instruction sets for repeatable tasks.
Each skill lives in `skills/<skill-name>/SKILL.md`.
Read the skill file **before** starting the task — it contains the step-by-step workflow.

Available skills:

| Skill                  | Purpose                                           |
|------------------------|---------------------------------------------------|
| `new-feature`          | End-to-end workflow for adding a new feature       |
| `bug-fix`              | Structured approach to reproducing and fixing bugs  |
| `refactor`             | Safe refactoring with preservation guarantees       |
| `add-domain`           | Bootstrap a new business domain module              |
| `doc-gardening`        | Scan and fix stale documentation                    |

---
*Last verified: [DATE]. If this file feels stale, run the `doc-gardening` skill.*
