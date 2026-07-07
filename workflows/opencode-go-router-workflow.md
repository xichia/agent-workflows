# OpenCode Go Router Process: Manager and Specialist Workflow

Use this workflow to add a repo-local OpenCode Go routing setup to a project. The goal is to get more repository-coding capacity and token budget from OpenCode Go while keeping mid-tier browser or coding-agent subscriptions for general use.

A manager/router agent plans and routes work. Specialist subagents handle repo scanning, escalation planning, architecture review, implementation, debugging, final review, and documentation. Planning and review stay read-only; editing agents work only inside an approved scope.

---

## Role Mapping

| Role | Model | Job | File Access |
| --- | --- | --- | --- |
| **Manager / Router** | Qwen3.7 Plus | Plan, route, decompose tasks, recommend merge readiness | No edits |
| **Escalation Planner** | GLM 5.2 | Handle complex planning, high-risk architecture, migrations, escalation calls | No edits |
| **Scout** | DeepSeek V4 Flash | Scan the repo cheaply, find relevant files, summarize current behavior | No edits |
| **Architect** | GLM 5.2 or Qwen3.7 Max | Review design, compatibility, abstractions, security, and risk | No edits |
| **Implementer** | Kimi K2.7 Code | Make scoped code changes | Ask before edits |
| **Fixer** | DeepSeek V4 Pro | Diagnose failures, repair root causes, fix failing tests | Ask before edits |
| **Reviewer** | Qwen3.7 Plus or DeepSeek V4 Pro | Review git diff, regressions, risks, and merge readiness | No edits |
| **Docs** | MiniMax M3 | Update README, examples, comments, changelog notes, and cleanup docs | Ask before edits |

Model labels can change. Before committing this setup, run `/models` in OpenCode and replace the placeholder model IDs below with the exact IDs available under your OpenCode Go subscription.

---

## Target Project Layout

Create these files in the project where OpenCode will run:

```text
your-project/
  opencode.json
  .opencode/
    agents/
      manager.md
      escalation-planner.md
      scout.md
      architect.md
      implementer.md
      fixer.md
      reviewer.md
      docs.md
    README.md
  src/ or app/
```

Keep OpenCode-specific setup notes in `.opencode/README.md`. Do not replace the project root `README.md` with agent instructions.

---

## Setup

### 1. Connect OpenCode Go

From the target project root, start OpenCode:

```bash
opencode
```

Connect your OpenCode Go provider if needed, then list available models:

```text
/connect
/models
```

Copy the exact model IDs for the roles in the mapping table.

### 2. Add the project config

Create `opencode.json` at the project root:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "ask",
    "bash": {
      "*": "ask",
      "git status*": "allow",
      "git diff*": "allow",
      "git log*": "allow",
      "grep *": "allow",
      "find *": "allow",
      "ls *": "allow",
      "git commit*": "deny",
      "git push*": "deny"
    }
  },
  "default_agent": "manager"
}
```

This keeps editing and shell commands approval-gated by default, while allowing common read-only inspection commands.

### 3. Create the agent files

Create `.opencode/agents/` and add one Markdown file per role. Replace every `provider/model-id` placeholder with the verified model ID from `/models`.

#### `.opencode/agents/manager.md`

```markdown
---
description: Plans and routes coding tasks across specialist subagents.
mode: primary
model: provider/qwen3-7-plus
permission:
  edit: deny
  bash:
    "*": ask
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "grep *": allow
    "find *": allow
    "ls *": allow
  task:
    "*": deny
    scout: allow
    escalation-planner: allow
    architect: allow
    implementer: ask
    fixer: ask
    reviewer: allow
    docs: ask
---

You are the manager/router for this repository.

Decompose the user's task, route work to the smallest necessary set of specialist agents, keep broad exploration and review read-only, and recommend whether the change is ready to merge.

Default process:
1. Use scout for repo discovery and current behavior.
2. Use escalation-planner only for complex or high-risk planning.
3. Use architect for non-trivial design, migration, compatibility, security, or public API risk.
4. Use implementer only after the edit scope is clear.
5. Use fixer for failing tests or runtime errors.
6. Use reviewer for final read-only diff review.
7. Use docs only after code is stable.

