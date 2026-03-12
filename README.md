# Harness Engineering — Initial Repository Bootstrap

> **Harness engineering** is the discipline of designing environments, constraints,
> and feedback loops that make AI coding agents reliable at scale.
>
> This repository provides the initial file structure and documentation to bootstrap
> an agent-first development workflow.

## What Is This?

This is a template repository implementing the harness engineering principles
described by OpenAI's Codex team and refined by the broader community. The key
insight: **the bottleneck is never the agent's ability to write code, but the
lack of structure, tools, and feedback mechanisms surrounding it.**

When an agent fails, the fix is never "try harder." The fix is always:
*what capability or context is missing, and how do we make it enforceable?*

## Repository Structure

```
├── AGENTS.md                              # Agent entry point (~100 lines, table of contents)
├── README.md                              # This file (human-facing)
├── docs/
│   ├── architecture/
│   │   ├── ARCHITECTURE.md                # System architecture map
│   │   ├── DEPENDENCY-RULES.md            # Layer dependency enforcement
│   │   └── DOMAIN-BOUNDARIES.md           # Bounded contexts and contracts
│   ├── design/
│   │   ├── DESIGN-PRINCIPLES.md           # Core beliefs guiding all decisions
│   │   └── PATTERNS.md                    # Preferred patterns & anti-patterns
│   ├── quality/
│   │   ├── QUALITY-GRADES.md              # Per-domain quality tracking
│   │   └── CODE-STANDARDS.md              # Style, formatting, testing rules
│   ├── workflows/
│   │   ├── TASK-LIFECYCLE.md              # Prompt → PR lifecycle
│   │   └── REVIEW-CHECKLIST.md            # Pre-merge verification
│   └── context/
│       ├── GLOSSARY.md                    # Domain terminology
│       └── DECISIONS.md                   # Architecture Decision Records
└── skills/
    ├── new-feature/SKILL.md               # Feature implementation workflow
    ├── bug-fix/SKILL.md                   # Bug reproduction and fix workflow
    ├── refactor/SKILL.md                  # Safe refactoring with guarantees
    ├── add-domain/SKILL.md                # New domain scaffolding
    └── doc-gardening/SKILL.md             # Documentation maintenance
```

## How Progressive Disclosure Works

The agent sees this hierarchy of information:

```
AGENTS.md  ← always loaded (the map, ~100 lines)
    │
    ├── docs/architecture/*  ← loaded when touching structure
    ├── docs/design/*        ← loaded when making design decisions
    ├── docs/quality/*       ← loaded to assess area health
    ├── docs/workflows/*     ← loaded to follow the correct process
    ├── docs/context/*       ← loaded for terminology and past decisions
    │
    └── skills/*             ← loaded for specific task types
```

The agent loads only what it needs for the current task, keeping context
focused and effective.

## How to Adopt

### 1. Customize AGENTS.md

Replace all `[PLACEHOLDERS]` with your project specifics:
- Project name, tech stack, architecture style.
- Add or remove skills based on your workflow.

### 2. Fill In Architecture Docs

- Define your actual domains in ARCHITECTURE.md and DOMAIN-BOUNDARIES.md.
- Set your real dependency rules in DEPENDENCY-RULES.md.
- Write your initial ADRs in DECISIONS.md.

### 3. Adapt Code Standards

- Update CODE-STANDARDS.md for your language/framework.
- Add language-specific sections.
- Configure linters and formatters to match.

### 4. Build Enforcement

The documentation is only as good as its enforcement. For each rule:
- Write a linter rule, structural test, or CI check.
- Include remediation instructions in error messages.
- The error message IS the teaching mechanism for the agent.

### 5. Start the Feedback Loop

As you work:
- When the agent makes a mistake, don't just fix it — ask: *what was missing?*
- Update the relevant doc, add a linter rule, or create a new skill.
- Run `doc-gardening` regularly to keep everything current.
- Every agent failure is an opportunity to improve the harness.

## Core Principles

1. **Repository is the source of truth** — if it's not in the repo, it doesn't exist for the agent.
2. **Enforce mechanically** — documentation without enforcement is just suggestion.
3. **Progressive disclosure** — give the agent a map, not an encyclopedia.
4. **Fail forward** — every failure improves the harness for next time.
5. **Boring technology wins** — agents reason best about well-understood, stable tools.

## Credits

Inspired by OpenAI's harness engineering methodology, Mitchell Hashimoto's
harness engineering concept, Anthropic's long-running agent patterns, and the
broader agent-first engineering community.
