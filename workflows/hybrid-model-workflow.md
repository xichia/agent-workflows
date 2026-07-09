# Hybrid Model Workflow for Agentic Coding

This document describes how to choose between available coding-agent executors and how OpenCode, Claude Code, Codex, Gemini, GPT-5.5, and Ian fit into the overall workflow.

Ian = human ratifier  
GPT-5.5 in browser = strategic project/model manager

## Executor Lane A — Gemini directly inside Antigravity IDE

- Gemini 3.5 Flash (Low, Medium, High)
- Gemini 3.1 Pro (Low, High)
- Best for direct IDE execution, quick implementation

## Executor Lane B — OpenCode

- Local OpenCode manager: Qwen3.7 Plus
- Routes internally to OpenCode-configured open-source models
- Best for multi-step routed work: inspect → plan → implement → review → report

### OpenCode internal model roles

- Manager / router / planning: Qwen3.7 Plus
- Scout / factual repo handoff: DeepSeek V4 Flash
- Escalation planning / complex planning: GLM 5.2
- Architecture review / design validation: GLM 5.2
- Consultant / high-level second opinion: Qwen3.7 Max
- Implementation: Kimi K2.7 Code
- Debugging / failing tests / runtime contradictions: DeepSeek V4 Pro
- Final review: DeepSeek V4 Pro
- Docs / cleanup: MiniMax M3

## Executor Lane C — Claude Code

- Claude Sonnet 5 handles the prompt directly in Claude Code
- `/advisor` uses Claude Opus 4.8 for higher-level review, strategy, architecture critique, and difficult judgment calls
- Best for agentic coding sessions where Claude Code can inspect, edit, test, and iterate directly
- Use Sonnet 5 for normal implementation flow; invoke `/advisor` Opus 4.8 when the task needs stronger reasoning, second-opinion review, or architectural escalation

## Executor Lane D — Codex IDE

- Codex handles the prompt directly
- Best for precise code surgery, debugging, refactors, disciplined patches, and fixing messy or subtle implementation problems

## Operating Model

- GPT-5.5 chooses the executor lane and writes the bounded mission prompt.
- If Lane A is chosen, GPT-5.5 chooses the Gemini model to handle the task directly inside Antigravity IDE.
- If Lane B is chosen, OpenCode’s Qwen3.7 Plus manager may route internally to its configured open-source specialist agents.
- If Lane C is chosen, Claude Sonnet 5 handles the task directly inside Claude Code; `/advisor` invokes Claude Opus 4.8 for advisory escalation.
- If Lane D is chosen, Codex handles the task directly inside Codex IDE.
- Ian reviews results, approves commits/pushes, and ratifies any source-of-truth changes.

## Short Rule

GPT-5.5 chooses the lane, following the preferred hierarchy:
Gemini first when sufficient, then OpenCode, then Claude Code, then Codex when precision/debugging is the better fit.

OpenCode only routes inside Lane B.
Claude Code uses Sonnet 5 directly, with /advisor Opus 4.8 for escalation.
Ian ratifies what becomes part of the project.
