# Spec-Driven Development Process

> Draft proposal for adding a Spec-Driven Development (SDD) harness to the template.
> This document is authoring guidance for the `ai-landscape` repository. Once stable,
> the process can be translated into template docs and skills under `template/`.

## Purpose

Spec-Driven Development makes the specification the controlling artifact for feature
delivery. The agent should not jump directly from a user request to implementation.
Instead, the harness should force a sequence of artifacts and gates:

```text
request
  -> spec.md
  -> plan.md
  -> plan validation
  -> task list
  -> implementation
  -> acceptance verification
  -> finished feature or rework
```

The goal is not documentation ceremony. The goal is a repeatable path from intent to
finished feature where each phase leaves enough structure for the next phase to be
checked mechanically or reviewed quickly.

## Is "Constitution" Still Needed?

The concept is still valid, but the file name should not be mandatory.

In GitHub Spec Kit, the constitution represents durable project principles: rules the
agent must obey before writing specs, plans, tasks, or code. In this template, that role
is already moving toward a repository architecture model:

- `AGENTS.md` is the agent entry point and routing layer.
- `ARCHITECTURE.md` is the system map and architectural invariant index.
- Architecture detail docs describe boundaries, dependency rules, and domain contracts.
- Quality docs define code standards, test expectations, and review gates.
- ADRs record decisions that explain why durable constraints exist.

Because of that, this template probably does not need a separate `CONSTITUTION.md`.
Instead, SDD should define a **constitutional context phase**: before specification work
starts, the harness loads the relevant standing rules from `AGENTS.md`,
`ARCHITECTURE.md`, architecture docs, quality docs, and ADRs.

Use the word **constitution** for the role if useful, but do not require a standalone
constitution artifact unless a downstream project explicitly wants one.

Recommended naming in this template:

- Prefer `project context`, `standing rules`, or `architecture context` in user-facing docs.
- Use `constitutional context` internally when describing the SDD state machine.
- Avoid creating another top-level policy file if `ARCHITECTURE.md` and linked docs already
  contain the durable rules.

## Core Artifacts

An SDD cycle should produce these artifacts, either as durable files or task-local
working documents depending on project size:

| Artifact | Producer | Purpose |
|---|---|---|
| Context summary | `sdd-context` | Captures the relevant standing rules, architecture constraints, and quality expectations for this cycle. |
| `spec.md` | `sdd-spec-author` | Captures user stories, functional requirements, acceptance criteria, scope, non-goals, and constraints. |
| Spec review report | `sdd-spec-validator` | Confirms the spec is clear, testable, bounded, and aligned with project rules. |
| `plan.md` | `sdd-implementation-planner` | Maps the spec to architecture, files, risks, tests, and sequencing. |
| Plan review report | `sdd-plan-validator` | Confirms the plan is feasible, architecture-aligned, testable, and ready for task generation. |
| Task list | `sdd-task-breakdown` | Converts the plan into ordered executable tasks traceable to acceptance criteria. |
| Implementation notes | `sdd-feature-implementer` | Records changed files, completed tasks, and deviations from the plan or spec. |
| Verification report | `sdd-acceptance-verifier` | Checks tests and acceptance criteria, then declares done or routes to rework. |

The artifact chain matters. Each phase must consume the previous artifact and produce a
stricter input for the next phase. For durable artifacts, file names also matter:
`spec.md` and `plan.md` should keep the same names in both the working area and the final
`specs/` archive.

## Artifact Storage

Active SDD cycle artifacts should live under `.agents/sdd/`. This is the working area for
the current SDD run, not the long-term specification archive.

`.agents/sdd/` should contain temporary or in-progress artifacts such as:

- `spec.md` before approval
- context summaries for the current cycle
- spec review reports
- `plan.md` before approval
- plan review reports
- task lists
- implementation notes
- verification reports
- rework notes and routing decisions

Use a per-cycle subdirectory so concurrent or historical runs do not collide:

```text
.agents/sdd/
  <feature-id>/
    context.md
    spec.md
    spec-review.md
    plan.md
    plan-review.md
    tasks.md
    implementation-notes.md
    verification-report.md
```

When the SDD cycle finishes, only the durable spec package should be promoted into
`specs/`. The durable package should contain:

