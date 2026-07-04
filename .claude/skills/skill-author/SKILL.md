---
name: skill-author
description: >
  Author, update, or remove a Claude Code skill inside the template
  (template/.claude/skills/), following meta/skill-guide.md as the design rubric,
  and keep the meta/skill-list.md index table in sync. Use when asked to create a
  new template skill, write a skill, add a skill to the template, update or rename
  an existing template skill, or regenerate the template skill list. Do NOT use to
  author authoring-repo skills under the root .claude/skills/ (those are not part
  of the template product).
metadata:
  author: ai-landscape
  version: 1.0.0
  category: authoring
---

# Skill: Skill Author

Use this skill to create, update, or remove a skill that ships **inside the
template** (`template/.claude/skills/`), and to keep the
[meta/skill-list.md](../../../meta/skill-list.md) index in sync afterward.

## Scope

This is an **authoring-repo** skill. It produces and maintains skills that are
part of the template product.

- **In scope**: skills under `template/.claude/skills/<skill-name>/SKILL.md`.
- **Out of scope**: authoring-repo skills under the root `.claude/skills/`
  (e.g. `ingest-link`, this skill). Those are tooling for maintaining the
  authoring repo, not part of the shipped template.

The template also bundles its own `meta` skill, which captures learnings *from
within a downstream project*. This `skill-author` skill is different: it authors
template skills *from the authoring repo* and owns the `meta/skill-list.md` index.

## Required Reading

Before drafting or editing any skill, read
[meta/skill-guide.md](../../../meta/skill-guide.md). It is the authoritative
rubric. Pay particular attention to:

- **Mastering the Description Field** — what + when + how, with diverse trigger phrases.
- **Writing the SKILL.md File** — structure, being specific and actionable, not over-explaining.
- **YAML Frontmatter Reference** — required and optional fields.
- **Progressive Disclosure** — keep SKILL.md lean; push detail to `references/`.
- **Quick Checklists** — use the during-development and testing checklists.

## Workflow

### Step 1: Confirm the Skill Belongs in the Template

Ask:
1. Is this a reusable workflow that downstream projects benefit from?
2. Is it general-purpose, not specific to this authoring repo?

If it is authoring-repo tooling, stop — it does not belong under `template/`.

### Step 2: Decide Create vs. Update

1. List existing template skills: read `template/.claude/skills/*/SKILL.md`.
2. If the new guidance fits an existing skill's scope, **update that skill**
   rather than creating a near-duplicate.
3. Create a new skill only when the scope is clearly distinct and can be
   described with its own trigger phrases.

### Step 3: Gather Requirements

Clarify before writing:
- **Name**: kebab-case, no `claude`/`anthropic`, matches the folder name.
- **Purpose**: one sentence on what the skill does.
- **Triggers**: the diverse phrases a user would say to invoke it.
- **Workflow**: the ordered steps, decision points, and validation gates.
- **Guardrails**: what the skill must never do.

If any answer is unclear, ask before writing files.

### Step 4: Write the SKILL.md

Create `template/.claude/skills/<skill-name>/SKILL.md` with valid YAML
frontmatter and a body that follows `meta/skill-guide.md`. Match the conventions
of the existing template skills:

```yaml
---
name: <skill-name>
description: >
  [What it does]. Use when [diverse trigger phrases].
metadata:
  author: ai-landscape
  version: 1.0.0
  category: workflow
---
```

Body essentials: a short overview, an ordered workflow, concrete examples, a
troubleshooting section, and a guardrails section. Use relative forward-slash
paths for any cross-references. Move long detail into `references/` rather than
bloating SKILL.md.

### Step 5: Refine Against the Guide

Launch a subagent to review the new or updated skill against
`meta/skill-guide.md`. Provide:
- The path of the skill you changed.
- Whether it is new or an update.
- The instruction to use `meta/skill-guide.md` as the rubric.
- A request to critique the description, trigger phrases, structure, examples,
  and troubleshooting.

