---
name: executing-plans
description: Use when you have an approved specification and written implementation plan to execute directly in the current session
---

# Executing Plans

## Overview

Execute the approved plan continuously in the main agent's current session using Single-Session execution (also called Native or inline execution). Honor the human partner's selection; do not redirect them because subagents are available or because you prefer a different workflow.

**Announce at start:** "I'm using the executing-plans skill to implement this plan in Single-Session mode."

## Setup

1. Use superpowers:using-git-worktrees to create or verify an isolated workspace.
2. Confirm the plan has an approved specification, then read both, including Global Constraints, Interfaces, and Review Focus when present.
3. Raise blocking gaps or conflicts before starting. The spec is the authority; do not silently change the approved plan.
4. Create todos for the plan tasks.

## Single-Session Workflow

Execute all tasks continuously in the main session:

1. Mark the current task in progress.
2. Implement every plan requirement for that task.
3. Add or update relevant automated tests.
4. Run the task's relevant tests and every verification command specified by the plan. Read the output and compare it with the plan's expected results.
5. Only after verification passes, tick completed steps in the plan, mark the task complete, and continue without a routine check-in.

In this workflow:

- Do not dispatch task implementer subagents.
- Do not run routine per-task code reviews or maintain progress/review ledgers.
- Do not pause at routine progress checkpoints.
- You may implement before tests when strict test-first sequencing is disproportionate. Selecting Single-Session explicitly permits this sequencing exception; tests are still mandatory.
- If the plan explicitly requires test-first development for a task, follow the plan.
- After compaction, read the plan's completed checkboxes and git history before resuming; do not redo verified tasks or create a ledger to recover your place.

After all tasks, self-review the complete diff against the plan and spec, including the plan's Review Focus when present, and run final project verification. **REQUIRED SUB-SKILL:** Use superpowers:verification-before-completion before claiming completion.

### Elevated-Risk Review

Use a separate final reviewer if execution reveals any of these conditions:

- security-sensitive behavior;
- data or schema migration;
- concurrency or synchronization changes;
- broad public API changes;
- material scope expansion beyond the approved specification or plan.

When triggered, after self-review and final verification:

- **REQUIRED SUB-SKILL:** Use superpowers:requesting-code-review. Supply the plan and spec, the full branch diff, and the plan's Review Focus when present. Address blocking findings and rerun affected tests and final verification.

Routine low-risk Single-Session execution does not require a separate reviewer.

## Complete Development

After all tasks and required verification:

- Announce use of superpowers:finishing-a-development-branch.
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch and follow it to complete the work.

## When to Stop and Ask for Help

Stop for blockers, plan/spec conflicts, unclear requirements, repeated verification failures, or material scope expansion. Ask rather than guess. Also ask before irreversible or destructive operations, security-sensitive actions, or external side effects that require consent (such as merging, pushing to a shared branch, or publishing). Otherwise continue until complete.

Never start implementation on main/master without explicit human-partner consent.