- `spec.md`: user stories, functional requirements, and acceptance criteria
- `plan.md`: the accepted implementation plan

The `specs/` folder is the permanent feature-contract archive. It should preserve what
the feature is expected to do and the approved implementation direction future work may
need to reference. Specs should remain in `specs/` after the SDD cycle completes unless
they are superseded by a later spec or deliberately retired by a documented change.

All other SDD artifacts are working artifacts and should remain under `.agents/sdd/`:
context summaries, draft specs, review reports, task lists, implementation notes,
verification reports, rework notes, and routing decisions.

Recommended durable layout:

```text
specs/
  <feature-id>/
    spec.md
    plan.md
```

The working artifact names should match the durable artifact names where possible. This
allows completion to promote files from `.agents/sdd/<feature-id>/` into
`specs/<feature-id>/` without rewriting their structure.

ADRs remain separate harness-level artifacts and should stay in the repository's
architecture decision location rather than under `.agents/sdd/` or `specs/`.

## Artifact Templates

Each SDD phase should have a matching artifact template. Templates are not just formatting
helpers; they are part of the prompt engineering for the harness. A phase template tells
the agent what information must exist before the phase can be considered complete.

Templates should serve four purposes:

- **Consistency:** every SDD cycle produces artifacts with the same shape.
- **Completeness:** required sections force agents to cover scope, risks, tests,
  assumptions, and acceptance criteria instead of omitting them when context is thin.
- **Traceability:** stable IDs let later artifacts reference earlier requirements,
  decisions, tasks, and verification results.
- **Context control:** subagents receive only the template, the compact input packet, and
  the relevant prior artifacts, then return a structured artifact.

Recommended template set:

| Template | Used by | Output |
|---|---|---|
| `templates/sdd/context.template.md` | `sdd-context` | `context.md` |
| `templates/sdd/spec.template.md` | `sdd-spec-author` | `spec.md` |
| `templates/sdd/spec-review.template.md` | `sdd-spec-validator` | `spec-review.md` |
| `templates/sdd/plan.template.md` | `sdd-implementation-planner` | `plan.md` |
| `templates/sdd/plan-review.template.md` | `sdd-plan-validator` | `plan-review.md` |
| `templates/sdd/tasks.template.md` | `sdd-task-breakdown` | `tasks.md` |
| `templates/sdd/implementation-notes.template.md` | `sdd-feature-implementer` | `implementation-notes.md` |
| `templates/sdd/verification-report.template.md` | `sdd-acceptance-verifier` | `verification-report.md` |

SDD templates should live under `templates/sdd/`. Keeping them in a dedicated subdirectory
separates SDD prompt artifacts from general harness templates such as ADRs, PR
descriptions, commits, and user stories.

### Template Requirements

All SDD templates should include:

- `Status`: draft, approved, rejected, implemented, verified, or rework-required.
- `Source`: link or reference to the prior artifact that this artifact consumes.
- `Scope`: what this artifact covers and what it deliberately excludes.
- `Assumptions`: decisions made because information was missing.
- `Traceability`: stable IDs that can be referenced by later phases.
- `Risks`: uncertainty, compatibility concerns, architecture impact, or validation gaps.
- `Next action`: the next phase or the correction needed before continuing.

The exact sections should differ by artifact, but the mandatory fields above keep the
handoff disciplined.

### Suggested Artifact Shapes

The `spec.md` template should include:

- feature ID and short title
- user stories
- functional requirements
- acceptance criteria with stable IDs such as `AC-001`
- problem statement, goals, and non-goals
- user-visible behavior and system behavior
- constraints from `sdd-context`
- edge cases
- assumptions and open questions

The spec review template should include:

- reviewed `spec.md` reference
- decision: approved or rejected
- blocking findings
- non-blocking concerns
- missing or ambiguous acceptance criteria
- architecture/context conflicts
- required corrections

The `plan.md` template should include:

- approved `spec.md` reference
- technical approach
- files or areas likely touched
- architecture impact
- ADR requirement: required, not required, or uncertain
- data or API changes
- test strategy
- documentation updates
- rollout or migration concerns
- requirement-to-plan traceability

The plan review template should include:

- reviewed `plan.md` reference
- decision: approved or rejected
- feasibility findings
- architecture boundary findings
- ADR finding: required, not required, missing, or insufficient
- validation strategy findings
- missing task inputs
- required corrections

