# Domain Glossary

> This glossary defines terms used across the codebase.
> When you encounter an unfamiliar term in a task description or code,
> check here first. If the term is not listed, ask for clarification
> and then add it.

## How to Use This Glossary

- Terms are grouped by domain.
- Each term has a **definition** and an **in-code representation** (where applicable).
- When naming variables, classes, or functions, use the glossary term exactly.
  Consistent naming prevents confusion across domains.

## Universal Terms

| Term                | Definition                                                        | In Code           |
|---------------------|-------------------------------------------------------------------|--------------------|
| Domain              | A bounded context with clear ownership of specific business rules | Folder in `src/domains/` |
| Aggregate           | A cluster of domain objects treated as a single unit for changes  | Class with ID      |
| Domain Event        | A record that something meaningful happened in a domain           | Event class/type   |
| Value Object        | An immutable object defined by its attributes, not identity       | Immutable type     |
| Boundary            | The interface between two domains or between a domain and external world | API handler, event consumer |

## [Domain 1] Terms

| Term                | Definition                                                        | In Code           |
|---------------------|-------------------------------------------------------------------|--------------------|
| [Term]              | [Definition]                                                      | [Class/type name]  |

## [Domain 2] Terms

| Term                | Definition                                                        | In Code           |
|---------------------|-------------------------------------------------------------------|--------------------|
| [Term]              | [Definition]                                                      | [Class/type name]  |

## Adding New Terms

When a new concept is introduced:
1. Add it to the appropriate domain section.
2. Use the exact term in code (class names, variable names).
3. If the term is used across domains, add it to "Universal Terms."
4. Commit with message `docs: add glossary term [term]`.
