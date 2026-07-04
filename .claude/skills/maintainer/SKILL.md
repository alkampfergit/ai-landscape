---
name: maintainer
description: >
  Re-analyze the template/ folder for internal consistency, keep
  template/structure.md (an annotated map of the template layout) up to date,
  and keep the meta/skill-list.md index in sync with the template skills.
  Use when checking template consistency, after adding/renaming/removing files
  under template/, when verifying internal links and skill tables resolve, or
  when asked to "run the maintainer", "check the template", "audit the
  template", or "update the template structure". Do NOT use for ingesting
  external sources (use ingest-link) or for editing project content unrelated
  to template consistency.
metadata:
  author: ai-landscape
  version: 1.0.0
  category: workflow
---

# Skill: maintainer

## Purpose

Keep the `template/` folder — the product of this authoring repository — internally
consistent, and maintain a dedicated structure map at `template/structure.md`.

This skill has two responsibilities:

1. **Consistency analysis** — audit `template/` so every internal link, skill-table row,
   "Where to Look" entry, and cross-document reference resolves correctly, and no
   placeholder or stale content remains.
2. **Structure map** — generate and keep up to date `template/structure.md`, a standalone
   annotated tree of the template folder. `template/BOOTSTRAP.md` describes the structure
   inline for bootstrapping; `structure.md` is the separate, always-current reference map.
3. **Skill index** — keep the root index `meta/skill-list.md` in sync with the skills under
   `template/.claude/skills/` (the one root file this skill controls, since it indexes
   template content).

## When to Use

Use this skill whenever:

- Files are added, renamed, moved, or removed under `template/`.
- You want to verify the template is self-consistent before committing or releasing.
- The structure of the template folder changed and `template/structure.md` needs refreshing.
- The user asks to "run the maintainer", "check/audit the template", "verify template
  consistency", or "update the template structure".

This skill operates **on the `template/` folder**, plus the single root index
`meta/skill-list.md` (which catalogs `template/.claude/skills/`). It does not touch other
root authoring infrastructure (root `AGENTS.md`, the rest of `meta/`, `knowledge/`) except to
read it for context.

---

## Inputs

| Input | Description |
|-------|-------------|
| (none) | The skill always audits the entire `template/` folder. Optionally the user may scope it to a subset (e.g. "just the docs"). |

---

## Workflow

Work through each phase in order. Do not skip phases. Report findings as you go.

---

### Phase 1 — Inventory the Template Folder

1. Build a complete file list of `template/` (including dotfiles and nested directories),
   excluding any `.git` paths. Use:

   ```bash
   find template -type f -not -path '*/.git/*' | sort
   ```

2. Build the directory list as well:

   ```bash
   find template -type d -not -path '*/.git/*' | sort
   ```

3. Record the current set of skills under `template/.claude/skills/` (one folder per skill,
   each containing a `SKILL.md`).

4. Hold this inventory as the source of truth for the rest of the run.

---

### Phase 2 — Consistency Checks

Run each check below against the inventory from Phase 1. Classify every finding as
**Critical** (broken reference / contradicts reality), **Warning** (stale or inconsistent
but not broken), or **Info** (cosmetic). Fix Critical findings; surface Warnings and ask
before large rewrites.

#### Check 1 — Internal links resolve

For every Markdown file under `template/`, extract relative links of the form `[text](path)`
(ignore external `http(s)://` links and pure anchors). For each, verify the target file or
directory exists relative to the linking file's location.

- Missing target → **Critical**. Fix by correcting the path or removing the dead link.

#### Check 2 — `template/AGENTS.md` "Where to Look" table

Every path referenced in the "Where to Look" table must exist in the inventory.

- Entry points to a non-existent file/dir → **Critical**.
- A significant top-level file/dir exists but has no entry → **Warning** (consider adding).

#### Check 3 — Skill table matches actual skills

Compare the Skills table in `template/AGENTS.md` against the actual skill folders under
`template/.claude/skills/`.

- Skill folder exists but no table row → **Critical** (add row).
- Table row points to a missing `SKILL.md` → **Critical** (fix or remove).
- Each `SKILL.md` should have valid YAML frontmatter with `name` and `description`; `name`
  must match its folder name → **Warning** if mismatched.

#### Check 4 — `template/README.md` structure diagram

If `template/README.md` contains a file-tree / structure diagram, verify it matches the
actual layout (no listed files that are absent, no major directories missing).

- Diagram lists a path that does not exist → **Critical**.
- A new top-level directory is missing from the diagram → **Warning**.

#### Check 5 — No leftover placeholders

Search template docs for unresolved authoring placeholders:

```bash
grep -rEn '\[PLACEHOLDER\]|\[e\.g\.,|TODO|FIXME' template --include='*.md'
```