The task list template should include:

- approved `plan.md` reference
- ordered tasks with stable IDs such as `T-001`
- requirement or acceptance criteria links for each task
- dependencies between tasks
- implementation tasks
- test tasks
- documentation tasks
- verification tasks

The implementation notes template should include:

- task list reference
- completed tasks
- changed files
- tests added or updated
- tests run
- deviations from spec or plan
- unresolved issues
- handoff notes for verification

The verification report template should include:

- spec, plan, task list, and implementation notes references
- acceptance criteria checklist
- tests and commands run
- evidence for each acceptance criterion
- deviations or failures
- final decision: verified, rework-required, or blocked
- routing decision if rework is required

### Templates as Prompts

Every SDD subagent should be invoked with the relevant template as part of its prompt.
The instruction should be explicit:

```text
Use this template exactly. Fill every required section. If a section does not apply,
write "Not applicable" and explain why. Do not omit sections.
```

This makes the template a control surface. It constrains the subagent output, gives the
orchestrator predictable fields to inspect, and makes missing reasoning visible.

## ADRs and SDD

Architecture Decision Records are part of the broader harness, not owned by SDD. SDD is
optional; ADRs remain useful even when a project uses a lighter feature workflow, a bug
fix workflow, a refactor workflow, or a human-led architecture process.

Because ADRs are harness-level artifacts, they should keep their own template and storage
location, such as `templates/adr.template.md` and the repository's architecture decision
log. SDD should reference that ADR process instead of creating an SDD-specific ADR format
by default.

SDD still has an important responsibility: when a feature introduces or changes a durable
architectural decision, the SDD cycle must detect that and route through the harness ADR
process. The SDD harness should force the planner to make an explicit ADR decision.

An ADR is required when the feature:

- changes architectural boundaries, dependency direction, or module ownership
- introduces a new major framework, runtime, persistence model, queue, external service,
  or deployment pattern
- changes public API shape, compatibility policy, data ownership, or migration strategy
- creates a new cross-cutting pattern for errors, logging, auth, observability, caching,
  configuration, or validation
- establishes a precedent other future features are expected to copy
- conflicts with, replaces, or narrows an existing ADR or architecture invariant

An ADR is usually not required when the feature:

- follows an existing documented pattern without changing it
- adds behavior inside an existing boundary
- updates copy, docs, tests, or local implementation details without architectural impact
- performs a refactor that preserves the current architecture and has no new policy

The `sdd-implementation-planner` must mark the ADR requirement as `required`,
`not required`, or `uncertain`. The `sdd-plan-validator` must challenge that decision.
If an ADR is required, task generation should not proceed until the ADR exists as a draft
or the plan includes an explicit task to create it before implementation.

The `sdd-acceptance-verifier` must also check ADR follow-through:

- required ADR exists
- affected architecture docs are updated or explicitly left unchanged
- the implementation matches the recorded decision
- enforcement or validation steps from the ADR were completed or tracked

This keeps ADRs tied to real feature work while avoiding a separate architecture process
that drifts away from implementation.

## Proposed Skill Set

All SDD-related skills should use the `sdd-` prefix. This keeps the process namespace
clear and avoids confusion with existing general-purpose skills such as `new-feature`,
`bug-fix`, `refactor`, or future non-SDD specification helpers.

The skill names describe the workflow capabilities. Some of those capabilities should be
implemented as direct skills loaded into the current agent context, while others should
run as subagents. The default rule:

- Use direct skills for orchestration, routing, and small context-gathering steps.
- Use subagents for bounded production or review phases that can consume artifacts and
  return a compact result.

This prevents the main conversation from accumulating every detail from every phase.
Each phase should hand off through written artifacts, not through implicit chat history.

### 1. `sdd-orchestrator`

Owns the state machine. It does not perform the detailed work directly. It decides which
skill runs next, checks that required artifacts exist, and prevents phase skipping.

Responsibilities:

- Start a new SDD cycle from a user request.
- Load the constitutional context before specification.
- Route work through spec, plan, tasks, implementation, and verification.
- Send failures back to the correct earlier phase.
- Produce the final status: done, blocked, or rework required.

### 2. `sdd-context`

Loads the durable project rules needed for the current feature.

