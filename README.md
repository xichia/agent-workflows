# AI Agent Workflows: Planner/Executor and Manager/Specialist Patterns

Reusable AI-assisted development workflows, prompts, and operating patterns.

This repository collects practical workflows for using AI coding tools safely and efficiently. The focus is on planner/executor and manager/specialist patterns: capable planning agents handle judgment-heavy work while bounded agents handle exploration, implementation, debugging, review, and documentation.

## Workflows

- [Core Hybrid Model Workflow for Agentic Coding](workflows/core-hybrid-model-workflow.md) — the simple default: ChatGPT in the browser manages the project, Codex performs repository work by default, and Claude Code is selected when a Claude model is the better fit.
- [Hybrid Model Workflow for Agentic Coding](workflows/hybrid-model-workflow.md) — the broader four-lane model: ChatGPT in the browser manages the project and selects among Codex, Claude Code, OpenCode, and Gemini according to task fit.
- [OpenCode Go Router Workflow: Manager and Specialist Agents](workflows/opencode-go-router-workflow.md) — replaces a manual multi-model copy-and-paste process with a repo-local OpenCode Go manager that routes work to specialist agents. Use it when you already have mid-tier browser or coding-agent subscriptions but want more repository-coding capacity, token budget, and role-based routing without maintaining multiple separate API subscriptions.

For OpenCode Go setup, follow the workflow's recommended project layout, prompt patterns, permissions, and manual smoke test. Use a project-root `opencode.json` plus `prompts/`, or a global `~/.config/opencode/` setup, as described in that workflow.

### Experimental workflows

- [Cross-Model Exchange Workflow for Agentic Coding](workflows/cross-model-exchange-workflow.md) — uses GPT-5.6 Sol as the project manager, Claude Code as the primary repository executor, and Codex for targeted independent or surgical work.

## Templates

- [Repo-local AGENT_WORKFLOW.md template](templates/AGENT_WORKFLOW.md)

## Core ideas

- **Core hybrid workflow**: ChatGPT manages. Codex executes by default. Claude Code is the task-fit alternative.
- **Hybrid model workflow**: ChatGPT manages and selects among four repository-executor lanes: Codex, Claude Code, OpenCode, and Gemini.
- **OpenCode Go workflow**: Manager routes. Specialists act within approved boundaries.

Across these workflows, start with read-only discovery and planning when risk is unclear, keep edits narrowly scoped, run relevant checks, and review the Git diff before committing.