- `[PLACEHOLDER]` / `[e.g.,` left in a non-`BOOTSTRAP.md` doc → **Warning** (the template
  ships with intentional fill-in points only inside `BOOTSTRAP.md`). Use judgment: do not
  strip placeholders that are part of the template's design.

#### Check 6 — Cross-document agreement

Spot-check that documents that reference each other agree (e.g. layer names in
`DEPENDENCY-RULES.md` vs `ARCHITECTURE.md`, glossary terms used consistently). Flag clear
contradictions as **Warning**.

#### Check 7 — `template/structure.md` is current

Confirm `template/structure.md` exists and its tree matches the Phase 1 inventory. If it is
missing or stale, it will be regenerated in Phase 3.

#### Check 8 — `meta/skill-list.md` matches the template skills

The root index `meta/skill-list.md` catalogs the skills under
`template/.claude/skills/`. This is the one file outside `template/` that the maintainer
controls, because it is an index *of* template content. Compare its table against the actual
skill folders from Phase 1.

- Skill folder exists but no row in `meta/skill-list.md` → **Critical** (add row).
- Row points to a skill that no longer exists → **Critical** (remove row).
- Row description has drifted from the skill's `description` frontmatter → **Warning**.
- Rows out of alphabetical order → **Info**.

`skill-author` owns this index at authoring time; the maintainer catches drift at audit
time. When fixing, derive each row from the skill's own frontmatter — never invent rows.

---

### Phase 3 — Generate / Update `template/structure.md`

Produce `template/structure.md` as a standalone, annotated map of the template folder.

1. Build the tree from the Phase 1 inventory (directories and files under `template/`,
   excluding `.git`). Keep it to a reasonable depth — list every directory and every file,
   but you may collapse large uniform groups with a short note rather than listing each.
2. Annotate each entry with a one-line purpose. Derive annotations from the file itself
   (first heading or frontmatter `description`) rather than inventing them.
3. Use the format below. Set the verification date to today's date.

```markdown
# Template Structure

> Standalone map of the `template/` folder, maintained by the `maintainer` skill.
> For the bootstrapping narrative, see [BOOTSTRAP.md](BOOTSTRAP.md). If this file
> looks stale, run the `maintainer` skill.

## Tree

\`\`\`
template/
├── AGENTS.md          ← <one-line purpose>
├── ...
\`\`\`

## Entries

| Path | Purpose |
|------|---------|
| `AGENTS.md` | <one-line purpose> |
| ... | ... |

---
*Last verified: <YYYY-MM-DD> by the `maintainer` skill.*
```

4. Write the file to `template/structure.md`.
5. If this is the first time `structure.md` is created, ensure it is discoverable: add a
   "Template structure map" row to the "Where to Look" table in `template/AGENTS.md` and,
   if a structure diagram exists in `template/README.md`, add `structure.md` to it.

---

### Phase 4 — Apply Fixes

For each **Critical** finding from Phase 2:

1. Apply the minimal fix that restores consistency (correct a path, add a missing skill row,
   update the structure diagram).
2. Preserve each file's existing tone, structure, and formatting conventions.
3. Do not introduce project-specific content — the template must stay general-purpose.

For **Warning** findings, fix the unambiguous ones; for anything requiring a judgment call or
a large rewrite, list it for the user and ask before proceeding.

After applying fixes, re-run the relevant checks from Phase 2 to confirm they pass.

---

### Phase 5 — Summary Report

Output a concise summary:

```
## maintainer complete

**Files scanned**: <N> under template/

**Consistency findings**:
- Critical: <count> (<fixed/remaining>)
- Warning:  <count>
- Info:     <count>

**Fixes applied**:
- <file> — <one-line change>
- …

**structure.md**: <created | updated | unchanged>

**Needs your decision**:
- <finding requiring judgment, or "none">
```

---

## Guardrails

- **Template-scoped.** Operate inside `template/`, plus the single root index
  `meta/skill-list.md`. Read other root files for context but do not modify root `AGENTS.md`,
  the rest of `meta/`, or `knowledge/` from this skill.
- **Stay general-purpose.** Never add project-, company-, or technology-specific content to
  the template.
- **Minimal footprint.** Each fix is the smallest change that restores consistency. Do not
  rewrite whole documents to fix a single broken link.
- **Don't invent annotations.** Structure-map purposes come from the file's own heading or
  frontmatter, not from guesswork.
- **Preserve intentional placeholders.** `BOOTSTRAP.md` and other fill-in points are designed
  to contain `[PLACEHOLDER]` text — do not strip them.
- **Verify before claiming done.** Re-run checks after fixing; report what still needs a human
  decision rather than forcing a change.
