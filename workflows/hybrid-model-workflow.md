# Hybrid Model Workflow for Agentic Coding

This document describes how to choose between available coding-agent executors and how OpenCode, Claude Code, Codex, Gemini, GPT-5.5, and the human operator fit into the overall workflow.

The model names below are example assignments. Adapt them to whatever subscriptions and model availability you actually have.

Human operator = final ratifier  
GPT-5.5 in browser = strategic project/model manager

## Executor Lane A — Gemini directly inside Antigravity IDE

- Gemini 3.5 Flash (Low, Medium, High)
- Gemini 3.1 Pro (Low, High)
- Best for direct IDE execution, quick implementation

## Executor Lane B — OpenCode

- Local OpenCode manager: Qwen 3.7 Plus
- Routes internally to configured specialist agents
- Best for multi-step routed work: inspect → factual handoff → plan/escalate if needed → implement → debug if needed → review → report
- For non-trivial repo/code work, the manager normally uses Scout first for a compact factual handoff before implementation or escalation
- Tiny docs, tiny targeted fixes, and already well-scoped one-file tasks may skip Scout when discovery would add overhead

### OpenCode internal model roles

- `manager` — Manager / router / planning: Qwen 3.7 Plus
- `scout` — Scout / factual repo handoff: DeepSeek V4 Flash
- `escalation-planner` — Escalation planning / complex planning: GLM 5.2
- `architect` — Architecture review / design validation: GLM 5.2
- `consultant` — Consultant / high-level second opinion: Qwen 3.7 Max
- `implementer` — Implementation: Kimi K2.7 Code
- `fixer` — Debugging / failing tests / runtime contradictions: DeepSeek V4 Pro
- `reviewer` — Final review: DeepSeek V4 Pro
- `docs` — Docs / cleanup: MiniMax M3

### OpenCode routing and safety notes

- Scout provides factual handoffs only; it does not perform broad strategy or edit files.
- Escalation planner handles unclear, cross-cutting, source-of-truth-sensitive, migration/security, or public-contract work before implementation.
- Architect validates design shape, data models, abstractions, state-machine changes, compatibility, and blast-radius risk.
- Consultant is manual opt-in only for high-level second opinions and is not part of default routing.
- Reviewer performs the final read-only diff review before completion, except for trivial single-file mechanical changes with explicit justification.
- Human approval is required for pushes, dependency modifications, destructive git operations, and complex plans before coding. Force-pushes are blocked.
- Runtime/browser/playtest behavior overrides model claims.

For full OpenCode setup, config, permissions, and prompt templates, see [OpenCode Go Router Workflow: Manager and Specialist Agents](opencode-go-router-workflow.md).

## Executor Lane C — Claude Code

- Claude Sonnet 5 handles the prompt directly in Claude Code
- `/advisor` uses Claude Opus 4.8 for higher-level review, strategy, architecture critique, and difficult judgment calls
- Best for agentic coding sessions where Claude Code can inspect, edit, test, and iterate directly
- Use Sonnet 5 for normal implementation flow; invoke `/advisor` Opus 4.8 when the task needs stronger reasoning, second-opinion review, or architectural escalation

## Executor Lane D — Codex IDE

- Codex handles the prompt directly
- Best for precise code surgery, debugging, refactors, disciplined patches, and fixing messy or subtle implementation problems

## Operating Model

- GPT-5.5 chooses the executor lane and writes the bounded mission prompt. The human operator then copies that prompt into the relevant IDE or model environment for the selected lane.
- If Lane A is chosen, GPT-5.5 chooses the Gemini model to handle the task directly inside Antigravity IDE.
- If Lane B is chosen, OpenCode’s Qwen 3.7 Plus manager routes internally to its configured specialist agents.
- If Lane C is chosen, Claude Sonnet 5 handles the task directly inside Claude Code; `/advisor` invokes Claude Opus 4.8 for advisory escalation.
- If Lane D is chosen, Codex handles the task directly inside Codex IDE.
- The human operator reviews results, approves commits/pushes, and ratifies any source-of-truth changes.

## Short Rule

GPT-5.5 chooses the lane, following the preferred hierarchy:
Gemini first when sufficient, then OpenCode, then Claude Code, then Codex when precision/debugging is the better fit.

The human operator ratifies what becomes part of the project.
