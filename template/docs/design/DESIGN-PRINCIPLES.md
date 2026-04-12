# Design Principles

> These principles are the "why" behind every architectural rule.
> When you face an ambiguous design choice, use these to decide.
> They are ordered by priority — when principles conflict, higher wins.

## P1: Correctness Over Cleverness

Prefer straightforward, boring implementations that are easy to verify.
The agent (and future agents) must be able to read, reason about, and safely
modify any piece of code. Clever solutions that only work because of non-obvious
invariants create fragile systems.

**Implication**: Choose the well-known pattern. If you're tempted to build
something novel, check if a simpler approach exists first.

## P2: Enforce Mechanically, Not Socially

Any rule that matters must be enforced by a tool — linter, test, CI gate, schema
validator. Rules that exist only in documentation are suggestions, not constraints.
Humans forget. Agents skip context. Tools never do.

**Implication**: Every time you add a new convention, ask: "How will this be
enforced automatically?" If the answer is "code review," that's insufficient.
When a failure reveals a missing constraint, encode the fix mechanically so the
same class of mistake cannot recur — don't just fix the symptom once and move on.
Each encoded correction raises the floor for all future agent runs.

## P3: Validate at Boundaries, Trust Within

All data entering a domain or crossing a layer boundary must be parsed into
typed shapes. Once inside a trusted boundary, code can assume valid types.
This eliminates an entire class of runtime errors.

**Implication**: Use schema validation libraries at every ingress point.
Never pass raw strings, untyped JSON, or dictionaries through business logic.

## P4: Repository as Source of Truth

Everything an agent needs to do its job must live in the repository:
architecture decisions, design rationale, domain glossary, quality assessments.
If knowledge lives in Slack threads, Google Docs, or someone's head, it
effectively does not exist.

**Implication**: When making a design decision in conversation, immediately
capture it as an ADR in `docs/context/DECISIONS.md`.

## P5: Progressive Disclosure — Organized by User Journeys

Don't load everything at once. Structure knowledge so agents (and humans)
can find what they need incrementally. Start with the map (AGENTS.md),
drill into architecture, then into domain-specific details.

The organizing principle behind progressive disclosure is the **user journey**:
a concrete path someone follows to accomplish a goal in the repository. Every
document earns its place by serving at least one identified journey. Documentation
that serves no journey is dead weight; a journey with missing documentation is a
trap that forces guesswork.

Typical journeys and the disclosure depth they require:

| Journey                        | Entry point     | Progressive depth                                      |
|--------------------------------|-----------------|--------------------------------------------------------|
| Orient to the codebase         | AGENTS.md       | → ARCHITECTURE.md → domain-specific docs               |
| Add a new feature              | AGENTS.md       | → skill `new-feature` → PATTERNS.md → domain docs      |
| Fix a bug                      | AGENTS.md       | → skill `bug-fix` → QUALITY-GRADES.md → domain docs    |
| Make an architectural decision | AGENTS.md       | → DECISIONS.md → DESIGN-PRINCIPLES.md → ARCHITECTURE.md|
| Onboard a new contributor      | README.md       | → AGENTS.md → GLOSSARY.md → TASK-LIFECYCLE.md          |
| Assess area health             | QUALITY-GRADES.md| → CODE-STANDARDS.md → domain-specific tests            |

Each row is a journey. Each arrow is a disclosure step — the reader loads the
next document only when the current one is insufficient. If a journey requires
more than 3–4 hops to reach actionable detail, the disclosure chain is too deep
and should be flattened.

**Implication**: Keep files focused. One topic per document. Link between
documents rather than duplicating information. When adding or reviewing a
document, identify which journey it serves and verify the disclosure chain
from entry point to that document is intact. Be mindful of the instruction
budget: frontier LLMs follow ~150–200 instructions reliably, and the agent's
system prompt already consumes ~50. Every instruction added to the top-level
agent file competes with task-specific context. Bundle detailed instructions
inside skill files so they load only when relevant.

## P6: Small, Composable Units

Prefer many small, single-purpose modules over large monolithic ones.
Each module should be understandable in isolation and testable independently.
This also makes parallel agent work possible without merge conflicts.

**Implication**: A single file should rarely exceed 300 lines. A single function
should rarely exceed 30 lines. If it does, split it.

## P7: Explicit Over Implicit

Make dependencies, configuration, and behavior explicit. Avoid magic strings,
ambient globals, dynamic dispatch without type information, and convention-based
wiring that requires deep framework knowledge.

**Implication**: Prefer dependency injection. Prefer explicit configuration over
convention. Prefer typed enums over string constants.

## P8: Fail Fast, Fail Loud

When something goes wrong, the system should report it immediately with enough
context to diagnose the problem. Silent failures compound into mysteries.

**Implication**: Use structured logging. Validate preconditions early. Prefer
exceptions or result types over returning null/undefined.

## P9: Iterate the Harness, Don't Pre-Optimize It

Build the harness incrementally. Start with the minimum configuration that lets the agent run the task. Add a new capability, hook, or enforcement rule only when the agent actually fails — and only to address the specific failure observed. Optimize for the speed of the design-test-iterate loop, not for the probability of one-shotting a task on the first attempt.

**Implication**: Resist installing MCP servers, skills, or rules "just in case." A harness shaped by real failures is leaner and more effective than one designed around anticipated failures. Battle-tested configurations, once validated, should be committed at the repository level so the whole team benefits automatically.
