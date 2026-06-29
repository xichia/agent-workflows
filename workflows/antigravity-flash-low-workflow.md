# Gemini 3.5 Flash (Low) in Antigravity: IDE Executor Workflow

This workflow keeps Antigravity IDE quota use low by splitting coding work into two roles:

- **Browser planner model**: a high-intelligence frontier model running outside the IDE, such as GPT-5.5 High Intelligence, Claude Opus 4.8 Max Effort, or whatever the strongest planning/reasoning model is available on your current mid-tier subscription. Use this model as planner, debugger, architect, and reviewer.
- **Gemini 3.5 Flash (Low) in Antigravity**: the IDE executor for terminal/file tasks and small mechanical edits.

The goal is to make Gemini 3.5 Flash (Low) do cheap, bounded IDE work while the browser planner model handles judgment-heavy reasoning.

---

## Core Principle

Use **Gemini 3.5 Flash (Low)** as the default Antigravity IDE model when the task is mechanical:

- Run commands
- Inspect files
- Search code
- Apply exact edits
- Run tests and linters
- Report errors and changed files

Use the **browser planner model** for:

- Debugging strategy
- Architecture
- Multi-step planning
- Dependency decisions
- Security or performance tradeoffs
- Complex refactors
- Interpreting vague failures

---

## Recommended Loop

```text
Gemini 3.5 Flash (Low) gathers facts
→ Browser planner model reasons
→ Browser planner model writes exact executor prompt
→ Gemini 3.5 Flash (Low) executes
→ Gemini 3.5 Flash (Low) reports output
→ Browser planner model reviews
```

Do not ask Gemini 3.5 Flash (Low) to “figure it out” unless the task is simple. Treat it like a junior terminal assistant.

---

## Gemini 3.5 Flash (Low) Session Prompt

Paste this at the start of an Antigravity session:

```text
You are my low-cost IDE executor running as Gemini 3.5 Flash (Low) in Antigravity.

Your role is to run commands, inspect files, apply exact edits, run tests, and report results.
Do not design architecture, choose dependencies, refactor broadly, or invent solutions unless explicitly asked.

When blocked or when a command fails, stop and report:

COMMANDS RUN:
RESULT:
ERROR OUTPUT:
FILES CHANGED:
BLOCKED ON:

Ask before destructive actions: rm -rf, git reset --hard, deploys, migrations, credential changes, or major dependency upgrades.
Keep responses short.
```

---

## Gemini 3.5 Flash (Low) Task Prompt

Use this for each small IDE task:

```text
Do this mechanical task only: [task]
Do not fix anything else.
Do not make architectural decisions.
Afterward, report commands run, files changed, and any errors.
```

---

## Browser Planner Prompt

Use this with the stronger model in your browser, for example GPT-5.5 High Intelligence, Claude Opus 4.8 Max Effort, or the strongest frontier-tier model available on your current subscription:

````text
You are my coding planner.
I will use Gemini 3.5 Flash (Low) in the Antigravity IDE as the executor.
Diagnose the issue and give me exact copy-paste instructions for the IDE executor.
Make the task small, mechanical, and safe.

Output:
DIAGNOSIS:
PLAN:
SEND TO GEMINI 3.5 FLASH (LOW):
```text
...
```
WHAT TO PASTE BACK:
...
````

---

## Good Gemini 3.5 Flash (Low) Tasks

```text
Run git status --short and npm test. Do not modify files.
```

```text
Search the repo for "createSession" and list the file paths only.
```

```text
Open src/auth/session.ts and report the function signatures only.
```

```text
Apply the following exact patch. Then run npm test -- src/auth/session.test.ts.
```

```text
Run the linter and report only the files with errors.
```

---

## Bad Gemini 3.5 Flash (Low) Tasks

Avoid prompts like:

```text
Refactor auth.
```

```text
Fix the app.
```

```text
Improve the architecture.
```

```text
Find the best way to solve this.
```

These prompts require judgment and can waste quota or create poor edits.

---

## First Debugging Prompt

For debugging, start with fact gathering:

```text
Run:
git status --short
npm test

Do not modify files.
Report only failing tests, stack traces, and relevant file paths.
```

Then paste the result into the browser planner model and ask it to diagnose.

---

## Exact Executor Prompt Pattern

After the browser planner model diagnoses the issue, ask it to produce something like this:

```text
Open src/api/users.ts.

Make only this change:
- Replace getUser(id) with await getUser(id) inside loadUserProfile.

Then run:
npm test -- src/api/users.test.ts

If it fails, paste the full error output.
Do not make further edits.
```

This is ideal for Gemini 3.5 Flash (Low) because the task is specific, bounded, and testable.

---

## Repo-Local Agent File

Create this file in each repo:

```text
AGENT_WORKFLOW.md
```

Suggested contents:

````markdown
# Agent Workflow

This project uses a planner/executor workflow.

## Model roles

- Browser planner model: GPT-5.5 High Intelligence, Claude Opus 4.8 Max Effort, or another high-intelligence frontier model available on your current mid-tier subscription.
- IDE executor model: Gemini 3.5 Flash (Low) in Antigravity, or a comparable low-cost IDE model.

## Gemini 3.5 Flash (Low) role

Gemini 3.5 Flash (Low) may:

- run commands
- inspect files
- search code
- apply exact edits
- run tests
- summarize errors

Gemini 3.5 Flash (Low) may not:

- redesign architecture
- choose new dependencies
- perform broad refactors
- make speculative fixes
- run destructive commands without confirmation

## Required response format

COMMANDS RUN:
- ...

RESULT:
- success / failure

ERROR OUTPUT:
```text
...
```

FILES CHANGED:
- ...

BLOCKED ON:
- ...
````

Then tell Antigravity:

```text
Read AGENT_WORKFLOW.md and follow it for this session.
```

---

## Structured Output Format

Ask Gemini 3.5 Flash (Low) to return this format whenever possible:

````text
COMMANDS RUN:
- ...

FILES READ:
- ...

FILES CHANGED:
- ...

RESULT:
- success / failure

ERROR OUTPUT:
```text
...
```

NEXT STEP NEEDED:
- ...
````

Structured output makes it easy to paste results back into the browser planner model.

---

## Escalation Rules

Recommended model escalation:

```text
Attempt 1: Gemini 3.5 Flash (Low)
Attempt 2: Gemini 3.5 Flash (Low) with more exact instructions
Attempt 3: Gemini 3.5 Flash (Medium), or the next smarter Antigravity IDE executor model
Planning/debugging/architecture: browser planner model
```

Escalate from **Gemini 3.5 Flash (Low)** to **Gemini 3.5 Flash (Medium)**, or the next smarter Antigravity IDE executor model, when:

- It cannot find the right file
- It misreads a long error log
- It applies edits incorrectly
- The task needs mild judgment
- The same command or edit fails twice

Do not escalate just because you are impatient. Most quota is wasted on vague prompts.

---

## Recommended Default Setup

Use **Gemini 3.5 Flash (Low)** as the default Antigravity IDE model. Use **Gemini 3.5 Flash (Medium)**, or the next smarter Antigravity IDE executor model, only when the task still belongs in the IDE but needs mild interpretation.

Use the browser planner model for everything involving judgment, strategy, debugging, design, or multi-step planning.

---

## One-Line Rule

```text
Browser planner thinks. Gemini 3.5 Flash (Low) executes.
```
