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

## Documentation & Decision Terms

| Term                          | Definition                                                                                          |
|-------------------------------|-----------------------------------------------------------------------------------------------------|
| ADR (Architecture Decision Record) | A document capturing a significant architectural decision: its context, the options considered, the decision made, the rationale, and the consequences. One decision per record. |
| ADL (Architecture Decision Log)    | The ordered collection of all ADRs for a project. In this template, the ADL lives in `docs/context/DECISIONS.md`. |
| ASR (Architecturally-Significant Requirement) | A requirement that has a measurable impact on the system's architecture — performance, security, scalability, or structural constraints that narrow architectural choices. |
| Fitness Function               | An objective, automated check that verifies the codebase continues to comply with a recorded architectural decision. Implemented as a structural test, linter rule, or CI gate. |
| Superseded ADR                 | An ADR whose decision has been replaced by a newer ADR. The original is kept immutable; its status is updated to "Superseded by ADR-NNN". |

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
