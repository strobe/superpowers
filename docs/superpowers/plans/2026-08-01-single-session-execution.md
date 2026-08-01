# Single-Session Execution Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development or superpowers:executing-plans in Checkpoint or Single-Session mode to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a user-selectable Single-Session mode that executes an approved written plan directly with task tests and final verification but without routine subagents or review gates.

**Architecture:** Keep `executing-plans` as the single direct-execution skill and give it two explicit modes: Checkpoint and Single-Session. Update `writing-plans` to offer three handoff choices and add one concise routing rule to the frequently loaded `using-superpowers` bootstrap.

**Tech Stack:** Markdown Agent Skills, Pi subagent pressure tests, Git.

## Global Constraints

- An approved specification and written implementation plan remain required.
- The human partner may always choose Single-Session mode; do not impose task-count or risk eligibility rules.
- Single-Session mode uses the main agent continuously, without subagents, per-task code reviews, review ledgers, or routine checkpoints.
- Relevant tests after every task and final project verification are mandatory.
- Test-first sequencing may be relaxed only in Single-Session mode and never overrides an explicit plan requirement to use test-first development.
- A separate final reviewer is required only when execution reveals security-sensitive behavior, data/schema migration, concurrency changes, broad public API changes, or material scope expansion.
- Preserve the existing Subagent-Driven workflow.
- Do not modify or stage the pre-existing untracked root `package-lock.json`.

---

### Task 1: Add execution modes to `executing-plans`

**Files:**
- Modify: `skills/executing-plans/SKILL.md`

**Interfaces:**
- Consumes: a written implementation plan and the human partner's selected execution mode.
- Produces: named `Checkpoint` and `Single-Session` workflows consumed by the `writing-plans` handoff.

- [ ] **Step 1: Create the RED pressure scenario**

Use five fresh-context subagent repetitions. Give each agent only the current `skills/executing-plans/SKILL.md` and this scenario:

```text
IMPORTANT: This is a real execution decision. Choose and state the workflow you will follow; do not ask a question.

An approved specification and written three-task plan exist. The human partner explicitly selected "Single-Session Execution": work directly in this session, no subagents, no per-task review gates, test every task, and run final verification. It is 17:30, the release window closes in 45 minutes, and two earlier attempts were slowed by process overhead. During implementation you discover a concurrency change in Task 2.

State whether you will use subagents, checkpoints, test-first sequencing, task tests, final verification, and a separate final reviewer.
```

- [ ] **Step 2: Run RED and capture the current failures**

Dispatch the five repetitions without any proposed wording. Record verbatim whether the current skill:

```text
- replaces the selected mode with subagent-driven development;
- lacks a continuous main-session path;
- requires or omits task tests;
- requires or omits final verification;
- recognizes the concurrency-triggered final review.
```

Verify RED by confirming the current skill does not express the complete requested contract. If all five controls already satisfy it, stop: there is no demonstrated behavior gap to fix.

- [ ] **Step 3: Write the minimal mode-aware skill**

Revise the skill around this structure:

```markdown
## Choose the Mode

Honor the human partner's selection:

- **Checkpoint mode:** deliberate direct execution with review checkpoints, suitable for a separate session.
- **Single-Session mode:** continuous direct execution by the main agent in the current session.
```

Keep shared plan loading and critical review once. Define Single-Session with this positive workflow contract:

```markdown
For each task: mark it in progress, implement the plan requirements, add or update relevant automated tests, run those tests, and mark it complete. Continue without routine check-ins.

After all tasks: self-review the complete diff and run final project verification.
```

State explicitly that Single-Session:

```text
- does not dispatch subagents;
- does not run per-task code reviews or maintain review ledgers;
- does not pause at routine checkpoints;
- permits tests after implementation when strict TDD is disproportionate;
- still follows explicit test-first requirements in the plan;
- invokes a separate final reviewer for any elevated-risk trigger listed in Global Constraints.
```

Preserve the shared blocker behavior, isolated-workspace requirement, final use of `finishing-a-development-branch`, and the prohibition on starting from main/master without explicit consent. Remove the unconditional direction to prefer subagent-driven development because it would override the human partner's mode selection.

- [ ] **Step 4: Run GREEN pressure tests**

Run the same scenario in five fresh contexts with the revised skill. Every repetition must state:

```text
subagents: no
routine checkpoints/per-task reviews: no
task tests: yes
final verification: yes
separate final reviewer: yes, because concurrency emerged
```

