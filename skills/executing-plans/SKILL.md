---
name: executing-plans
description: Use when you have an approved specification and written implementation plan to execute directly without task implementer subagents
---

# Executing Plans

## Overview

Load the plan, review it critically, and execute it directly using the human partner's selected cadence.

**Announce at start:** "I'm using the executing-plans skill to implement this plan in [Checkpoint/Single-Session] mode."

## Choose the Mode

Honor the human partner's selection. Do not redirect them because subagents are available or because you prefer a different workflow.

- **Checkpoint mode:** deliberate direct execution with review checkpoints, suitable for a separate session.
- **Single-Session mode:** continuous direct execution by the main agent in the current session.

If no mode was selected, ask which mode to use before implementation.

## Shared Setup

1. Use superpowers:using-git-worktrees to create or verify an isolated workspace.
2. Confirm the plan has an approved specification, then read both.
3. Raise blocking gaps or conflicts before starting.
4. Create todos for the plan tasks.

## Checkpoint Mode

Execute the plan in reviewable batches. For each task, mark it in progress, follow its steps, run its verification, and mark it complete. At each agreed checkpoint, report results and wait before continuing.

## Single-Session Mode

Execute all tasks continuously in the main session:

1. Mark the current task in progress.
2. Implement every plan requirement for that task.
3. Add or update relevant automated tests.
4. Run the task's relevant tests.
5. Mark the task complete and continue without a routine check-in.

In this mode:

- Do not dispatch task implementer subagents.
- Do not run routine per-task code reviews or maintain review ledgers.
- Do not pause at routine progress checkpoints.
- You may implement before tests when strict test-first sequencing is disproportionate. Tests are still mandatory.
- If the plan explicitly requires test-first development for a task, follow the plan.

After all tasks, self-review the complete diff and run final project verification.

### Elevated-Risk Review

Use a separate final reviewer if execution reveals any of these conditions:

- security-sensitive behavior;
- data or schema migration;
- concurrency or synchronization changes;
- broad public API changes;
- material scope expansion beyond the approved specification or plan.

When triggered, after self-review and final verification:

- **REQUIRED SUB-SKILL:** Use superpowers:requesting-code-review.

Routine low-risk Single-Session execution does not require a separate reviewer.

## Complete Development

After all tasks and required verification:

- Announce use of superpowers:finishing-a-development-branch.
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch and follow it to complete the work.

## When to Stop and Ask for Help

Stop for blockers, plan/spec conflicts, unclear requirements, repeated verification failures, or material scope expansion. Ask rather than guess. Checkpoint mode also stops at agreed checkpoints; Single-Session mode otherwise continues until complete.

Never start implementation on main/master without explicit human-partner consent.
