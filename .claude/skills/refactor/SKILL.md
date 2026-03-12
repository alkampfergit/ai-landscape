# Skill: Refactor

> Use this skill for code improvement without behavior change.
> The cardinal rule: ALL existing tests must pass before AND after.
> No behavior change means no test changes (except adding new tests).

## Prerequisites

Load:
- `docs/quality/QUALITY-GRADES.md` — know the current state.
- `docs/design/PATTERNS.md` — know what "good" looks like.

## Workflow

### Step 1: Define the Refactoring Goal

Clearly state what you're improving and why:
- Reducing duplication?
- Extracting a shared utility?
- Splitting a file that exceeds 300 lines?
- Aligning code with a pattern from PATTERNS.md?
- Removing a deprecated pattern?

**Scope boundary**: Define what you will and won't touch. Refactoring has
a tendency to expand. Set a boundary and stick to it.

### Step 2: Verify the Safety Net

1. Run all existing tests. They must ALL pass.
2. If test coverage is low in the affected area, write characterization tests
   FIRST — tests that capture current behavior, even if that behavior is imperfect.
3. Record the test results (count of passing tests).

### Step 3: Refactor in Small Steps

Each step must:
1. Be a single, coherent transformation (rename, extract, move, inline).
2. Leave all tests passing after each step.
3. Be committable independently.

**Order of operations for common refactors**:

**Extracting a shared utility**:
1. Identify all instances of the duplicated pattern.
2. Create the utility function in the `shared` package.
3. Add tests for the utility.
4. Replace each instance one at a time, running tests after each.

**Splitting a large file**:
1. Identify the responsibility boundaries within the file.
2. Create new files for each responsibility.
3. Move functions/classes one at a time.
4. Update imports throughout the codebase.
5. Run tests after each move.

**Aligning with a new pattern**:
1. Implement the new pattern alongside the old one.
2. Migrate callers one at a time.
3. Remove the old implementation when no callers remain.

### Step 4: Verify Preservation

1. Run all tests. The same tests that passed before must still pass.
2. Compare test count — it should be equal or higher, never lower.
3. Run linter and structural tests.
4. If any test changed, justify why in the PR description.

### Step 5: Update Quality Grade

If the refactor significantly improved a domain's quality:
1. Update the grade in QUALITY-GRADES.md.
2. Note what was improved.

### Step 6: Submit

1. PR title: `refactor: [brief description of improvement]`
2. PR description:
   - What was refactored and why.
   - Confirmation that no behavior changed.
   - Updated quality grade (if applicable).
