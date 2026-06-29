# Antigravity Flash Low Planner/Executor Workflow

This workflow keeps Antigravity IDE quota use low by splitting coding work into two roles:

- **Higher-thinking browser model**: planner, debugger, architect, reviewer
- **Antigravity Flash Low**: terminal/file executor for small mechanical tasks

The goal is to make Flash Low do cheap, bounded IDE work while the stronger model handles judgment-heavy reasoning.

---

## Core Principle

Use **Flash Low** as the default IDE model when the task is mechanical:

- Run commands
- Inspect files
- Search code
- Apply exact edits
- Run tests and linters
- Report errors and changed files

Use the **browser model** for:

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
Flash Low gathers facts
→ Browser model reasons
→ Browser model writes exact executor prompt
→ Flash Low executes
→ Flash Low reports output
→ Browser model reviews
```

Do not ask Flash Low to “figure it out” unless the task is simple. Treat it like a junior terminal assistant.

---

## Flash Low Session Prompt

Paste this at the start of an Antigravity session:

```text
You are my low-cost IDE executor.

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

## Flash Low Task Prompt

Use this for each small IDE task:

```text
Do this mechanical task only:

[task]

Do not fix anything else.
Do not make architectural decisions.
Afterward, report commands run, files changed, and any errors.
```

---

## Browser Planner Prompt

Use this with the stronger model in your browser:

```text
You are my coding planner. I will use a weak IDE model as executor.

Diagnose the issue and give me exact copy-paste instructions for the IDE executor.

Make the task small, mechanical, and safe.

Output:

DIAGNOSIS:
PLAN:
SEND TO FLASH LOW:
```text
...
```
WHAT TO PASTE BACK:
...
```

---

## Good Flash Low Tasks

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

## Bad Flash Low Tasks

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

Then paste the result into the browser model and ask it to diagnose.

---

## Exact Executor Prompt Pattern

After the browser model diagnoses the issue, ask it to produce something like this:

```text
Open src/api/users.ts.

Make only this change:
- Replace getUser(id) with await getUser(id) inside loadUserProfile.

Then run:
npm test -- src/api/users.test.ts

If it fails, paste the full error output. Do not make further edits.
```

This is ideal for Flash Low because the task is specific, bounded, and testable.

---

## Repo-Local Agent File

Create this file in each repo:

```text
AGENT_WORKFLOW.md
```

Suggested contents:

```markdown
# Agent Workflow

This project uses a planner/executor workflow.

## Flash Low role

Flash Low may:
- run commands
- inspect files
- search code
- apply exact edits
- run tests
- summarize errors

Flash Low may not:
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
```

Then tell Antigravity:

```text
Read AGENT_WORKFLOW.md and follow it for this session.
```

---

## Structured Output Format

Ask Flash Low to return this format whenever possible:

```text
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
```

Structured output makes it easy to paste results back into the browser model.

---

## Escalation Rules

Recommended model escalation:

```text
Attempt 1: Flash Low
Attempt 2: Flash Low with more exact instructions
Attempt 3: Flash Medium
Planning/debugging/architecture: browser model
```

Escalate from **Flash Low** to **Flash Medium** when:

- It cannot find the right file
- It misreads a long error log
- It applies edits incorrectly
- The task needs mild judgment
- The same command or edit fails twice

Do not escalate just because you are impatient. Most quota is wasted on vague prompts.

---

## Recommended Default Setup

Use **Flash Low** as the default Antigravity IDE model.

Use **Flash Medium** only as a smarter executor when the task still belongs in the IDE but needs mild interpretation.

Use the browser model for everything involving judgment, strategy, debugging, design, or multi-step planning.

---

## One-Line Rule

```text
Browser model thinks. Flash Low executes.
```
