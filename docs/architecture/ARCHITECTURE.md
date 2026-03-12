# Architecture Map

> This document provides the top-level view of the system's architecture.
> Read this first when working on any structural change.

## System Overview

```
[PROJECT_NAME] follows a layered domain architecture.
Each business domain is a self-contained vertical slice with internal layers.
Cross-domain communication happens exclusively through well-defined interfaces.
```

## Domain Map

List every business domain here. Each domain is a bounded context with clear ownership.

| Domain           | Description                              | Status    | Quality Grade |
|------------------|------------------------------------------|-----------|---------------|
| `[domain-1]`    | [Brief description of responsibility]     | Active    | See QUALITY   |
| `[domain-2]`    | [Brief description of responsibility]     | Active    | See QUALITY   |
| `[shared]`      | Cross-cutting utilities and shared types  | Active    | See QUALITY   |

## Layer Structure (per domain)

Each domain is internally divided into layers with strict dependency direction:

```
Types → Config → Repository → Service → Runtime → UI/API
  ←  dependencies flow LEFT to RIGHT only  →
```

- **Types**: Value objects, domain events, enums, DTOs. Zero dependencies on other layers.
- **Config**: Configuration schemas and defaults. Depends only on Types.
- **Repository**: Data access and persistence. Depends on Types and Config.
- **Service**: Business logic and orchestration. Depends on Types, Config, Repository.
- **Runtime**: Application bootstrap, DI wiring, middleware. Depends on all inner layers.
- **UI/API**: Controllers, handlers, presentation. Depends on Service and Types.

See [DEPENDENCY-RULES.md](DEPENDENCY-RULES.md) for enforcement details.

## Cross-Domain Communication

Domains communicate through:
1. **Published events** — Domain events emitted to a shared bus/stream.
2. **Shared contracts** — Interfaces and DTOs defined in the `shared` package.
3. **Direct service calls** — Only when explicitly allowed in the domain boundary map.

Anti-patterns:
- ❌ Domain A importing Domain B's internal types.
- ❌ Direct database queries across domain boundaries.
- ❌ Circular dependencies between domains.

## Infrastructure

| Component         | Technology           | Notes                              |
|-------------------|---------------------|------------------------------------|
| Primary database  | [e.g., PostgreSQL]  | [connection/access notes]          |
| Event store       | [e.g., EventStoreDB]| [if using event sourcing]          |
| Cache             | [e.g., Redis]       |                                    |
| Message bus       | [e.g., RabbitMQ]    |                                    |
| CI/CD             | [e.g., GitHub Actions]|                                   |
| Observability     | [e.g., OpenTelemetry + Grafana] |                         |

## Key Architectural Decisions

For the full list, see [DECISIONS.md](../context/DECISIONS.md).

Most impactful decisions:
1. [ADR-001]: [Title — e.g., "Use event sourcing for domain X"]
2. [ADR-002]: [Title — e.g., "Strict layer enforcement via custom linters"]
3. [ADR-003]: [Title — e.g., "Repository-as-source-of-truth policy"]
