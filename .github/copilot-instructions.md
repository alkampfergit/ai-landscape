# Copilot Instructions

## Project Context

This project uses a harness engineering approach where AI agents are the primary
code authors. Humans design the environment, specify intent, and review output.

## Key Principles

- Dependencies flow one direction: Types → Config → Repository → Service → Runtime → UI/API
- Validate all data at boundaries — never trust unvalidated shapes
- Prefer boring, well-understood patterns over clever solutions
- Every rule must be mechanically enforced (linter, CI, test)

## Before Writing Code

1. Check `docs/architecture/ARCHITECTURE.md` for the system map
2. Check `docs/design/PATTERNS.md` for preferred patterns
3. Check `docs/quality/QUALITY-GRADES.md` for area health
4. Follow `docs/workflows/TASK-LIFECYCLE.md` for the correct process

## Code Standards

- One concept per file, max 300 lines per file, max 30 lines per function
- Constructor injection for dependencies
- Result types for expected failures, exceptions for programmer errors
- Structured logging with correlation IDs
- Test naming: `[unit]_[scenario]_[expected]`

## Skills

For repeatable tasks, read the corresponding skill file before starting:

- `skills/new-feature/SKILL.md` — feature implementation
- `skills/bug-fix/SKILL.md` — bug reproduction and fix
- `skills/refactor/SKILL.md` — safe refactoring
- `skills/add-domain/SKILL.md` — new domain scaffolding
- `skills/doc-gardening/SKILL.md` — documentation maintenance

## Commit Convention

Format: `[type]: [brief description]`
Types: feat, fix, refactor, docs, test, chore
