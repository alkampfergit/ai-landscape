# CLAUDE.md — Claude Code Agent Instructions

> This file is automatically loaded by Claude Code at session start.
> It is the agent's entry point — a map, not an encyclopedia.
> For full details, follow the pointers below.

## Project

- **Name**: [PROJECT_NAME]
- **Stack**: [e.g., .NET 9 / C# / TypeScript / Python]
- **Architecture**: [e.g., DDD + Event Sourcing, Clean Architecture]
- **Repo root**: This directory

## Core Beliefs

1. The repository is the single source of truth — if it's not here, it doesn't exist.
2. Every constraint is mechanically enforced (linter, CI, structural test).
3. When a task fails, fix the environment — not the symptom.
4. Prefer boring, well-understood technologies.
5. Progressive disclosure: load only the context you need right now.

## Navigation — Where to Look

| What you need                      | File                                                     |
|------------------------------------|----------------------------------------------------------|
| Full architecture map              | `docs/architecture/ARCHITECTURE.md`                      |
| Layer dependency rules             | `docs/architecture/DEPENDENCY-RULES.md`                  |
| Domain boundaries & contracts      | `docs/architecture/DOMAIN-BOUNDARIES.md`                 |
| Design principles                  | `docs/design/DESIGN-PRINCIPLES.md`                       |
| Patterns & anti-patterns           | `docs/design/PATTERNS.md`                                |
| Quality grades per domain          | `docs/quality/QUALITY-GRADES.md`                         |
| Code style & standards             | `docs/quality/CODE-STANDARDS.md`                         |
| Task lifecycle (prompt → PR)       | `docs/workflows/TASK-LIFECYCLE.md`                       |
| Pre-merge review checklist         | `docs/workflows/REVIEW-CHECKLIST.md`                     |
| Domain glossary                    | `docs/context/GLOSSARY.md`                               |
| Architecture Decision Records      | `docs/context/DECISIONS.md`                              |

## Before Writing Code

1. Read the architecture and design docs for the domain you're touching.
2. Check the quality grade of the affected area.
3. Plan before implementing — write approach first if task touches 3+ files.
4. Validate against dependency rules and run all tests before submitting.

## Always-Enforced Rules

- Dependencies flow in ONE direction: `Types → Config → Repository → Service → Runtime → UI/API`
- All data crossing a boundary must be parsed and validated.
- Every public API has at least one test covering the happy path.
- No linter rule overrides without an ADR.

## Skills — Reusable Workflows

Read the skill file BEFORE starting the task.

| Skill                         | When to use                              |
|-------------------------------|------------------------------------------|
| `skills/new-feature/SKILL.md` | Implementing a new feature               |
| `skills/bug-fix/SKILL.md`     | Reproducing and fixing a bug             |
| `skills/refactor/SKILL.md`    | Improving code without behavior change   |
| `skills/add-domain/SKILL.md`  | Bootstrapping a new business domain      |
| `skills/doc-gardening/SKILL.md`| Scanning and fixing stale documentation |

## Build & Test Commands

```bash
# [CUSTOMIZE: replace with actual commands for your stack]

# Build
# dotnet build / npm run build / make build

# Test
# dotnet test / npm test / pytest

# Lint
# dotnet format --verify-no-changes / npm run lint / ruff check .

# Structural tests (dependency rule verification)
# [add your structural test command here]
```

## Commit Convention

Format: `[type]: [brief description]`

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`
