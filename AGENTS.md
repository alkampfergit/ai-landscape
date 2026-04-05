# AGENTS.md — AI Landscape: Template Authoring Repository

> This is the canonical instruction file for this repository.
> Tool-specific bootstrap files (CLAUDE.md, etc.) exist only to redirect here.
> If any file disagrees with this one, this file wins.

## What This Repository Is

**ai-landscape** is the authoring repository for the **harness engineering template**.

It is _not_ a project to develop features or services in.
Its single purpose is to author, improve, and maintain the contents of the `template/` folder —
a ready-to-use bootstrap for agent-first, harness-engineered repositories.

## Repository Structure

```
/
├── AGENTS.md          ← You are here. Canonical agent entry point.
├── CLAUDE.md          ← Claude Code auto-load (redirects here)
├── README.md          ← Human-facing overview of this authoring repo
├── .gitignore         ← Root ignore rules for this repo
├── knowledge/         ← External sources that inform the template
│   └── links.md       ← Master index of ingested URLs and their insights
├── .claude/           ← Authoring-repo skills (distinct from template skills)
│   └── skills/
│       └── ingest-link/SKILL.md  ← Ingest a URL, update docs, patch template
└── template/          ← The harness engineering template (the product)
    ├── AGENTS.md      ← Template's own agent entry point
    ├── CLAUDE.md      ← Template's Claude Code bootstrap
    ├── BOOTSTRAP.md   ← Prompt for configuring the template in a new project
    ├── README.md      ← Template's human-facing documentation
    ├── LICENSE
    ├── docs/          ← Template documentation (architecture, design, quality, etc.)
    ├── templates/     ← Output templates for PRs, ADRs, commits
    ├── meta/          ← Harness engineering guides and skill authoring notes
    ├── extra/         ← Human reference links
    ├── .github/       ← GitHub-specific files (PR template, Copilot instructions)
    └── .claude/       ← Template's Claude Code skills and agent configuration
```

## Harness Model (for this repo)

- Humans author and improve the template content under `template/`.
- Agents assist with editing template files, maintaining consistency, and improving documentation.
- When suggesting improvements, always ask: *does this belong in the template, or is it specific to the authoring repo?*
- Keep this file short. All template-specific guidance lives inside `template/AGENTS.md`.

## Default Rules

1. All changes target the `template/` folder unless explicitly stated otherwise.
2. The template must remain self-consistent: internal links, skill references, and doc cross-references inside `template/` must always resolve correctly.
3. When editing template docs, preserve the progressive-disclosure pattern: `template/AGENTS.md` is the map, linked docs are the detail.
4. Do not introduce project-specific content into the template. It must stay general-purpose.
5. When adding or renaming markdown files inside `template/`, run the `agents-md-sync` skill to keep `template/AGENTS.md` in sync.
6. When a new external source should inform the template, use the `ingest-link` skill — do not manually edit `knowledge/links.md` or template docs in isolation.

## Where to Look

| What you need                          | Where to find it |
|----------------------------------------|------------------|
| The template itself                    | [template/](template/) |
| Template agent entry point             | [template/AGENTS.md](template/AGENTS.md) |
| Template documentation                 | [template/docs/](template/docs/) |
| Template skills (Claude Code)          | [template/.claude/skills/](template/.claude/skills/) |
| Bootstrap prompt for new projects      | [template/BOOTSTRAP.md](template/BOOTSTRAP.md) |
| Harness engineering theory             | [template/meta/hareness-foundation.md](template/meta/hareness-foundation.md) |
| Skill authoring guide                  | [template/meta/skill-guide.md](template/meta/skill-guide.md) |
| External knowledge sources index       | [knowledge/links.md](knowledge/links.md) |
| Authoring-repo skills                  | [.claude/skills/](.claude/skills/) |

## Authoring Skills

Skills for maintaining this authoring repository (distinct from skills bundled in the template).

| Skill | Purpose | Location |
|-------|---------|----------|
| `ingest-link` | Fetch a URL, extract insights, update knowledge index, patch template docs | [.claude/skills/ingest-link/SKILL.md](.claude/skills/ingest-link/SKILL.md) |

## Before You Make Changes

1. Confirm your change targets `template/` content (not root authoring infrastructure).
2. Read the relevant section of `template/AGENTS.md` and any linked docs you will touch.
3. After adding, removing, or renaming markdown files in `template/`, verify cross-references are intact.
4. Self-review: does the change keep the template general-purpose and self-consistent?

---
*This repository authors the harness engineering template. The template lives in `template/`.*
