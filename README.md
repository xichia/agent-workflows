# AI Agent Workflows: Planner/Executor and Manager/Specialist Patterns

Reusable AI-assisted development workflows, prompts, and operating patterns.

This repository collects practical workflows for using AI coding tools safely and efficiently. The focus is on planner/executor and manager/specialist patterns: capable planning agents handle judgment-heavy work while bounded agents handle exploration, implementation, debugging, review, and documentation.

The Antigravity workflow uses these specific terms:

- **Browser planner model** means a high-intelligence frontier model running outside the IDE, such as GPT-5.5 High Intelligence, Claude Opus 4.8 Max Effort, or whatever the strongest planning/reasoning model is available on your current mid-tier subscription.
- **IDE executor model** means Gemini 3.5 Flash (Low) running in the Antigravity IDE, or a comparable low-cost/low-effort IDE model used for mechanical execution.

## Workflows

- [Gemini 3.5 Flash (Low) in Antigravity: IDE Executor Workflow](workflows/antigravity-flash-low-workflow.md) — pairs a frontier browser planner with a low-cost IDE executor for bounded file and terminal tasks.
- [OpenCode Go Router Workflow: Manager and Specialist Agents](workflows/opencode-go-router-workflow.md) — replaces a manual multi-model copy-and-paste process with a repo-local OpenCode Go manager that routes work to specialist agents. Use it when you already have mid-tier browser or coding-agent subscriptions but want more repository-coding capacity, token budget, and role-based routing without maintaining multiple separate API subscriptions.
- [Core Hybrid Model Workflow for Agentic Coding](workflows/core-hybrid-model-workflow.md) — the simple default: ChatGPT in the browser manages the project, Codex performs repository work by default, and Claude Code is selected when a Claude model is the better fit.
- [Hybrid Model Workflow for Agentic Coding](workflows/hybrid-model-workflow.md) — the broader four-lane model: ChatGPT in the browser manages the project and selects among Codex, Claude Code, OpenCode, and Gemini according to task fit.

For OpenCode Go setup, follow the workflow's recommended project layout, prompt patterns, permissions, and manual smoke test. Use a project-root `opencode.json` plus `prompts/`, or a global `~/.config/opencode/` setup, as described in that workflow.

## Templates

- [Repo-local AGENT_WORKFLOW.md template](templates/AGENT_WORKFLOW.md)

## Core ideas

- **Antigravity workflow**: Browser planner thinks. IDE executor acts.
- **OpenCode Go workflow**: Manager routes. Specialists act within approved boundaries.
- **Core hybrid workflow**: ChatGPT manages. Codex executes by default. Claude Code is the task-fit alternative.
- **Hybrid model workflow**: ChatGPT manages and selects among four repository-executor lanes: Codex, Claude Code, OpenCode, and Gemini.

In both workflows, start with read-only discovery and planning when risk is unclear, keep edits narrowly scoped, run relevant checks, and review the Git diff before committing.
