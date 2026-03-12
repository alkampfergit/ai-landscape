# Task Lifecycle

> Every task follows this lifecycle. No shortcuts.
> The phases exist to catch errors early and reduce rework.

## Overview

```
Human Prompt → Understand → Plan → Implement → Validate → Review → Merge
```

## Phase 1: Understand

**Goal**: Ensure the task is clear before writing any code.

Actions:
1. Read the task description / prompt carefully.
2. Identify which domain(s) and layer(s) are affected.
3. Load the relevant architecture and design docs for those areas.
4. Check the quality grade of the affected area.
5. Identify any ambiguities. If the task is unclear, ask — don't guess.

**Output**: A mental model of what needs to change and where.

## Phase 2: Plan

**Goal**: Define the approach before implementation.

Actions:
1. List the files that will be created or modified.
2. Identify if any new dependencies (packages, cross-domain imports) are needed.
3. Check if the change requires a new ADR (architectural decision).
4. If the task is complex (touches 3+ files or crosses domain boundaries),
   write a brief plan as a comment or in a `PLAN.md` file.

**Output**: A concrete plan that can be verified before code is written.

## Phase 3: Implement

**Goal**: Write the code following all standards and patterns.

Actions:
1. Follow the patterns in [PATTERNS.md](../design/PATTERNS.md).
2. Respect all dependency rules in [DEPENDENCY-RULES.md](../architecture/DEPENDENCY-RULES.md).
3. Write tests alongside the implementation — not after.
4. Keep changes focused. One task = one logical change set.
5. If you discover additional work needed, note it as a follow-up task.
   Do not scope-creep.

**Output**: Working code with tests.

## Phase 4: Validate

**Goal**: Ensure the change is correct and doesn't break anything.

Actions:
1. Run ALL tests (unit, integration, structural).
2. Run the linter and formatter.
3. Verify no dependency rule violations.
4. If the change affects an API, verify the contract hasn't broken.
5. If the change affects events, verify producers and consumers are aligned.

**Output**: All checks green. No warnings.

## Phase 5: Review

**Goal**: Catch issues that automated checks can't.

Actions (agent self-review):
1. Re-read the original task. Does the implementation actually solve it?
2. Check for unnecessary complexity. Could this be simpler?
3. Check for missing edge cases.
4. Verify documentation is updated if behavior changed.
5. Run the [REVIEW-CHECKLIST.md](REVIEW-CHECKLIST.md).

**Output**: A PR ready for human review (if required) or auto-merge.

## Phase 6: Merge

**Goal**: Clean integration.

Actions:
1. Ensure the PR has a clear title and description.
2. Link to the original task/issue.
3. Squash commits if there are fixup commits.
4. Merge only when all CI gates are green.

## When Things Go Wrong

If a phase fails:
- **Don't retry blindly**. Diagnose WHY it failed.
- **Check if context is missing**. Load additional docs if needed.
- **Check if a constraint is unclear**. If so, clarify it in the relevant doc.
- **If stuck**: Flag for human escalation. Describe what was attempted and what failed.