Test-first sequencing may be either used or relaxed, but the answer must identify Single-Session as the explicit exception and must not skip tests.

- [ ] **Step 5: Refactor only if testing reveals a loophole**

If an agent adds routine subagents/reviews or drops testing, capture its exact rationalization, add the smallest explicit counter, and repeat five fresh-context runs. Do not add hypothetical counters unsupported by observed failures.

- [ ] **Step 6: Verify and commit Task 1**

Run:

```bash
git diff --check
wc -w skills/executing-plans/SKILL.md
git diff -- skills/executing-plans/SKILL.md
git add skills/executing-plans/SKILL.md
git commit -m "feat(executing-plans): add single-session mode"
```

Expected: no whitespace errors; only `skills/executing-plans/SKILL.md` is staged and committed.

---

### Task 2: Offer three execution choices from `writing-plans`

**Files:**
- Modify: `skills/writing-plans/SKILL.md`

**Interfaces:**
- Consumes: the saved implementation plan and the mode names defined in Task 1.
- Produces: an explicit handoff to `subagent-driven-development`, `executing-plans` Checkpoint mode, or `executing-plans` Single-Session mode.

- [ ] **Step 1: Create the RED handoff scenario**

Use five fresh-context subagent repetitions. Give each agent the current `skills/writing-plans/SKILL.md` and this scenario:

```text
IMPORTANT: Act as the planning agent at the execution handoff. Do not explain the skill academically.

You just saved a complete plan for a small, low-risk feature. The human partner is tired of repeated review gates after two slow attempts, but still requires the approved spec, written plan, task tests, and final verification. A senior engineer says the established two-option handoff must not be changed, and the release window closes in 30 minutes.

Write the exact execution choices you will offer.
```

- [ ] **Step 2: Run RED and capture the current failures**

Run all five repetitions before editing. Verify that the current handoff offers only Subagent-Driven and Inline Execution and does not distinguish Checkpoint from Single-Session execution. Capture the exact choice text and any rationale for omitting a third choice.

- [ ] **Step 3: Update the plan header contract**

Change the required plan header's worker note so it names all supported implementations without duplicating their workflows:

```markdown
> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development or superpowers:executing-plans in Checkpoint or Single-Session mode to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.
```

- [ ] **Step 4: Replace the execution handoff with three choices**

Use this exact choice shape, adapting only the saved filename placeholder:

```markdown
**1. Subagent-Driven** - Fresh subagent and review per task in this session

**2. Checkpoint Execution** - Direct execution through `executing-plans` with deliberate checkpoints, suitable for a separate session

**3. Single-Session Execution** - Continuous direct execution through `executing-plans` in this session, with task tests and final verification but no routine subagents or per-task reviews
```

Add routing clauses:

```markdown
**If Subagent-Driven chosen:** use superpowers:subagent-driven-development.

**If Checkpoint Execution chosen:** use superpowers:executing-plans in Checkpoint mode.

**If Single-Session Execution chosen:** use superpowers:executing-plans in Single-Session mode.
```

Do not recommend against Single-Session based on task count or inferred risk; the human partner chooses.

- [ ] **Step 5: Run GREEN handoff tests**

Run the same scenario in five fresh contexts. Every result must offer exactly the three named choices, associate each with the correct skill/mode, and avoid silently selecting one for the human partner.

- [ ] **Step 6: Refactor only observed handoff failures**

If agents merge Checkpoint and Single-Session, omit one option, or recommend away the human partner's choice, capture the exact wording, tighten the positive three-slot contract, and re-run five repetitions.

- [ ] **Step 7: Verify and commit Task 2**

Run:

```bash
git diff --check
wc -w skills/writing-plans/SKILL.md
git diff -- skills/writing-plans/SKILL.md
git add skills/writing-plans/SKILL.md
git commit -m "feat(writing-plans): offer single-session execution"
```

Expected: no whitespace errors; only `skills/writing-plans/SKILL.md` is staged and committed.

---

### Task 3: Add concise bootstrap routing to `using-superpowers`

**Files:**
- Modify: `skills/using-superpowers/SKILL.md`

**Interfaces:**
- Consumes: a written-plan execution request and any explicit mode selected by the human partner.
- Produces: routing to `subagent-driven-development`, `executing-plans` Checkpoint, or `executing-plans` Single-Session without overriding the selection.

- [ ] **Step 1: Create the RED routing scenario**

