# AGENTS.md — Canonical Agent Entry Point

> This is the canonical instruction file for the repository.
> Tool-specific bootstrap files may exist, but they must only point here.
> Treat this file as the map and the linked docs as the system of record.

## Harness Model

- Humans design the environment, specify intent, and review outcomes.
- Agents execute inside the repository using repository-local knowledge.
- When a task fails, do not "try harder" blindly. Identify the missing capability, context, or enforcement and encode the fix in the repo.
- Keep this file short and stable. Push detail into the linked docs, skills, and checks.

## Identity

- **Project**: Harness engineering repository bootstrap
- **Purpose**: Template for agent-first repositories with progressive disclosure and mechanical enforcement
- **Style**: Tool-agnostic, repository-first, documentation-backed

## Default Rules

1. The repository is the single source of truth. If knowledge matters, store it here.
2. Use progressive disclosure. Load only the docs and skills relevant to the task.
3. Follow the task lifecycle before changing code: understand, plan, implement, validate, review, merge.
4. Prefer boring, legible, well-understood patterns over clever abstractions.
5. Enforce important rules mechanically with CI, linting, structural tests, or scripts.
6. When behavior, process, or architecture changes, update the repository documentation in the same change.
7. Tool-specific files must not redefine repository policy. They are adapters, not alternate sources of truth.

## Shared Engineering Invariants

- Dependencies flow one direction only: Types → Config → Repository → Service → Runtime → UI/API.
- Validate and parse data at boundaries. Never trust unvalidated input shapes.
- Prefer result types for expected failures and exceptions for programmer errors.
- Use constructor injection for dependencies.
- Use structured logging with correlation IDs when logging is part of the system.
- Keep files and functions small enough to stay legible. See the code standards for the current limits.
- Every public API should have at least one happy-path test.
- No linter or rule override without a documented decision.

## Where to Look

| What you need                      | Where to find it |
|------------------------------------|------------------|
| System architecture map            | [docs/architecture/ARCHITECTURE.md](docs/architecture/ARCHITECTURE.md) |
| Dependency direction rules         | [docs/architecture/DEPENDENCY-RULES.md](docs/architecture/DEPENDENCY-RULES.md) |
| Domain boundaries and contracts    | [docs/architecture/DOMAIN-BOUNDARIES.md](docs/architecture/DOMAIN-BOUNDARIES.md) |
| Design principles                  | [docs/design/DESIGN-PRINCIPLES.md](docs/design/DESIGN-PRINCIPLES.md) |
| Preferred patterns and anti-patterns | [docs/design/PATTERNS.md](docs/design/PATTERNS.md) |
| Code standards and style           | [docs/quality/CODE-STANDARDS.md](docs/quality/CODE-STANDARDS.md) |
| Quality grades and risk areas      | [docs/quality/QUALITY-GRADES.md](docs/quality/QUALITY-GRADES.md) |
| Task workflow                      | [docs/workflows/TASK-LIFECYCLE.md](docs/workflows/TASK-LIFECYCLE.md) |
| Review checklist                   | [docs/workflows/REVIEW-CHECKLIST.md](docs/workflows/REVIEW-CHECKLIST.md) |
| Terminology                        | [docs/context/GLOSSARY.md](docs/context/GLOSSARY.md) |
| Decisions and ADRs                 | [docs/context/DECISIONS.md](docs/context/DECISIONS.md) |
| Repeatable task workflows          | [.claude/skills/](.claude/skills/) |
| Output templates (PRs, ADRs, commits) | [templates/](templates/) |
| Initial project setup prompt       | [BOOTSTRAP.md](BOOTSTRAP.md) |
| Human reference files (not for agents) | [extra/](extra/) |

## Before You Write Code

1. Read the relevant architecture, design, and quality docs for the area you will touch.
2. Identify whether the task maps to an existing skill and load it before starting.
3. If the task touches 3 or more files or crosses boundaries, write a brief plan in the repo or task context first.
4. Validate changes with the relevant tests, linters, and structural checks.
5. Self-review against the review checklist before considering the task complete.

## Skills

Skills are reusable workflows for repeatable tasks.
They live in `.claude/skills/` as the single source of truth (Claude Code-first).
Users adopting other AI tools can copy skills from `.claude/skills/` to their tool's expected location.
Read the skill before starting the work it covers.

| Skill | Purpose | Location |
|-------|---------|----------|
| `new-feature` | End-to-end workflow for adding a new feature | [.claude/skills/new-feature/SKILL.md](.claude/skills/new-feature/SKILL.md) |
| `bug-fix` | Structured workflow for reproducing and fixing bugs | [.claude/skills/bug-fix/SKILL.md](.claude/skills/bug-fix/SKILL.md) |
| `refactor` | Safe refactoring with preservation guarantees | [.claude/skills/refactor/SKILL.md](.claude/skills/refactor/SKILL.md) |
| `add-domain` | Bootstrap a new business domain module | [.claude/skills/add-domain/SKILL.md](.claude/skills/add-domain/SKILL.md) |
| `doc-gardening` | Scan and fix stale documentation | [.claude/skills/doc-gardening/SKILL.md](.claude/skills/doc-gardening/SKILL.md) |
| `meta` | End-of-task retrospective workflow for updating or creating skills | [.claude/skills/meta/SKILL.md](.claude/skills/meta/SKILL.md) |

## Tool-Specific Bootstrap Files

- [CLAUDE.md](CLAUDE.md) exists only because Claude Code auto-loads it.
- [.github/copilot-instructions.md](.github/copilot-instructions.md) exists only because GitHub Copilot supports workspace instructions there.
- If any bootstrap file disagrees with this file, AGENTS.md wins.

---
*Last verified: 2026-03-14. If this file feels stale, run the `doc-gardening` skill.*
