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

### New repository

1. Copy the contents of [`template/`](template/) into your new repository.
2. Paste the contents of `template/BOOTSTRAP.md` to your AI assistant to auto-configure the docs for your project.
3. Start building — the harness is ready.

### Existing repository

For an existing project you do not want to bulk-copy the template files in. Instead, point an AI
session at both `template/` and the target project simultaneously so the agent can read the
template as a reference and write only the files it actually generates into the project.

Two approaches are available. Both result in the same end state: harness files written into the
target project, nothing else.

#### Option A — `--add-dir` (no git overhead)

Open a Claude Code session rooted in the target project and mount this repo as an additional
read path:

```bash
claude --add-dir /path/to/ai-landscape /path/to/your-project
```

The agent can now read `template/` from this repo while writing harness files into the target
project directory. Paste the contents of `template/BOOTSTRAP.md` as the first prompt.

#### Option B — git submodule (tracked, repeatable)

Add the target project as a submodule of this repo, then open a session here. This keeps the
setup session in git and lets you revisit it later.

```bash
# From the root of this repo
git submodule add <your-project-repo-url> project
git submodule update --init
```

Open a Claude Code session in this repo — the agent can see both `template/` and `project/`
without any extra flags. Paste `template/BOOTSTRAP.md` as the first prompt, telling the agent
to write output into `project/` rather than the current directory.

When setup is complete, commit and push the submodule changes from inside `project/`:

```bash
cd project
git add .
git commit -m "feat: add harness engineering scaffold"
git push
```

Then update the submodule pointer in this repo if you want to keep the reference:

```bash
cd ..
git add project
git commit -m "chore: update project submodule after harness setup"
```

Or remove the submodule entirely if you no longer need it here:

```bash
git submodule deinit project
git rm project
git commit -m "chore: remove project submodule after harness setup"
```

## Knowledge Base

External sources that inform the template are tracked in [`knowledge/links.md`](knowledge/links.md) —
a curated index of articles, papers, and case studies with brief descriptions and links to full notes.
Use the `ingest-link` skill to add new sources.

## Contributing to the Template

All improvements to the template go into the `template/` folder.
See [AGENTS.md](AGENTS.md) for agent-specific guidance on working in this repo.
