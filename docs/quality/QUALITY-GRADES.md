# Quality Grades

> This document grades the current quality of each domain and architectural layer.
> Grades are updated by the `doc-gardening` skill on a regular cadence.
> Use this to know what areas need attention before modifying them.

## Grading Scale

| Grade | Meaning                                                        |
|-------|----------------------------------------------------------------|
| A     | Well-tested, clean, documented. Safe for agent modification.   |
| B     | Mostly solid. Minor gaps in tests or docs. Proceed with care.  |
| C     | Known issues. Read domain-specific notes before touching.       |
| D     | Significant debt. Plan extra time for refactoring alongside work.|
| F     | Broken or heavily degraded. Fix before adding features.         |

## Current Grades

| Domain / Area         | Grade | Test Coverage | Doc Status  | Known Issues          | Last Reviewed |
|-----------------------|-------|---------------|-------------|-----------------------|---------------|
| `[domain-1]`         | [?]   | [?]%          | [?]         | [brief notes]         | [DATE]        |
| `[domain-2]`         | [?]   | [?]%          | [?]         | [brief notes]         | [DATE]        |
| `shared`             | [?]   | [?]%          | [?]         | [brief notes]         | [DATE]        |
| Infrastructure / CI  | [?]   | N/A           | [?]         | [brief notes]         | [DATE]        |

## How to Improve a Grade

- **D→C**: Add missing tests for the critical path. Fix the most impactful known issue.
- **C→B**: Reach 70%+ coverage on business logic. Update stale documentation.
- **B→A**: Full happy-path + edge-case coverage. Architecture rules all passing. Docs current.

## Agent Behavior by Grade

- **Grade A–B**: Proceed normally. Standard workflow.
- **Grade C**: Read known issues first. Run extra tests after changes. Consider a small refactor.
- **Grade D–F**: Before feature work, run the `refactor` skill to stabilize.
  If the task is urgent, document new debt incurred and open a follow-up task.

## Updating Grades

Grades are updated when:
1. A significant refactor or feature lands.
2. The `doc-gardening` skill runs its periodic scan.
3. A human engineer reviews and reassesses.

To update: modify the table above and commit with message `docs: update quality grades [domain]`.