Responsibilities:

- Read `AGENTS.md` and the relevant architecture, design, quality, and workflow docs.
- Identify architectural invariants and dependency rules that constrain the feature.
- Identify existing skills that may apply to the implementation.
- Summarize only the context needed for the current SDD cycle.

This skill replaces the need for a mandatory `CONSTITUTION.md` file.

### 3. `sdd-spec-author`

Turns the user request into `spec.md`.

Responsibilities:

- Define the problem, desired outcome, scope, and non-goals.
- Capture user-visible behavior and system behavior.
- Record assumptions and open questions.
- Define acceptance criteria.
- Avoid implementation details unless they are explicit constraints.

If clarification is required, this skill asks the minimum necessary questions. If the
answer can be reasonably assumed, it records the assumption and continues.

Execution model: run as a subagent for non-trivial features. The subagent receives the
user request and `sdd-context` summary, then returns only `spec.md`, assumptions, and open
questions.

### 4. `sdd-spec-validator`

Reviews the specification before planning.

Responsibilities:

- Find ambiguity, missing edge cases, hidden implementation decisions, and untestable
  acceptance criteria.
- Check the spec against the loaded architecture context.
- Confirm the feature is general-purpose and not project-specific when it targets the
  template.
- Approve the spec or return it to `sdd-spec-author` with concrete corrections.

Execution model: run as a subagent. The validator should receive `spec.md` and the
context summary, not the full authoring conversation.

### 5. `sdd-implementation-planner`

Converts the approved `spec.md` into `plan.md`.

Responsibilities:

- Identify the likely files, modules, docs, and tests involved.
- Explain the implementation approach.
- Call out architecture impact, migrations, compatibility risks, and rollout concerns.
- Decide whether an ADR is required and create or schedule it when needed.
- Define the validation strategy.
- Keep the plan traceable to acceptance criteria.

Execution model: run as a subagent. The planner should receive the approved `spec.md`,
context summary, and only the repository files needed for planning.

### 6. `sdd-plan-validator`

Reviews `plan.md` before task generation.

Responsibilities:

- Check that `plan.md` actually satisfies the approved `spec.md`.
- Verify that the proposed approach respects architecture boundaries and dependency rules.
- Confirm the validation strategy is sufficient for the feature risk.
- Validate the planner's ADR decision and reject missing or insufficient ADR coverage.
- Identify missing docs, tests, migrations, rollout steps, or compatibility concerns.
- Reject plans that are too vague to turn into reliable implementation tasks.
- Approve the plan or return it to `sdd-implementation-planner` with concrete corrections.

This is a separate gate because planning mistakes are cheaper to fix before the task list
locks the agent into a sequence of implementation steps.

Execution model: run as a subagent. The validator should receive the approved `spec.md`,
`plan.md`, and context summary, then return an approval or concrete plan
corrections.

### 7. `sdd-task-breakdown`

Creates the executable task list from the approved plan.

Responsibilities:

- Produce ordered tasks that can be completed independently where possible.
- Attach each task to one or more spec requirements or acceptance criteria.
- Separate implementation, tests, docs, and verification tasks.
- Include ADR creation or update tasks before implementation when required.
- Mark tasks that require special care, such as migrations or cross-boundary changes.

Execution model: run as a subagent for medium or large features. For very small changes,
the orchestrator may keep this inline if the task list is short.

### 8. `sdd-feature-implementer`

Executes the task list.

Responsibilities:

- Read the approved `spec.md`, `plan.md`, and task list before editing.
- Make the required code, docs, config, or test changes.
- Follow existing repository patterns and loaded project rules.
- Record completed tasks and deviations.
- Avoid silently changing scope. Any spec-impacting change must be recorded for verifier
  review or routed back to planning.

Execution model: run as the main coding agent or as one or more implementation subagents,
depending on tool support and repository risk. If subagents are used, each should receive
only the approved `spec.md`, `plan.md`, relevant tasks, and relevant repository context.
The output must be changed files plus implementation notes, not a long transcript.

### 9. `sdd-acceptance-verifier`

Closes the loop.

Responsibilities:

