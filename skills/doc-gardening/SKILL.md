# Skill: Doc Gardening

> Use this skill to scan the repository for stale, incorrect, or missing documentation.
> Run this periodically (weekly recommended) or after major changes.
> This is the "garbage collection" for the knowledge base.

## What Gets Checked

### 1. AGENTS.md Freshness

- [ ] All links in AGENTS.md point to files that exist.
- [ ] The skill table matches the actual contents of `skills/`.
- [ ] The "Where to Look" table matches the actual `docs/` structure.
- [ ] The "Last verified" date is within the last 2 weeks.

### 2. Architecture Docs Accuracy

- [ ] The domain table in ARCHITECTURE.md matches actual `src/domains/` directories.
- [ ] Infrastructure table reflects current tech stack.
- [ ] DEPENDENCY-RULES.md matches what the linter actually enforces.
- [ ] DOMAIN-BOUNDARIES.md lists all cross-domain contracts that exist in code.

### 3. Quality Grades Currency

- [ ] Every domain in the codebase has a row in QUALITY-GRADES.md.
- [ ] No domain has a "Last Reviewed" date older than 1 month.
- [ ] Test coverage percentages match actual CI reports.

### 4. Glossary Completeness

- [ ] Every domain-specific type name in the codebase appears in GLOSSARY.md.
- [ ] No glossary terms refer to types/concepts that no longer exist.

### 5. ADR Completeness

- [ ] Every significant structural pattern in the code has a corresponding ADR.
- [ ] No ADRs reference technologies or patterns that have been removed.
- [ ] Superseded ADRs are marked as such with a pointer to the replacement.

### 6. Code Comment Hygiene

- [ ] No TODO comments without issue IDs.
- [ ] No references to removed files or renamed modules in comments.

## Workflow

### Step 1: Scan

Run through each checklist section above. For each item:
- **Pass**: Mark as verified.
- **Fail**: Log the specific issue.

### Step 2: Categorize Findings

Group issues by severity:
- **Critical**: Broken links, missing docs for active domains, incorrect rules.
- **Important**: Stale dates, outdated references, missing glossary entries.
- **Minor**: Formatting inconsistencies, cosmetic issues.

### Step 3: Fix

1. Fix all Critical issues in this pass.
2. Fix Important issues if time permits.
3. Log Minor issues as follow-up tasks.

### Step 4: Update Timestamps

After fixing, update the "Last verified" or "Last Reviewed" dates
in all touched documents.

### Step 5: Submit

1. PR title: `docs: doc gardening pass [DATE]`
2. PR description:
   - Summary of findings (counts by severity).
   - What was fixed.
   - Any follow-up tasks created for deferred fixes.

## Automation Hints

This skill can be partially automated:
- **Link checking**: Script that verifies all markdown links resolve to real files.
- **Domain sync**: Script that compares `src/domains/*/` with ARCHITECTURE.md rows.
- **Glossary sync**: Script that extracts exported type names and checks against GLOSSARY.md.
- **TODO audit**: `grep -rn "TODO" src/ | grep -v "TODO(#"` to find unlinked TODOs.

When you write these automation scripts, place them in `tools/doc-gardening/`.
