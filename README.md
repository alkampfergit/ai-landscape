# AI Landscape — Harness Engineering Template Authoring Repo

This repository **authors and maintains** the harness engineering template for agent-first development.

The template itself lives in [`template/`](template/).

## What Is the Template?

The `template/` folder is a ready-to-use bootstrap for any software project that wants to work
effectively with AI coding agents. It implements **harness engineering** principles:

> *The bottleneck is never the agent's ability to write code.
> It is the lack of structure, context, and enforcement mechanisms surrounding it.*

When you instantiate the template in a new project, you get:
- A canonical `AGENTS.md` entry point and progressive-disclosure documentation structure
- Architecture, design, quality, and workflow docs ready to be filled in
- Reusable Claude Code skills (`new-feature`, `bug-fix`, `refactor`, `add-domain`, etc.)
- Output templates for PRs, ADRs, and commit messages
- A `BOOTSTRAP.md` prompt to auto-configure everything for your specific project

## How to Use the Template

1. Copy the contents of [`template/`](template/) into your new repository.
2. Paste the contents of `template/BOOTSTRAP.md` to your AI assistant to auto-configure the docs for your project.
3. Start building — the harness is ready.

## Contributing to the Template

All improvements to the template go into the `template/` folder.
See [AGENTS.md](AGENTS.md) for agent-specific guidance on working in this repo.
