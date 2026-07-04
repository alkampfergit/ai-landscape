# Knowledge Sources

This file is the **master index of external sources** that inform the harness engineering template.
Each entry contains the source URL, a brief description, and a link to the detailed notes.

> **Do not edit manually.**
> Use the `ingest-link` skill to add or update entries.
> Run `ingest-link <url>` to analyse a new source and propagate its insights into the template.

---

## Format Reference

Each entry follows this structure:

```
### [Title](URL)
- **Added**: YYYY-MM-DD
- **Category**: one of: methodology | pattern | tool | case-study | reference
- **Description**: 1–2 sentence summary of the source's key argument or content.
- **Details**: [Full notes](sources/<slug>.md)
```

---

## Entries

<!-- ingest-link appends entries below this line -->

### [User Journeys vs. User Flows](https://www.nngroup.com/articles/user-journeys-vs-user-flows/)
- **Added**: 2026-04-12
- **Category**: methodology
- **Summary**: Nielsen Norman Group defines user journeys as scenario-based sequences of steps a user takes to accomplish a high-level goal, usually across channels and over time, contextualized with thoughts, emotions, and pain points. User flows, by contrast, describe the specific discrete interactions within a product for a common task. Journeys are the holistic macro view (why and what the user experiences); flows are the micro view (how they interact step by step). Both are useful and complementary — journeys identify which documentation needs to exist and why, flows describe how documents link to each other.
- **Key insights**:
  - A user journey is the end-to-end experience over time — not just the steps taken, but the thoughts, emotions, pain points, and confidence shifts at each phase
  - User flows are the specific discrete interactions within a single phase — the micro-level complement to the macro-level journey
  - Journey mapping starts with a timeline of user goals and actions, then adds thoughts and emotions to create a narrative
  - Journeys answer "what is this person trying to accomplish and what do they experience?"; flows answer "what specific steps do they take?"
  - Both views should be captured: journeys tell you which documents need to exist; flows tell you how they link together
  - Journey phases can be made concrete and testable through user stories ("As a [role], I want [goal], so that [benefit]")
  - Documentation earns its place by serving identified journey phases — docs that no journey phase reaches for are candidates for pruning
  - Progressive disclosure is the mechanism that maps to journey phases: the right information surfaces at the right moment in the experience
- **Template documents updated**: `template/docs/design/DESIGN-PRINCIPLES.md`, `template/meta/harness-foundation.md`, `template/docs/context/GLOSSARY.md`, `template/AGENTS.md`, `template/.claude/skills/doc-gardening/SKILL.md`, `template/templates/user-story.template.md` (new)

### [Skill Issue: Harness Engineering for Coding Agents](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents)
- **Added**: 2026-04-05
- **Updated**: 2026-04-06
- **Category**: methodology
- **Description**: Argues that poor agent performance is almost always a harness problem, not a model problem. Documents what consistently backfires (over-engineering upfront, running full test suites after every session) versus what works (start simple, add harness only after real failures, distribute configs at repo level).
- **Details**: [Full notes](sources/skill-issue-harness-engineering.md)

### [Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)
- **Added**: 2026-04-05
- **Category**: methodology
- **Description**: Shows how harness-only changes (system prompt, context middleware, self-verification loops) improved a coding agent from 52.8% to 66.5% on Terminal Bench 2.0 with no model change. Introduces the "Reasoning Sandwich" pattern and trace-based improvement loop.
- **Details**: [Full notes](sources/improving-deep-agents.md)

### [Natural-Language Agent Harnesses](https://arxiv.org/html/2603.25723v1)
- **Added**: 2026-04-05
- **Category**: methodology
- **Description**: Proposes externalizing harness logic as structured natural-language files (NLAHs) with explicit contracts, roles, state semantics, and failure taxonomies, rather than burying it in code. Demonstrates improved reliability through durable file-backed state and structured failure recovery.
- **Details**: [Full notes](sources/natural-language-agent-harnesses.md)

### [Harness engineering: leveraging Codex in an agent-first world](https://openai.com/index/harness-engineering/)
- **Added**: 2026-04-05
- **Category**: case-study
- **Description**: OpenAI's Codex team shipped ~1M lines of code in 5 months with zero human-written lines, using harness engineering practices: a short AGENTS.md as a navigation map, machine-readable docs, mechanically-enforced invariants via CI, and encoding every failure-driven correction permanently into the harness.
- **Details**: [Full notes](sources/harness-engineering-codex.md)

### [Architecture Decision Records — Comprehensive Guide](https://www.perplexity.ai/search/search-info-about-adr-document-fCZ3UaxrS3qoEPY_gzh4_w)
- **Added**: 2026-04-05
- **Category**: methodology
- **Description**: Covers ADR formats (Nygard, MADR, Tyree-Akerman), governance, and tooling. Core discipline is preserving the "why" behind decisions so agents and developers can understand constraints rather than re-litigate them; records are immutable once accepted.
- **Details**: [Full notes](sources/architecture-decision-records.md)

### [Writing effective tools for AI agents—using AI agents](https://www.anthropic.com/engineering/writing-tools-for-agents)
- **Added**: 2026-04-06
- **Category**: reference
- **Description**: Anthropic's guide on designing tools for AI agents. Key practices: prompt-engineer tool descriptions as carefully as system prompts, return natural-language identifiers, implement pagination/truncation with sensible defaults, and use evaluation-driven development to iterate on tool specs.
- **Details**: [Full notes](sources/writing-tools-for-agents.md)
