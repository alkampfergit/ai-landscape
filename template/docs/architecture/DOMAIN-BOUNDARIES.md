# Domain Boundaries

> This document defines every bounded context, its ownership, and the
> contracts through which domains interact. Update this whenever a new
> domain is added or boundaries change.

## Boundary Map

```
┌─────────────┐     events      ┌─────────────┐
│  Domain A   │ ──────────────► │  Domain B   │
│             │                 │             │
│  [owner]    │ ◄────────────── │  [owner]    │
└─────────────┘   shared DTOs   └─────────────┘
       │                               │
       │         ┌─────────────┐       │
       └────────►│   Shared    │◄──────┘
                 │  Contracts  │
                 └─────────────┘
```

## Domain Registry

For each domain, define:

### [Domain Name]

- **Responsibility**: [One sentence describing what this domain owns]
- **Bounded context**: [What concepts live here and nowhere else]
- **Published events**: [List of domain events this domain emits]
- **Consumed events**: [List of events this domain subscribes to]
- **Public API surface**: [Endpoints or service interfaces exposed]
- **Data ownership**: [Which database tables/collections this domain owns exclusively]

### [Domain Name 2]

- **Responsibility**: ...
- **Bounded context**: ...
- *(same structure)*

## Integration Contracts

Every cross-domain interaction must be documented here:

| Source Domain | Target Domain | Mechanism       | Contract Location                |
|--------------|---------------|-----------------|----------------------------------|
| [A]          | [B]           | Domain Event    | `shared/events/order_placed.ts`  |
| [B]          | [A]           | Sync API Call   | `shared/contracts/pricing_api.ts`|

## Rules for Modifying Boundaries

1. **Adding a new domain**: Use the `add-domain` skill. It scaffolds the full layer structure.
2. **Splitting a domain**: Create an ADR first. Both old and new domains must work during transition.
3. **Merging domains**: Requires review. Update this document, the architecture map, and all affected contracts.
4. **Adding a cross-domain dependency**: Must go through shared contracts. Direct imports are forbidden.
