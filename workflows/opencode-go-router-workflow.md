# OpenCode Go Router Process: Manager and Specialist Workflow

This workflow replaces a manual copy-and-paste, multi-model coding process with a repo-local OpenCode Go setup. A manager agent plans and routes work to specialist subagents for exploration, architecture, implementation, debugging, review, and documentation.

It is intended for users who already have mid-tier browser or coding-agent subscriptions, but want more repository-coding capacity and token budget through OpenCode Go without maintaining multiple separate API subscriptions.

---

## Core Concept

The **manager** plans, routes, escalates, and recommends whether a change is ready to merge. It does not edit files. Specialist agents perform bounded tasks within their assigned roles.

This keeps the workflow:

- **Repo-local**: configuration and prompts travel with the target project
- **Repeatable**: the same roles and handoffs apply to each task
- **Controlled**: read-only passes happen before edits when risk is unclear
- **Safer**: permissions limit which agents may change files
- **Reviewable**: tests, `git status`, and `git diff` expose the result before commit

---

## Role Mapping

| Role | Model | Responsibility | File Access |
| --- | --- | --- | --- |
| **Manager** | GLM 5.2 | Planning, routing, escalation, and merge recommendation | No edits |
| **Scout** | DeepSeek V4 Flash | Cheap, read-only codebase exploration | No edits |
| **Architect** | GLM 5.2 or Qwen Max | Design, risk review, migrations, and abstractions | No edits |
| **Implementer** | Kimi K2.7 Code | Main code changes and feature work | Ask before edits |
| **Fixer** | DeepSeek V4 Pro | Debugging and failing-test repair | Ask before edits |
| **Reviewer** | Qwen Plus/Max or DeepSeek V4 Pro | Final diff and risk review | No edits |
| **Docs** | MiniMax M3 | README, examples, comments, and changelog notes | Ask before edits |

Keep planning and review roles read-only. Editing roles should receive a narrow approved scope and ask before changing files.

---

## Recommended Project Layout

Apply the router setup inside the target project:

```text
your-project/
  opencode.json
  .opencode/
    prompts/
      manager.md
      scout.md
      architect.md
      implementer.md
      fixer.md
      reviewer.md
      docs.md
    README.md
  scripts/
  src/ or app/
```

Put OpenCode-specific setup notes in `.opencode/README.md`. Do not overwrite the project's root `README.md` with agent instructions.

---

## First Manual Smoke Test

From the target project's root, start OpenCode:

```text
opencode
```

Then enter:

```text
Use @manager to plan and route this task: review the existing ideation scripts and suggest safe improvements. Do not edit files yet.
```

This verifies that OpenCode loads the project configuration, recognizes `@manager`, follows the read-only boundary, and can route exploration without modifying the working tree. Confirm the result with `git status --short`.

---

## Normal Workflow Loop

```text
manager
  scout: find relevant files and current behavior →
  architect: design the smallest safe change if risk is non-trivial →
  implementer: edit only the approved scope →
  fixer: repair failing tests or runtime errors →
  reviewer: inspect git diff and decide merge readiness →
  docs: update docs after the code is stable →
```

Not every task needs every role. The manager should skip unnecessary stages, but retain a read-only review before merge.

---

## Prompt Patterns

### Read-Only Planning

```text
Use @manager to plan and route this task: [describe the task].

Start with @scout to identify the relevant files, current behavior, tests, and constraints.
Use @architect only if the change has non-trivial design, migration, security, or compatibility risk.
Do not edit files or run destructive commands.
Return the smallest safe implementation plan, the proposed edit scope, risks, and verification commands.
```

### Approved Implementation

```text
The plan and this edit scope are approved: [list exact files or components].

Route implementation to @implementer.
Change only the approved scope and ask before any additional edits.
Do not add dependencies, change public APIs, or refactor unrelated code without approval.
Run the relevant tests and report commands run, files changed, results, and any remaining risk.
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

### Final Review

```text
Use @reviewer for a read-only final review.

Inspect git status and the complete git diff.
Check correctness, regressions, security, compatibility, test coverage, documentation impact, and out-of-scope edits.
Do not edit files.
Return findings by severity, required fixes, checks still needed, and one decision: READY TO MERGE or NOT READY TO MERGE.
```

---

## Safety Rules

- Start with read-only planning for broad, risky, or unfamiliar tasks.
- Run `git status --short` before and after agent work.
- Review the complete `git diff` before committing.
- Keep the manager, scout, architect, and reviewer read-only where possible.
- Do not let multiple agents edit the same working tree at the same time.
- Use a branch or Git worktree for risky experiments.
- Give editing agents an explicit file or component scope.
- Ask before destructive commands, dependency changes, migrations, deploys, or credential changes.
- Do not store secrets in files that agents can read. Never paste tokens or credentials into prompts.
- Commit only after tests and the final review are complete.

Use these recovery commands before attempting a manual rollback:

```text
git status --short
git diff
git restore path/to/file
```

`git restore path/to/file` discards uncommitted changes in that file. Review the diff first and specify the path deliberately.

---

## Optional Runner Script

A shell runner is optional. Do not add one until the manual workflow, role routing, permissions, and model selection have been tested successfully.

If automation is useful later, prefer one of these locations:

```text
.opencode/scripts/ai-route.sh
```

or:

```text
scripts/opencode-route.sh
```

Keep the runner thin: it should invoke the established workflow, preserve approval boundaries, stop on failures, and avoid hiding agent output or Git changes.

---

## Troubleshooting Checklist

| Problem | Check |
| --- | --- |
| `@manager` is not recognized | Start OpenCode from the target project root, then confirm `opencode.json` and the manager prompt configuration are present and valid. |
| Prompt files cannot be loaded | Confirm the prompt paths, filenames, case, and Markdown readability under `.opencode/prompts/`. |
| The wrong model appears | Use `/models`, check the model IDs in `opencode.json`, and compare them with the current official OpenCode documentation. |
| An agent edits during planning | Stop the run, inspect `git diff`, restore unwanted changes, and tighten that role's permissions and prompt to read-only. |
| The project README is overwritten | Restore the root README and move OpenCode-specific notes to `.opencode/README.md`. |
| The scripts folder is disturbed | Inspect the diff, restore unrelated script changes, and give the editing agent an explicit path-level scope. |

---

## Clean Committed State for a Target Project

When applying the router setup to another project, the intended committed files are:

```text
.opencode/prompts/architect.md
.opencode/prompts/docs.md
.opencode/prompts/fixer.md
.opencode/prompts/implementer.md
.opencode/prompts/manager.md
.opencode/prompts/reviewer.md
.opencode/prompts/scout.md
opencode.json
```

Add `.opencode/README.md` only when the target project needs OpenCode-specific setup notes. Add a runner script only after the manual workflow is proven useful. Keep runtime output, secrets, unrelated generated files, and accidental root README changes out of the commit.

---

## Verify Model IDs

OpenCode Go model labels and availability can change. Before relying on the exact names in this workflow, use `/models` and the official OpenCode documentation to verify the current model IDs supported by your subscription.

---

## One-Line Rule

```text
Manager routes. Specialists act within approved boundaries. Git shows what changed.
```
