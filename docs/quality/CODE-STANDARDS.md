# Code Standards

> These standards apply to ALL code in the repository.
> Most are enforced by formatters and linters. The rest are enforced by CI structural tests.

## File Organization

- **One concept per file**. A file named `order_service.ts` contains the OrderService and nothing else.
- **Max file length**: 300 lines. If a file exceeds this, split by responsibility.
- **Max function length**: 30 lines. Extract helper functions or decompose the logic.
- **Naming convention**: Files use `snake_case`. Types/classes use `PascalCase`. Functions/variables use `camelCase` (or language-appropriate equivalent).

## Import Ordering

Group imports in this order, separated by blank lines:
1. Standard library / runtime imports
2. External dependencies (third-party packages)
3. Internal shared packages
4. Same-domain imports (relative)

## Error Handling

- Use result types for expected business failures (validation errors, not-found, conflicts).
- Use exceptions/panics only for unexpected programmer errors (null reference, index out of bounds).
- Never silently swallow errors. At minimum, log them with structured context.
- Always include enough context in error messages for an agent to diagnose the issue.

## Testing Standards

- **Unit tests**: Every public function in the Service and Repository layers.
- **Integration tests**: Every API endpoint and event handler.
- **Structural tests**: Verify dependency rules, module boundaries, and naming conventions.
- **Test naming**: `[unit-under-test]_[scenario]_[expected-result]`
  - Example: `orderService_whenInvalidQuantity_returnsValidationError`

## Documentation in Code

- Public APIs: JSDoc/XML doc comment with parameter descriptions and return type.
- Complex logic: A brief comment explaining "why," not "what."
- No commented-out code. Delete it. Git has history.
- No TODO comments without an associated issue/task ID.

## Commit Messages

Format: `[type]: [brief description]`

Types:
- `feat`: New feature or capability
- `fix`: Bug fix
- `refactor`: Code improvement without behavior change
- `docs`: Documentation only
- `test`: Adding or fixing tests
- `chore`: Build, CI, tooling changes

Example: `feat: add order validation at API boundary`

## Language-Specific Rules

*(Add sections below as the project adopts specific technologies)*

### [Language/Framework]

- [Specific conventions for this language]
- [Linter configuration reference]
- [Formatter command]
