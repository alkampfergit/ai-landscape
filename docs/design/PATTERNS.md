# Patterns & Anti-Patterns

> When implementing a feature, consult this catalog first.
> Use the preferred pattern. If you find yourself reaching for an anti-pattern,
> stop and reconsider the approach.

## Preferred Patterns

### Data Validation at Boundaries

**Use**: Schema validation (e.g., Zod, FluentValidation, Pydantic) at every ingress point.

```
// GOOD: Parse and validate at the boundary
const input = RequestSchema.parse(rawBody);
orderService.process(input);  // input is now typed and trusted

// BAD: Pass raw data through
orderService.process(req.body);  // what shape is this? unknown
```

**Why**: Eliminates type confusion deep in business logic.
See: Design Principle P3.

### Result Types Over Exceptions for Expected Failures

**Use**: Return typed result objects for operations that can fail in expected ways.

```
// GOOD: Caller must handle both cases
Result<Order, ValidationError> result = orderService.Create(command);

// BAD: Exception for expected business rule violation
throw new OrderValidationException("...");
```

**Why**: Makes failure paths explicit and forces callers to handle them.

### Shared Utilities Over Hand-Rolled Helpers

**Use**: Place cross-cutting helper functions in the `shared` package.
Check if a utility exists before writing a new one.

**Why**: Centralizes invariants. Prevents drift between multiple
implementations of the same logic.

### Constructor Injection for Dependencies

**Use**: All dependencies injected through constructor. No service locator.

**Why**: Makes dependencies explicit, testable, and inspectable.
See: Design Principle P7.

### Structured Logging with Correlation

**Use**: Every log entry includes a correlation ID, domain context, and structured fields.

```
logger.info("Order processed", { orderId, customerId, duration, domain: "orders" });
```

**Why**: Enables agents to use observability data for debugging.

## Anti-Patterns — Do NOT Use

### ❌ God Objects

A single class/module that knows about everything. Split by responsibility.

### ❌ Stringly-Typed APIs

Passing magic strings where enums or typed constants should be used.

### ❌ Hidden Side Effects

Functions that modify state, call external services, or write to disk
without this being obvious from their signature.

### ❌ Deep Inheritance Hierarchies

Prefer composition. Max inheritance depth: 2 levels (base + one override).

### ❌ Raw SQL / Raw Queries in Business Logic

Use repository abstractions. The service layer must not know about query syntax.

### ❌ Ambient Configuration

Reading environment variables or config files deep inside business logic.
All config is loaded at the Runtime layer and injected inward.

## When You Encounter a New Pattern

If you find a useful pattern not listed here:
1. Implement it in the current task.
2. Add it to this file with a clear example and rationale.
3. If it should be enforced, write a linter rule (see DEPENDENCY-RULES.md for format).
