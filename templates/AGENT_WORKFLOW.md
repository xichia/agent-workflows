# Agent Workflow

This project uses a planner/executor workflow.

## Executor role

The IDE executor may:

- run commands
- inspect files
- search code
- apply exact edits
- run tests
- summarize errors

The IDE executor may not:

- redesign architecture
- choose new dependencies
- perform broad refactors
- make speculative fixes
- run destructive commands without confirmation

## Required response format

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

NEXT STEP NEEDED:
...