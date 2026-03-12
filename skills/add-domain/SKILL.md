# Skill: Add Domain

> Use this skill when adding a new business domain (bounded context) to the project.
> This creates the full directory structure, layer scaffolding, and documentation entries.

## Prerequisites

Load:
- `docs/architecture/ARCHITECTURE.md` — understand existing domains.
- `docs/architecture/DOMAIN-BOUNDARIES.md` — understand current boundaries.
- `docs/context/GLOSSARY.md` — check for naming conflicts.

## Before You Start

Answer these questions:
1. **Domain name**: What is this domain called? (Use a single noun or short noun phrase.)
2. **Responsibility**: What business rules does this domain own?
3. **Bounded context**: What concepts live exclusively in this domain?
4. **Events**: What domain events will this domain publish/consume?
5. **Dependencies**: Which existing domains (if any) does this domain interact with?

If any answer is unclear, ask for clarification.

## Workflow

### Step 1: Write the ADR

Create an ADR in `docs/context/DECISIONS.md` documenting:
- Why this domain is needed.
- What it owns that no other domain owns.
- How it will interact with existing domains.

### Step 2: Scaffold the Directory Structure

Create the following structure:

```
src/domains/[domain-name]/
├── types/           # Value objects, DTOs, events, enums
│   └── index.ts     # Public type exports
├── config/          # Configuration schemas and defaults
│   └── index.ts
├── repository/      # Data access interfaces and implementations
│   └── index.ts
├── service/         # Business logic
│   └── index.ts
├── runtime/         # DI wiring, bootstrap
│   └── index.ts
└── README.md        # Domain-specific documentation
```

Adapt the file extensions and structure to the project's actual tech stack.

### Step 3: Create the Domain README

In the domain's `README.md`, include:
- Domain name and responsibility (from your answers above).
- Layer structure overview.
- Public API surface (even if empty initially).
- Events published/consumed.

### Step 4: Register in Architecture Docs

1. Add the domain to the domain table in `docs/architecture/ARCHITECTURE.md`.
2. Add the domain boundary definition in `docs/architecture/DOMAIN-BOUNDARIES.md`.
3. Add new integration contracts if this domain communicates with others.
4. Add domain-specific terms to `docs/context/GLOSSARY.md`.

### Step 5: Add to Quality Grades

Add a row in `docs/quality/QUALITY-GRADES.md` with initial grade (typically B
for a clean scaffold with basic tests).

### Step 6: Create Structural Tests

Add tests that verify:
- The domain's internal layers follow the dependency direction rule.
- No other domain imports this domain's internal types.
- Only types listed as "public" in `types/index` are accessible from outside.

### Step 7: Validate

1. All existing tests still pass.
2. New structural tests pass.
3. Linter is clean.
4. Architecture docs are consistent with the new domain.

### Step 8: Submit

1. PR title: `feat: add [domain-name] domain scaffold`
2. PR description:
   - Link to the ADR.
   - Summary of the domain's purpose.
   - Any follow-up feature work planned.