- Run the relevant tests, linters, build commands, link checks, or manual inspections.
- Check each acceptance criterion explicitly.
- Confirm docs and architecture references were updated when needed.
- Confirm required ADRs exist and match the implementation.
- Identify deviations from the spec, plan, or task list.
- Return the cycle to the right phase if verification fails.
- Mark the feature complete only when the implemented behavior satisfies the spec.

Execution model: run as a subagent when possible. Verification benefits from isolation
because the verifier should judge the artifacts and repository state, not inherit the
implementer's assumptions.

## Context Isolation Model

The SDD harness should avoid passing the full chat history from phase to phase. Each
phase receives a deliberately small input packet and produces a durable output packet.

Recommended packet shape:

```text
phase input:
  - current artifact from the previous approved phase
  - compact context summary from `sdd-context`
  - relevant repository files or paths
  - explicit constraints and acceptance criteria

phase output:
  - produced artifact
  - decisions made
  - assumptions
  - risks or blockers
  - required next phase
```

The main `sdd-orchestrator` should keep only:

- current lifecycle state
- artifact locations or artifact summaries
- approval status for each gate
- unresolved questions
- final verification status

It should not keep every reasoning step from every phase in its active context.

Recommended execution split:

| Phase | Direct skill or subagent? | Reason |
|---|---|---|
| `sdd-orchestrator` | Direct skill | Owns state and user interaction. |
| `sdd-context` | Direct skill | Loads and summarizes project rules for the cycle. |
| `sdd-spec-author` | Subagent for non-trivial work | Produces a bounded spec from request plus context. |
| `sdd-spec-validator` | Subagent | Independent review should not inherit author assumptions. |
| `sdd-implementation-planner` | Subagent | Planning may require broad inspection but should return a compact plan. |
| `sdd-plan-validator` | Subagent | Independent review catches flawed technical plans before tasks. |
| `sdd-task-breakdown` | Subagent or direct skill | Subagent for larger plans; direct for small task lists. |
| `sdd-feature-implementer` | Main agent or subagent(s) | Depends on tool support, write access, and change size. |
| `sdd-acceptance-verifier` | Subagent | Verifier should evaluate artifacts and repo state independently. |

The important rule is that subagents communicate through artifacts. They do not depend on
the previous subagent's private reasoning or chat context.

## Forced Lifecycle

The harness should enforce this lifecycle:

```text
START
  sdd-orchestrator

CONTEXT
  sdd-context
  output: context summary

SPECIFY
  sdd-spec-author
  output: spec.md

VALIDATE SPEC
  sdd-spec-validator
  output: approved spec.md or required corrections

PLAN
  sdd-implementation-planner
  output: plan.md

VALIDATE PLAN
  sdd-plan-validator
  output: approved plan.md or required corrections

TASKS
  sdd-task-breakdown
  output: ordered task list

IMPLEMENT
  sdd-feature-implementer
  output: changed files and implementation notes

VERIFY
  sdd-acceptance-verifier
  output: pass/fail verification report

DONE OR REWORK
  sdd-orchestrator routes to the correct earlier phase
```

## Comparison With GitHub Spec Kit

| GitHub Spec Kit concept | Proposed harness skill |
|---|---|
| `constitution` | `sdd-context` backed by `AGENTS.md`, `ARCHITECTURE.md`, quality docs, and ADRs |
| `specify` | `sdd-spec-author` |
| `clarify` | `sdd-spec-author` initially; split into `sdd-spec-clarifier` only if needed |
| `checklist` | `sdd-spec-validator` |
| `plan` | `sdd-implementation-planner` |
| plan review gate | `sdd-plan-validator` |
| `tasks` | `sdd-task-breakdown` |
| `analyze` | `sdd-acceptance-verifier` initially; split into `sdd-consistency-analyzer` only if needed |
| `implement` | `sdd-feature-implementer` |
| review gates | `sdd-orchestrator` |

## Minimal Version

The template should implement the minimal five-skill model first while documenting the
full nine-phase lifecycle. This keeps the initial SDD harness practical while making the
intended end state explicit.

Start with five skills:

1. `sdd-orchestrator`
2. `sdd-spec-author`
3. `sdd-implementation-planner`
4. `sdd-feature-implementer`
5. `sdd-acceptance-verifier`

In that version:

- `sdd-orchestrator` also loads project context.
- `sdd-spec-author` also performs clarification.
- `sdd-implementation-planner` also validates the plan and produces tasks.
- `sdd-acceptance-verifier` also performs consistency analysis.

