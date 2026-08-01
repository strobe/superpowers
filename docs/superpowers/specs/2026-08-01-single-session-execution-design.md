# Single-Session Execution Design

## Problem

The current implementation-plan handoff offers subagent-driven development or inline execution through `executing-plans`. For small features, both paths can impose disproportionate process overhead through strict test-first sequencing, repeated checkpoints, and review gates.

Users need a third handoff choice that preserves an approved specification, a written implementation plan, task-level testing, and final verification while allowing the main agent to execute a small plan continuously in the current session.

## Goals

- Add Single-Session Execution as an explicit user-selectable plan execution option.
- Keep an approved specification and written implementation plan mandatory.
- Execute directly in the main agent's current session without subagents.
- Require relevant tests after each task and final project verification.
- Avoid routine per-task reviews, review ledgers, and human checkpoints.
- Use a separate final reviewer only when execution reveals elevated risk.

## Non-Goals

- Replacing specifications or implementation plans with inline checklists.
- Automatically restricting Single-Session Execution by task count or risk level.
- Removing tests or final verification.
- Creating a separate execution-strategy or single-session skill.
- Changing the subagent-driven development workflow.

## Skill Structure and Routing

`executing-plans` will support two explicit modes:

1. **Checkpoint mode** preserves deliberate plan execution with review checkpoints and remains suitable for execution in a separate session.
2. **Single-Session mode** has the main agent execute continuously in the current session with reduced procedural overhead.

`writing-plans` will offer three handoff choices after a plan is saved:

1. **Subagent-Driven** — fresh subagent and review per task through `subagent-driven-development`.
2. **Checkpoint Execution** — deliberate execution through `executing-plans` in Checkpoint mode.
3. **Single-Session Execution** — continuous direct execution through `executing-plans` in Single-Session mode.

The human partner may always select Single-Session Execution. The agent may explain trade-offs but must not impose a task-count or risk eligibility rule.

`using-superpowers` will include concise routing guidance so agents distinguish the three choices without invoking another chooser skill.

## Single-Session Workflow

When the human partner selects Single-Session mode, the main agent will:

1. Load and critically review the written plan once.
2. Raise only ambiguities or gaps that block correct implementation.
3. Create task tracking and execute all plan tasks continuously.
4. Preserve every plan requirement and test requirement.
5. Allow implementation before tests when strict TDD is disproportionate, while still requiring relevant automated tests for each task.
6. Run each task's relevant tests before marking that task complete.
7. Avoid subagents, per-task code reviews, review ledgers, and routine progress checkpoints.
8. Stop only for a genuine blocker, a plan conflict, or failed verification that cannot be resolved safely.
9. Self-review the complete diff and run final project verification.
10. Invoke a separate final reviewer only if execution reveals elevated risk.
11. Complete the branch through `finishing-a-development-branch`.

Selecting Single-Session mode is an explicit exception to strict test-first sequencing. It is not permission to skip tests, plan requirements, self-review, or final verification. If the plan explicitly mandates test-first work for a task, that requirement still governs.

## Elevated-Risk Review Trigger

A separate final code review becomes required if execution reveals any of the following:

- security-sensitive behavior;
- data or schema migration;
- concurrency or synchronization changes;
- broad public API changes;
- material scope expansion beyond the approved specification or plan.

Routine low-risk execution ends after main-agent self-review and successful final verification.

## Files

- Modify `skills/executing-plans/SKILL.md` to define Checkpoint and Single-Session modes.
- Modify `skills/writing-plans/SKILL.md` to offer all three execution choices.
- Modify `skills/using-superpowers/SKILL.md` to document concise execution routing.

## Evaluation Strategy

Skill changes will follow `writing-skills` RED-GREEN-REFACTOR discipline and be completed one skill at a time:

1. Run a baseline pressure scenario against the unchanged skill and capture the unwanted behavior.
2. Apply the minimal behavior-shaping change.
3. Re-run the same scenario with the updated skill.
4. Check cross-skill consistency, frontmatter, Markdown, and routing terminology.
5. Commit the verified skill before modifying the next skill.

Scenarios will verify that agents:

- offer all three execution choices;
- honor a Single-Session selection without adding subagents or routine review gates;
- run relevant tests after every task and complete final verification;
- treat test-first sequencing as optional only within the defined exception;
- require a separate reviewer when elevated risk emerges.

## Success Criteria

- Plan handoff presents three distinct, accurately described execution choices.
- Selecting Single-Session routes to the main-agent workflow in `executing-plans`.
- The workflow requires an approved specification and written plan.
- The workflow has no routine subagent, per-task review, ledger, or checkpoint overhead.
- Task tests and final verification remain mandatory.
- Elevated-risk work receives a separate final review.
- Existing Subagent-Driven and Checkpoint execution remain available and unambiguous.
