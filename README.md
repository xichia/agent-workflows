# Agent Workflows

Reusable AI-assisted development workflows, prompts, and operating patterns.

This repository collects practical workflows for using AI coding tools safely and efficiently. The focus is on planner/executor patterns: stronger reasoning models handle architecture, debugging, and review, while lower-cost IDE models handle bounded file and terminal tasks.

## Workflows

- [Antigravity Flash Low Planner/Executor Workflow](workflows/antigravity-flash-low-workflow.md)

## Templates

- [Repo-local AGENT_WORKFLOW.md template](templates/AGENT_WORKFLOW.md)

## Core idea

Browser model thinks. IDE executor acts.

Use the stronger model for planning, debugging, architecture, and review. Use the IDE model for running commands, inspecting files, applying exact edits, and reporting results.