Apply the resulting improvements.

### Step 6: Update meta/skill-list.md

Regenerate the table in [meta/skill-list.md](../../../meta/skill-list.md) so it
reflects the current contents of `template/.claude/skills/`:

1. Enumerate every `template/.claude/skills/*/SKILL.md`.
2. For each, take the skill name from the folder and a one-line description
   distilled from its `description` frontmatter.
3. Rewrite the table with one row per skill, alphabetically by name.
4. Remove rows for skills that no longer exist; add rows for new ones.

The index must always match the directory exactly — no missing, stale, or extra rows.

### Step 7: Keep the Template's AGENTS.md in Sync

Template skills are also listed in `template/AGENTS.md`. If a skill was added,
removed, or renamed, update that table too (or delegate to the template's
`agents-md-sync` agent at
[template/.claude/agents/agents-md-sync.md](../../../template/.claude/agents/agents-md-sync.md)).
`meta/skill-list.md` is the authoring-repo index; `template/AGENTS.md` is the
in-template map. Both must stay consistent.

### Step 8: Validate

- [ ] `SKILL.md` exists at the skill folder root, spelled exactly.
- [ ] Frontmatter is valid YAML with `name` and `description`; `name` matches the folder.
- [ ] No XML tags in frontmatter; name has no `claude`/`anthropic`.
- [ ] Description states what + when with multiple trigger phrases.
- [ ] `meta/skill-list.md` matches the skill directory exactly.
- [ ] `template/AGENTS.md` skill references resolve and are current.

## Examples

**Example 1: Create a new template skill**

User says: "Add a `release-notes` skill to the template that drafts release notes from merged PRs."

Actions:
1. Confirm it is general-purpose template content.
2. Verify no existing skill covers it.
3. Write `template/.claude/skills/release-notes/SKILL.md` per the guide.
4. Run a subagent refinement pass against `meta/skill-guide.md`.
5. Add the `release-notes` row to `meta/skill-list.md` and update `template/AGENTS.md`.

Result: a new, guide-compliant template skill with both indexes in sync.

**Example 2: Update an existing skill and resync the index**

User says: "Tighten the `bug-fix` skill's triggers and update the skill list."

Actions:
1. Edit `template/.claude/skills/bug-fix/SKILL.md`.
2. Refine against `meta/skill-guide.md`.
3. Regenerate the `bug-fix` row in `meta/skill-list.md` from its new description.

Result: the skill and its index entry agree.

## Troubleshooting

**Question: Should this skill go in the template or the authoring repo?**

If downstream projects use it, it belongs under `template/.claude/skills/`. If it
only maintains this authoring repo, it belongs under the root `.claude/skills/`
and is out of scope for this skill.

**Question: The skill list and the directory disagree.**

The directory is the source of truth. Regenerate `meta/skill-list.md` from the
actual `template/.claude/skills/*/SKILL.md` files; never invent rows.

**Question: Create a new skill or extend an existing one?**

Extend when the guidance fits an existing scope. Create only when the scope is
distinct enough to warrant its own trigger phrases.

## Guardrails

- Do not author template skills outside `template/.claude/skills/`.
- Do not skip reading `meta/skill-guide.md` before writing.
- Do not skip the subagent refinement pass.
- Do not leave `meta/skill-list.md` out of sync with the skill directory.
- Prefer updating an existing skill over creating a duplicate.

## See Also

- [meta/skill-guide.md](../../../meta/skill-guide.md) — the design rubric.
- [meta/skill-list.md](../../../meta/skill-list.md) — the index this skill maintains.
- [template/.claude/skills/](../../../template/.claude/skills/) — the template skills.
- [template/AGENTS.md](../../../template/AGENTS.md) — the in-template skill map.
- [template/.claude/agents/agents-md-sync.md](../../../template/.claude/agents/agents-md-sync.md) — agent that reconciles `template/AGENTS.md` with the file tree.