Split these responsibilities into the full nine-skill model only after real usage shows
that the combined skills are too broad.

## Gap to the Full Nine-Skill Model

The first implementation should not hide the fact that it is a compressed version of the
full lifecycle. It should include a short document that explains which nine-phase
responsibilities are currently folded into the five implemented skills and what future
work is needed to split them out.

Recommended gap mapping:

| Full lifecycle capability | Initial owner | Future split |
|---|---|---|
| Project context loading | `sdd-orchestrator` | Extract to `sdd-context` when context loading becomes large or reusable. |
| Specification authoring | `sdd-spec-author` | Keep as-is. |
| Clarification | `sdd-spec-author` | Extract to `sdd-spec-clarifier` only if clarification becomes a frequent standalone gate. |
| Specification validation | `sdd-spec-author` or `sdd-acceptance-verifier` | Extract to `sdd-spec-validator` when independent spec review is needed. |
| Implementation planning | `sdd-implementation-planner` | Keep as-is. |
| Plan validation | `sdd-implementation-planner` or `sdd-acceptance-verifier` | Extract to `sdd-plan-validator` when plans need independent technical review. |
| Task breakdown | `sdd-implementation-planner` | Extract to `sdd-task-breakdown` when task generation becomes large or reusable. |
| Implementation | `sdd-feature-implementer` | Keep as-is. |
| Acceptance verification | `sdd-acceptance-verifier` | Keep as-is. |

The gap document should make this explicit so users know the process has not been
forgotten or weakened; it has been intentionally compressed for the first version.

## Relationship to `new-feature`

Keep `new-feature` as the simple feature-delivery workflow. SDD should not replace it.
Instead, `new-feature` should be treated as **minimal SDD in a single skill**:

```text
request
  -> brief analysis/spec
  -> implicit plan
  -> ordered implementation steps
  -> docs/ADR check
  -> validation
  -> PR handoff
```

This gives downstream projects two aligned paths:

| Workflow | Use when | Shape |
|---|---|---|
| `new-feature` | The feature is small, local, low-risk, or follows established patterns. | Single skill, compact context, lightweight spec/plan/validation. |
| SDD | The feature is complex, cross-boundary, high-risk, ambiguous, or architecturally significant. | Multi-artifact process with explicit gates, templates, and optional subagents. |

Both workflows should follow the same general practice:

- understand the request before editing
- identify affected architecture and quality constraints
- decide whether ADR work is required
- plan before implementation
- implement in ordered steps
- update docs when behavior, process, or architecture changes
- validate before completion

The difference is ceremony and enforcement, not philosophy. `new-feature` compresses the
SDD lifecycle into one skill; SDD expands the lifecycle into explicit artifacts and gates.

## Design Principles

- **No implementation without an approved `spec.md`.** The spec is the feature contract.
- **No task list without a plan.** Tasks should come from architecture-aware planning, not
  from the raw request.
- **No tasks from an unvalidated plan.** Planning should have its own gate because a flawed
  plan turns into misleading tasks.
- **No silent scope changes.** Implementation may discover reality, but deviations must be
  recorded and routed through the process.
- **Traceability matters.** Acceptance criteria should connect to plan items, tasks, tests,
  and verification results.
- **Architecture decisions belong to the harness.** SDD must detect and trigger ADR work
  when a feature changes durable architecture, but ADRs remain separate harness artifacts
  usable outside SDD.
- **Templates are part of the harness.** Each phase should use a required artifact
  template so the output is predictable, complete, and machine-checkable enough for the
  next phase.
- **Artifacts carry context between phases.** Subagents should receive compact input
  packets and return structured outputs, not inherit the full conversation.
- **Architecture context replaces constitution files.** Durable rules should live in the
  repository's normal policy and architecture structure.
- **Completion is earned by verification.** A feature is not done because code changed; it
  is done because the verifier can prove the spec is satisfied.

## Open Questions

- Which verification checks should be mandatory across all projects, and which should be
  project-defined?
- Which SDD phases should always use subagents, and which should remain direct skills for
  small changes?
- Does the existing `adr.template.md` need optional fields for links back to SDD specs
  and plans, while remaining usable outside SDD?
