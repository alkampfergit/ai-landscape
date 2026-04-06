# Skill: ingest-link

## Purpose

Ingest an external URL as a knowledge source: fetch and analyse its content, record a brief
entry in `knowledge/links.md`, write detailed notes to `knowledge/sources/<slug>.md`, identify
which template documents need updating based on the new insights, patch those documents, and
update the template structure if the changes affect the overall layout.

## When to Use

Use this skill whenever a new external source (article, paper, blog post, documentation page,
talk transcript, etc.) should inform the harness engineering template.

Trigger: user provides a URL, e.g.:
- `ingest-link https://example.com/article`
- `/ingest-link <url>`
- "add this link to the knowledge base: <url>"

---

## Inputs

| Input | Description |
|-------|-------------|
| `url` | The URL to ingest (required) |

---

## Workflow

Work through each phase in order. Do not skip phases. Mark each phase complete before moving on.

---

### Phase 1 — Fetch and Analyse the Source

1. Use `WebFetch` to retrieve the full content of `url`.
2. If the fetch fails, report the error to the user and stop.
3. Read the content and extract:
   - **Title**: the page/article title
   - **Slug**: a short lowercase dash-separated identifier derived from the title (e.g. `my-article-title`)
   - **Category**: classify as one of `methodology | pattern | tool | case-study | reference`
   - **Description**: 1–2 sentences summarising the source's main argument or content (for the index)
   - **Summary**: a single paragraph synthesising the source's main argument or content (for the detail file)
   - **Key insights**: a bullet list of points that are directly actionable for harness engineering
     (agent workflows, enforcement mechanisms, documentation patterns, skill design, CI/CD practices, etc.)
   - **Relevance verdict**: which areas of the template this source is most relevant to
     (architecture docs, design principles, patterns, quality standards, workflows, skills, AGENTS.md, etc.)

---

### Phase 2 — Write Detail File (knowledge/sources/<slug>.md)

1. Determine the detail file path: `knowledge/sources/<slug>.md`.
2. Check whether the file already exists.
   - If it exists: update it in place with refreshed content, preserve `Added`, and set `Updated` to today's date.
   - If it does not exist: create it with `Added` set to today's date and omit `Updated` until a later refresh.
3. Format the detail file as:

```markdown
# <title>

- **URL**: <url>
- **Added**: <YYYY-MM-DD> <!-- original creation date -->
- **Updated**: <YYYY-MM-DD> <!-- optional; last refresh date, only present after the first update -->
- **Category**: <category>

## Summary

<summary paragraph>

## Key Insights

- <insight 1>
- <insight 2>
- …

## Template Documents Updated

- <file> — <one-line reason>
- …
(fill in after Phase 5; write "none" if no changes are warranted)
```

4. Write the file to `knowledge/sources/<slug>.md`.

---

### Phase 3 — Record in knowledge/links.md

1. Read `knowledge/links.md`.
2. Check whether an entry for `url` already exists (compare URLs exactly).
   - If it exists: update the existing entry in place with refreshed content. Note the update date.
   - If it does not exist: append a new entry after the `<!-- ingest-link appends entries below this line -->` marker.
3. Format the index entry as:

```markdown
### [<title>](<url>)
- **Added**: <YYYY-MM-DD>
- **Category**: <category>
- **Description**: <description (1–2 sentences)>
- **Details**: [Full notes](sources/<slug>.md)
```

4. Write the updated `knowledge/links.md`.

---

### Phase 4 — Identify Template Documents to Update

1. Read `template/AGENTS.md` to understand the current template structure.
2. Based on the **Relevance verdict** from Phase 1, identify the specific files under `template/`
   that the new insights apply to. Consult this mapping:

