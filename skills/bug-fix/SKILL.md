# Skill: Bug Fix

> Use this skill when fixing a reported bug.
> The key discipline: REPRODUCE before you FIX.

## Prerequisites

Load:
- `docs/architecture/ARCHITECTURE.md` — locate the affected domain.
- `docs/quality/QUALITY-GRADES.md` — check if this area has known issues.

## Workflow

### Step 1: Understand the Bug Report

Extract from the report:
1. **Expected behavior**: What should happen?
2. **Actual behavior**: What happens instead?
3. **Reproduction steps**: How to trigger the bug?
4. **Context**: Which domain, which endpoint, which input?

If any of these are missing, ask for clarification before proceeding.

### Step 2: Reproduce

**Write a failing test FIRST.**

The test must:
- Exercise the exact scenario described in the bug report.
- Fail with the current code (proving the bug exists).
- Use a descriptive name: `[unit]_[bugScenario]_[expectedCorrectBehavior]`

If you cannot reproduce the bug with a test, investigate using observability
tools (logs, metrics, traces) if available. Do not guess at a fix without
reproducing the issue.

### Step 3: Locate the Root Cause

1. Trace the execution path from the reproduction test.
2. Identify the exact line or condition where behavior diverges from expectations.
3. Determine if this is a logic error, a missing validation, a data shape mismatch,
   or an integration issue.

**Important**: Fix the ROOT CAUSE, not the symptom. If the real problem is
a missing boundary validation and the symptom is a null reference, fix the
validation — don't just add a null check.

### Step 4: Implement the Fix

1. Make the minimal change that fixes the root cause.
2. Do NOT refactor unrelated code in the same change (use the `refactor` skill separately).
3. Verify the failing test now passes.
4. Check for similar patterns elsewhere — if this bug could occur in other
   places with the same pattern, note follow-up tasks.

### Step 5: Add Regression Coverage

Beyond the reproduction test, add:
- Tests for related edge cases the bug exposed.
- If the bug was caused by a missing constraint, add a linter rule or structural
  test to prevent recurrence (see DEPENDENCY-RULES.md for format).

### Step 6: Validate and Submit

1. All tests pass (existing + new).
2. Linter and CI are green.
3. Run through REVIEW-CHECKLIST.md.
4. PR title: `fix: [brief description of what was broken]`
5. PR description includes:
   - Root cause analysis (1-2 sentences).
   - What the fix does.
   - Any follow-up tasks identified.