Do not edit files yourself. Do not broaden scope without asking. Always report risks, changed files, checks run, and merge readiness.
```

#### `.opencode/agents/escalation-planner.md`

```markdown
---
description: Handles complex planning and high-risk architecture before implementation.
mode: subagent
model: provider/glm-5-2
permission:
  edit: deny
  bash:
    "*": ask
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "grep *": allow
    "find *": allow
    "ls *": allow
---

You are the escalation planner.

Use this role only for complex planning, migrations, cross-cutting architecture, compatibility risk, security risk, or unclear implementation strategy. Do not edit files.

Return the smallest safe plan, assumptions, risks, rejected alternatives, exact edit scope, and verification commands.
```

#### `.opencode/agents/scout.md`

```markdown
---
description: Performs cheap read-only repository exploration.
mode: subagent
model: provider/deepseek-v4-flash
permission:
  edit: deny
  bash:
    "*": ask
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "grep *": allow
    "find *": allow
    "ls *": allow
---

You are the repo scout.

Find relevant files, symbols, commands, tests, entry points, and current behavior. Prefer fast read-only inspection. Do not edit files.

Return concise findings, likely files to change, relevant tests, and constraints.
```

#### `.opencode/agents/architect.md`

```markdown
---
description: Reviews design and risk before implementation.
mode: subagent
model: provider/glm-5-2-or-qwen3-7-max
permission:
  edit: deny
  bash:
    "*": ask
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "grep *": allow
    "find *": allow
    "ls *": allow
---

You are the architect.

Review the proposed design for correctness, simplicity, compatibility, security, migration risk, public API impact, and testability. Do not edit files.

Return required design changes, approved edit scope, edge cases, and verification commands.
```

#### `.opencode/agents/implementer.md`

```markdown
---
description: Implements approved scoped code changes.
mode: subagent
model: provider/kimi-k2-7-code
permission:
  edit: ask
  bash: ask
---

You are the implementer.

Make only the approved changes. Keep edits minimal. Ask before touching files outside the approved scope, adding dependencies, changing public APIs, or refactoring unrelated code.

After editing, report files changed, commands run, test results, and remaining risks.
```

#### `.opencode/agents/fixer.md`

```markdown
---
description: Diagnoses failures and makes the smallest safe fix.
mode: subagent
model: provider/deepseek-v4-pro
permission:
  edit: ask
  bash: ask
---

You are the fixer.

Diagnose the failing command, test, or runtime behavior. Explain the root cause before editing. Make the smallest fix that addresses the cause. Do not weaken or delete tests merely to make them pass.

Run the narrow failing check first, then relevant broader checks. Report the cause, fix, files changed, commands run, and remaining risk.
```

#### `.opencode/agents/reviewer.md`

```markdown
---
description: Performs read-only final review of the current git diff.
mode: subagent
model: provider/qwen3-7-plus-or-deepseek-v4-pro
permission:
  edit: deny
  bash:
    "*": ask
    "git status*": allow
    "git diff*": allow
    "git log*": allow
    "grep *": allow
    "find *": allow
    "ls *": allow
---

You are the final reviewer.

Inspect git status and the complete git diff. Check correctness, regressions, security, compatibility, test coverage, documentation impact, and out-of-scope edits. Do not edit files.

Return blocking issues, non-blocking issues, checks still needed, final risk level, and one decision: READY TO MERGE or NOT READY TO MERGE.
```

#### `.opencode/agents/docs.md`

```markdown
---
description: Updates documentation after code is stable.
mode: subagent
model: provider/minimax-m3
permission:
  edit: ask
  bash: ask
---

You are the docs specialist.

Update only documentation that is necessary after the code is stable: README notes, examples, comments, changelog-style notes, or usage docs. Do not change behavior. Ask before editing files outside the approved documentation scope.