| Relevance area             | Files to check |
|----------------------------|----------------|
| Architecture / boundaries  | `template/docs/architecture/ARCHITECTURE.md`, `template/docs/architecture/DEPENDENCY-RULES.md`, `template/docs/architecture/DOMAIN-BOUNDARIES.md` |
| Design philosophy          | `template/docs/design/DESIGN-PRINCIPLES.md`, `template/docs/design/PATTERNS.md` |
| Quality / standards        | `template/docs/quality/CODE-STANDARDS.md`, `template/docs/quality/QUALITY-GRADES.md` |
| Agent workflows / lifecycle| `template/docs/workflows/TASK-LIFECYCLE.md`, `template/docs/workflows/REVIEW-CHECKLIST.md` |
| Terminology / decisions    | `template/docs/context/GLOSSARY.md`, `template/docs/context/DECISIONS.md` |
| Skill design               | `template/.claude/skills/*/SKILL.md` |
| Agent entry point / map    | `template/AGENTS.md` |
| Output templates           | `template/templates/` |
| Bootstrap prompt           | `template/BOOTSTRAP.md` |

3. Read each candidate file.
4. For each file, determine: **does this source provide new or contradicting information that
   should update this document?**
   - New best practice not yet mentioned → add it
   - Pattern or anti-pattern newly identified → add to PATTERNS.md
   - New term → add to GLOSSARY.md
   - Decision implied by the source → add to DECISIONS.md as an ADR entry
   - Existing guidance that the source contradicts or supersedes → update it and note the change
   - No actionable delta → skip the file

5. Produce a **patch plan**: a list of `(file, change description)` pairs for files that need updating.
   If the patch plan is empty, set `Template Documents Updated: none` in the detail file and stop here.
   The `knowledge/links.md` entry remains unchanged apart from its normal Description/Details content.

---

### Phase 5 — Patch Template Documents

For each `(file, change)` pair in the patch plan:

1. Read the file if not already loaded.
2. Apply the minimal change that incorporates the insight:
   - Prefer adding a new bullet, row, or paragraph over rewriting existing prose.
   - Preserve the document's existing structure, tone, and formatting conventions.
   - Do not add content unrelated to the insight being incorporated.
   - Do not remove existing content unless it directly conflicts with the new insight.
3. If a new ADR is warranted, append it to `template/docs/context/DECISIONS.md` using the
   format already present in that file.
4. Write the updated file.

After all patches are applied, re-read each modified file and verify:
- Internal links still resolve (no broken `[text](path)` references)
- No duplicate content was introduced
- The progressive-disclosure principle is preserved (AGENTS.md stays as the short map;
  detail lives in the linked docs)

---

### Phase 6 — Update Template Structure if Needed

Check whether the patches from Phase 5 created structural changes that require updating the
template's navigation layer:

1. **New files added** under `template/`: add entries to `template/AGENTS.md`'s "Where to Look"
   table and to `template/README.md`'s structure diagram.
2. **Files renamed or moved** under `template/`: update all cross-references in
   `template/AGENTS.md`, `template/README.md`, and any docs that linked to the old path.
3. **New skills added** under `template/.claude/skills/`: add a row to the Skills table in
   `template/AGENTS.md`.
4. **Sections added to existing docs**: no structural update needed unless the section is
   significant enough to warrant its own "Where to Look" entry.

If no structural changes occurred, skip this phase.

---

### Phase 7 — Update Detail File and Index Entry

Go back to the detail file written in Phase 2 and fill in the final value of
`Template Documents Updated` with the actual list of files changed (relative paths from repo root).

If the index entry in `knowledge/links.md` needs any correction, update it now.

---

### Phase 8 — Summary Report

Output a concise summary to the user:

```
## ingest-link complete

**Source**: <title> (<url>)
**Category**: <category>

**Insights extracted**: <N>

**Detail file**: knowledge/sources/<slug>.md

**Template documents updated**:
- <file> — <one-line reason>
- …
(or "none" if no changes were warranted)

**Structural changes**: <yes/no — brief description if yes>
```

---

## Guardrails

- **Never fabricate content.** Only incorporate insights that are clearly supported by the
  fetched source. If the source is behind a paywall or returns no useful content, report it
  and stop at Phase 1.
- **Stay general-purpose.** The template must remain applicable to any software project.
  Do not add project-specific, company-specific, or technology-specific content.
- **Minimal footprint.** Each patch should be the smallest change that captures the insight.
  Avoid rewriting whole sections unless the source fundamentally contradicts existing guidance.
- **Preserve voice.** Match the existing tone and formatting of each file you patch.
- **One source at a time.** This skill processes one URL per invocation. For multiple URLs,
  invoke the skill separately for each.