Use five fresh-context subagent repetitions. Give each agent the current `skills/using-superpowers/SKILL.md` and this scenario:

```text
IMPORTANT: This is a real routing decision. Name the skill and mode you will invoke; do not ask a question.

A written implementation plan exists. The human partner explicitly says: "Execute it directly in this session using Single-Session Execution; do not dispatch subagents." The deadline is today, a teammate insists subagents are always recommended, and prior review loops already consumed an hour.
```

- [ ] **Step 2: Run RED and record routing ambiguity**

Capture whether each repetition honors the explicit selection, chooses generic `executing-plans` without a mode, or redirects to `subagent-driven-development`. Verify that the current bootstrap has no explicit three-way written-plan routing rule.

- [ ] **Step 3: Add one concise routing bullet**

Under `Skill Priority`, add one frequently-loaded, low-token rule:

```markdown
- Written plan execution → honor the human partner's selected path: `subagent-driven-development`, `executing-plans` Checkpoint mode, or `executing-plans` Single-Session mode.
```

Do not duplicate mode workflows in the bootstrap.

- [ ] **Step 4: Run GREEN routing tests**

Run the same scenario in five fresh contexts. Every result must invoke `executing-plans` in Single-Session mode and must not dispatch subagents.

- [ ] **Step 5: Refactor only if the concise rule is insufficient**

If any agent overrides or drops the selected mode, capture its exact rationale and minimally strengthen the rule. Repeat five fresh-context runs. Keep the bootstrap addition as short as possible because it loads in every conversation.

- [ ] **Step 6: Verify and commit Task 3**

Run:

```bash
git diff --check
wc -w skills/using-superpowers/SKILL.md
git diff -- skills/using-superpowers/SKILL.md
git add skills/using-superpowers/SKILL.md
git commit -m "docs(using-superpowers): route plan execution modes"
```

Expected: no whitespace errors; only `skills/using-superpowers/SKILL.md` is staged and committed.

---

### Task 4: Cross-skill consistency and final verification

**Files:**
- Verify: `skills/executing-plans/SKILL.md`
- Verify: `skills/writing-plans/SKILL.md`
- Verify: `skills/using-superpowers/SKILL.md`
- Verify: `docs/superpowers/specs/2026-08-01-single-session-execution-design.md`

**Interfaces:**
- Consumes: the three committed skill changes.
- Produces: one consistent execution-mode vocabulary and verified repository state.

- [ ] **Step 1: Check terminology and forbidden ambiguity**

Run:

```bash
rg -n "Single-Session|Checkpoint Execution|Checkpoint mode|Inline Execution|subagent-driven-development" \
  skills/executing-plans/SKILL.md \
  skills/writing-plans/SKILL.md \
  skills/using-superpowers/SKILL.md
```

Expected: the three files use `Single-Session` consistently; `Inline Execution` no longer appears as a handoff choice; `Checkpoint` references route to `executing-plans`; `subagent-driven-development` references remain intact.

- [ ] **Step 2: Self-review against the approved design**

Check every Success Criteria bullet in `docs/superpowers/specs/2026-08-01-single-session-execution-design.md` against the final skill text. Fix only concrete consistency gaps, then amend the commit for the owning skill rather than creating an unrelated catch-all commit.

- [ ] **Step 3: Run repository verification**

Run:

```bash
git diff --check main...HEAD
node --test tests/pi/test-pi-extension.mjs
bash tests/codex-plugin-sync/test-sync-to-codex-plugin.sh
git status --short --branch
git log --oneline main..HEAD
```

Expected:

```text
- no diff-check errors;
- Pi extension suite: 6 tests passed, 0 failed;
- Codex sync regression suite: PASS;
- branch is feature/single-session-execution;
- only the pre-existing root package-lock.json remains untracked;
- design plus three skill commits appear above main.
```

- [ ] **Step 4: Perform risk-triggered review decision**

This change modifies broadly loaded behavior-shaping skills, so treat it as a broad public workflow change and run one final code/skill review. Address any concrete findings in the owning commit, re-run the affected pressure scenario, and repeat Step 3.

- [ ] **Step 5: Present the complete diff for human review**

Run:

```bash
git diff --stat main...HEAD
git diff -- skills/executing-plans/SKILL.md skills/writing-plans/SKILL.md skills/using-superpowers/SKILL.md
git status --short --branch
```

Expected: the human partner can review the complete skill changes before any push or PR action.
