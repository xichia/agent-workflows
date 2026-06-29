# Agent Workflow

This project uses a planner/executor workflow for AI-assisted development. The goal is to keep IDE-agent work bounded, mechanical, and easy to review.

## Model roles

- **Browser planner model**: GPT-5.5 High Intelligence, Claude Opus 4.8 Max effort, or another high-intelligence frontier model available on your current mid-tier subscription. Use this model for architecture, debugging strategy, dependency decisions, security/performance tradeoffs, and review.
- **IDE executor model**: Gemini 3.5 Flash (Low) in Antigravity, or a comparable low-cost IDE model. Use this model for terminal/file tasks and clear reporting.

## Executor role

The IDE executor may:

- run commands
- inspect files
- search code
- apply exact edits
- run tests and linters
- summarize errors
- report changed files

The IDE executor may not:

- redesign architecture
- choose new dependencies
- perform broad refactors
- make speculative fixes
- run destructive commands without confirmation
- deploy, migrate, reset, delete, or modify credentials without explicit approval

## Required response format

Use this format whenever possible:

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
...

NEXT STEP NEEDED:
- ...
```

## Safety rules

Ask before destructive or high-risk actions, including:

- `rm -rf`
- `git reset --hard`
- deleting branches
- force-pushing
- deployments
- database migrations
- credential or token changes
- major dependency upgrades

Do not print secrets, API keys, tokens, private credentials, or raw sensitive data.

## Escalation rules

Stop and report instead of guessing when:

- a command fails twice
- the correct file cannot be found
- the task requires architectural judgment
- tests fail for unclear reasons
- a requested edit conflicts with existing design
- authentication, permissions, or credentials are involved

Use the browser planner model for diagnosis, architecture, security, performance tradeoffs, or multi-step design decisions.

## One-line rule

Browser planner thinks. Gemini 3.5 Flash (Low) executes.
