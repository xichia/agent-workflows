# AI Agent Workflows: Planner/Executor and Manager/Specialist Patterns

Reusable AI-assisted development workflows, prompts, and operating patterns.

This repository collects practical workflows for using AI coding tools safely and efficiently. The focus is on planner/executor and manager/specialist patterns: capable planning agents handle judgment-heavy work while bounded agents handle exploration, implementation, debugging, review, and documentation.

The Antigravity workflow uses these specific terms:

- **Browser planner model** means a high-intelligence frontier model running outside the IDE, such as GPT-5.5 High Intelligence, Claude Opus 4.8 Max Effort, or whatever the strongest planning/reasoning model is available on your current mid-tier subscription.
- **IDE executor model** means Gemini 3.5 Flash (Low) running in the Antigravity IDE, or a comparable low-cost/low-effort IDE model used for mechanical execution.

## Workflows

- [Gemini 3.5 Flash (Low) in Antigravity: IDE Executor Workflow](workflows/antigravity-flash-low-workflow.md) — pairs a frontier browser planner with a low-cost IDE executor for bounded file and terminal tasks.
- [OpenCode Go Router Process: Manager and Specialist Workflow](workflows/opencode-go-router-workflow.md) — replaces a manual multi-model copy-and-paste process with a repo-local OpenCode Go manager that routes work to specialist agents. Use it when you already have mid-tier browser or coding-agent subscriptions but want more repository-coding capacity, token budget, and role-based routing without maintaining multiple separate API subscriptions.

For OpenCode Go setup, follow the workflow's recommended project layout, prompt patterns, permissions, and manual smoke test. Keep OpenCode-specific notes under `.opencode/` in the target project.

## Templates

- [Repo-local AGENT_WORKFLOW.md template](templates/AGENT_WORKFLOW.md)

## Core ideas

- **Antigravity workflow**: Browser planner thinks. IDE executor acts.
- **OpenCode Go workflow**: Manager routes. Specialists act within approved boundaries.

In both workflows, start with read-only discovery and planning when risk is unclear, keep edits narrowly scoped, run relevant checks, and review the Git diff before committing.
