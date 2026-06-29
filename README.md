# AI Agent Workflows: Frontier Planner, IDE Executor

Reusable AI-assisted development workflows, prompts, and operating patterns.

This repository collects practical workflows for using AI coding tools safely and efficiently. The focus is on planner/executor patterns: frontier browser models handle architecture, debugging, and review, while lower-cost IDE models handle bounded file and terminal tasks.

In this workflow:

- **Browser planner model** means a high-intelligence frontier model running outside the IDE, such as GPT-5.5 High Intelligence, Claude Opus 4.8 Max Effort, or whatever the strongest planning/reasoning model is available on your current mid-tier subscription.
- **IDE executor model** means Gemini 3.5 Flash (Low) running in the Antigravity IDE, or a comparable low-cost/low-effort IDE model used for mechanical execution.

## Workflows

- [Gemini 3.5 Flash (Low) in Antigravity: IDE Executor Workflow](workflows/antigravity-flash-low-workflow.md)

## Templates

- [Repo-local AGENT_WORKFLOW.md template](templates/AGENT_WORKFLOW.md)

## Core idea

Browser planner thinks. IDE executor acts.

Use the stronger browser planner model for planning, debugging, architecture, and review. Use Gemini 3.5 Flash (Low) in Antigravity for running commands, inspecting files, applying exact edits, and reporting results.
