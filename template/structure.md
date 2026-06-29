# Template Structure

> Standalone map of the `template/` folder, maintained by the `maintainer` skill.
> For the bootstrapping narrative and how to fill these files in, see
> [BOOTSTRAP.md](BOOTSTRAP.md). If this file looks stale, run the `maintainer` skill.

## Tree

```
template/
├── AGENTS.md                         ← Canonical agent entry point (the map)
├── CLAUDE.md                         ← Claude Code bootstrap (points to AGENTS.md)
├── BOOTSTRAP.md                      ← Prompt for configuring the template in a new project
├── README.md                        ← Human-facing template documentation
├── structure.md                     ← This file: standalone map of the template layout
├── LICENSE                          ← MIT license
├── docs/
│   ├── architecture/
│   │   ├── ARCHITECTURE.md           ← System architecture map
│   │   ├── DEPENDENCY-RULES.md       ← Layer dependency direction rules
│   │   └── DOMAIN-BOUNDARIES.md      ← Bounded contexts and integration contracts
│   ├── design/
│   │   ├── DESIGN-PRINCIPLES.md      ← Core principles guiding decisions
│   │   └── PATTERNS.md               ← Preferred patterns & anti-patterns
│   ├── quality/
│   │   ├── CODE-STANDARDS.md         ← Style, formatting, and testing rules
│   │   └── QUALITY-GRADES.md         ← Per-domain quality tracking
│   ├── workflows/
│   │   ├── TASK-LIFECYCLE.md         ← Prompt → PR task lifecycle
│   │   └── REVIEW-CHECKLIST.md       ← Pre-merge verification checklist
│   └── context/
│       ├── GLOSSARY.md               ← Domain terminology
│       └── DECISIONS.md              ← Architecture Decision Records (ADRs)
├── templates/
│   ├── pr.template.md                ← PR description template
│   ├── adr.template.md               ← ADR template
│   ├── commit.template.md            ← Commit message template
│   └── user-story.template.md        ← User story template
├── .github/
│   ├── PULL_REQUEST_TEMPLATE.md      ← GitHub PR template
│   └── copilot-instructions.md       ← GitHub Copilot workspace instructions
└── .claude/
    ├── settings.local.json           ← Local Claude Code settings
    ├── agents/
    │   └── agents-md-sync.md         ← Agent that keeps AGENTS.md in sync with file changes
    └── skills/                       ← Claude Code skills (single source of truth)
        ├── new-feature/SKILL.md      ← End-to-end feature implementation workflow
        ├── bug-fix/SKILL.md          ← Bug reproduction and fix workflow
        ├── refactor/SKILL.md         ← Safe refactoring with preservation guarantees
        ├── add-domain/SKILL.md       ← New business domain scaffolding
        ├── doc-gardening/SKILL.md    ← Documentation maintenance
        └── meta/SKILL.md             ← End-of-task skill creation/update workflow
```

## Entries

| Path | Purpose |
|------|---------|
| `AGENTS.md` | Canonical agent entry point; the map that links to all detail docs. |
| `CLAUDE.md` | Claude Code auto-load bootstrap; redirects to `AGENTS.md`. |
| `BOOTSTRAP.md` | Copy-paste prompt that configures the template for a specific project. |
| `README.md` | Human-facing overview of the template. |
| `structure.md` | This standalone structure map, maintained by the `maintainer` skill. |
| `LICENSE` | MIT license. |
| `docs/architecture/ARCHITECTURE.md` | System architecture map: overview, domain map, infrastructure. |
| `docs/architecture/DEPENDENCY-RULES.md` | Layer dependency direction rules and enforcement. |
| `docs/architecture/DOMAIN-BOUNDARIES.md` | Domain registry, bounded contexts, integration contracts. |
| `docs/design/DESIGN-PRINCIPLES.md` | Core design principles guiding all decisions. |
| `docs/design/PATTERNS.md` | Preferred patterns and anti-patterns. |
| `docs/quality/CODE-STANDARDS.md` | Style, formatting, naming, and testing rules. |
| `docs/quality/QUALITY-GRADES.md` | Per-domain quality grades and risk areas. |
| `docs/workflows/TASK-LIFECYCLE.md` | The prompt-to-PR task lifecycle. |
| `docs/workflows/REVIEW-CHECKLIST.md` | Pre-merge self-review checklist. |
| `docs/context/GLOSSARY.md` | Domain terminology and in-code representations. |
| `docs/context/DECISIONS.md` | Architecture Decision Records. |
| `templates/pr.template.md` | PR description output template. |
| `templates/adr.template.md` | ADR output template. |
| `templates/commit.template.md` | Commit message output template. |
| `templates/user-story.template.md` | User story output template. |
| `.github/PULL_REQUEST_TEMPLATE.md` | GitHub-native PR template. |
| `.github/copilot-instructions.md` | GitHub Copilot workspace instructions (adapter to `AGENTS.md`). |
| `.claude/settings.local.json` | Local Claude Code settings for the template. |
| `.claude/agents/agents-md-sync.md` | Sub-agent that reconciles `AGENTS.md` after file changes. |
| `.claude/skills/new-feature/SKILL.md` | End-to-end workflow for adding a new feature. |
| `.claude/skills/bug-fix/SKILL.md` | Workflow for reproducing and fixing bugs. |
| `.claude/skills/refactor/SKILL.md` | Safe refactoring with preservation guarantees. |
| `.claude/skills/add-domain/SKILL.md` | Bootstrap a new business domain module. |
| `.claude/skills/doc-gardening/SKILL.md` | Scan and fix stale documentation. |
| `.claude/skills/meta/SKILL.md` | End-of-task retrospective for updating/creating skills. |

---
*Last verified: 2026-06-29 by the `maintainer` skill.*