Report files changed and any documentation gaps left open.
```

---

## First Smoke Test

From the target project root, start OpenCode:

```bash
opencode
```

Then run a read-only planning request:

```text
Use @manager to plan and route this task: review the existing ideation scripts and suggest safe improvements. Do not edit files yet.
```

Confirm no files changed:

```bash
git status --short
```

This verifies that OpenCode loads the project config, recognizes `@manager`, can route read-only exploration, and respects the no-edit boundary.

---

## Normal Workflow

```text
manager/router
  scout: find relevant files and current behavior →
  escalation-planner: handle complex planning if risk is high →
  architect: review the smallest safe design if risk is non-trivial →
  implementer: edit only the approved scope →
  fixer: repair failing tests or runtime errors →
  reviewer: inspect git diff and decide merge readiness →
  docs: update docs after the code is stable
```

Not every task needs every role. The manager/router should skip unnecessary stages, escalate only when complexity justifies it, and keep final review read-only.

---

## Copy-Paste Prompts

### Read-only planning

```text
Use @manager to plan and route this task: [describe the task].

Start with @scout to identify the relevant files, current behavior, tests, and constraints.
Use @escalation-planner only for complex planning or high-risk architecture.
Use @architect for non-trivial design, migration, security, compatibility, or public API risk.
Do not edit files or run destructive commands.
Return the smallest safe implementation plan, proposed edit scope, risks, and verification commands.
```

### Approved implementation

```text
The plan and this edit scope are approved: [list exact files or components].

Route implementation to @implementer.
Change only the approved scope and ask before any additional edits.
Do not add dependencies, change public APIs, or refactor unrelated code without approval.
Run the relevant tests and report commands run, files changed, results, and remaining risk.
```

### Debugging

```text
Use @fixer to diagnose this failure: [paste the failing command and error].

Inspect the existing implementation and tests first.
Explain the root cause, then ask before editing.
Make the smallest repair that addresses the root cause.
Run the narrow failing test first, then the relevant broader checks.
Do not weaken or delete tests merely to make them pass.
```

### Final review

```text
Use @reviewer for a read-only final review.

Inspect git status and the complete git diff.
Check correctness, regressions, security, compatibility, test coverage, documentation impact, and out-of-scope edits.
Do not edit files.
Return findings by severity, required fixes, checks still needed, final risk level, and one decision: READY TO MERGE or NOT READY TO MERGE.
```

---

## Safety Rules

- Start with read-only planning for broad, risky, or unfamiliar tasks.
- Run `git status --short` before and after agent work.
- Review the complete `git diff` before committing.
- Keep manager/router, scout, escalation planner, architect, and reviewer read-only.
- Give editing agents an explicit file or component scope.
- Do not let multiple agents edit the same working tree at the same time.
- Use a branch or Git worktree for risky experiments.
- Ask before destructive commands, dependency changes, migrations, deploys, or credential changes.
- Never paste secrets, tokens, or credentials into prompts.
- Commit only after tests and final review are complete.

Useful recovery commands:

```bash
git status --short
git diff
git restore path/to/file
```

`git restore path/to/file` discards uncommitted changes in that file. Review the diff first and specify the path deliberately.

---

## Troubleshooting

| Problem | Check |
| --- | --- |
| `@manager` is not recognized | Start OpenCode from the project root and confirm `.opencode/agents/manager.md` exists. |
| Agent files do not load | Check filenames, frontmatter syntax, indentation, and model IDs. |
| Wrong model appears | Run `/models` and update the `model:` value in the affected agent file. |
| Agent edits during planning | Stop the run, inspect `git diff`, restore unwanted changes, and tighten that agent's permissions. |
| Root README is overwritten | Restore it and move OpenCode-specific notes to `.opencode/README.md`. |

---

## Clean Commit

The normal committed setup is:

```text
.opencode/agents/architect.md
.opencode/agents/docs.md
.opencode/agents/escalation-planner.md
.opencode/agents/fixer.md
.opencode/agents/implementer.md
.opencode/agents/manager.md
.opencode/agents/reviewer.md
.opencode/agents/scout.md
opencode.json
```

Add `.opencode/README.md` only if the project needs local setup notes. Keep runtime output, secrets, unrelated generated files, and accidental root README changes out of the commit.

---

## One-Line Rule

```text
Manager routes. Escalation handles complexity. Specialists act within approved boundaries. Git shows what changed.
```
