# Architecture Decision Records — Comprehensive Guide

- **URL**: https://www.perplexity.ai/search/search-info-about-adr-document-fCZ3UaxrS3qoEPY_gzh4_w
- **Added**: 2026-04-05
- **Canonical sources fetched**: https://github.com/joelparkerhenderson/architecture-decision-record (README)
- **Category**: methodology

## Summary

Architecture Decision Records (ADRs) are documents that capture significant architectural decisions along with their context, the options considered, the rationale for the choice made, and the resulting consequences. Collected together they form an Architecture Decision Log (ADL). Multiple formats exist — Nygard (simple), MADR (options-and-trade-offs focused), Tyree-Akerman (sophisticated), and others — each suited to different team contexts. The core discipline is preserving the "why" behind decisions so future developers and agents can understand constraints rather than re-litigate them. Records should be immutable once accepted; changes are captured as new superseding ADRs.

## Key Insights

- ADRs must capture **rationale** (the decisive pros/cons and reasoning), not just the decision — the "why" is more valuable than the "what"
- One decision per record; keep them specific and focused
- Records are **immutable** once accepted — amend by creating a new ADR that supersedes the old one; never rewrite history
- MADR format explicitly surfaces **options considered and trade-offs**, which is more useful for agents than a flat decision statement
- File naming convention: lowercase, dash-separated, present-tense imperative verb (e.g. `choose-database.md`)
- Status lifecycle: `Proposed → Accepted → Deprecated | Superseded by ADR-NNN`
- Schedule an **after-action review** ~1 month after acceptance to capture real consequences
- Governance: define who can propose, who accepts, when to skip — prevents ADR fatigue
- **Fitness functions** (ArchUnit, structural tests, linter rules) can mechanically enforce ADR decisions in code
- **Decision Guardian** tooling can surface relevant ADRs during PR review, embedding decision context in the workflow
- AI/LLM fitness functions can evaluate compliance with ADRs using extended-context reasoning

## Template Documents Updated

- `template/templates/adr.template.md` — added `Options Considered` section (MADR-style), renamed `Alternatives Considered` → `Options Considered`, added `Rationale` section separate from `Decision`, added immutability note, expanded `Context` guidance, added file naming convention
- `template/docs/context/DECISIONS.md` — added ADL concept, immutability rule, after-action review, governance section, skip criteria, status lifecycle diagram, updated inline template to reference standalone template
- `template/docs/context/GLOSSARY.md` — added Documentation & Decision Terms section with ADR, ADL, ASR, Fitness Function, Superseded ADR definitions
- `template/docs/workflows/TASK-LIFECYCLE.md` — expanded ADR trigger criteria in Phase 2 Plan with explicit write/skip conditions
