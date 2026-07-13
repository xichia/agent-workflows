# Hybrid Model Workflow for Agentic Coding

This document defines the operating model for the hybrid-model agentic coding workflow, establishing the boundaries between the human operator, the strategic project manager, and the repository-aware executors.

## Role Boundaries

### The Human Operator
- **Role:** The final ratifier.
- **Responsibilities:** Reviews assignment packets, manually selects the browser model when required, executes the assigned prompt in the appropriate lane, and ratifies what becomes part of the project.

### ChatGPT in the Browser
- **Role:** The strategic project/model manager.
- **Capabilities:** The project manager can do more than write prompts. It can:
  - research current external facts when needed;
  - inspect supplied files, diffs, logs, reports, and command output;
  - compare executor outputs;
  - identify contradictions and missing evidence;
  - coordinate multi-stage work;
  - prepare supporting artifacts;
  - define approval gates;
  - review validation evidence;
  - decide the next executor, model, and reasoning level.
- **Important Boundary:** The project manager must not claim live repository knowledge unless that state is available through uploaded files, connected tools, command output, diffs, logs, or executor reports.

### Repository-Aware Executors
- **Role:** File inspection, editing, and execution.
- **Responsibilities:** Executors (such as Gemini, OpenCode, Claude Code, and Codex) inspect files, edit, run commands, validate, and report back to the project manager.
- **Hierarchy of Truth:** Source files, runtime behavior, tests, and git evidence outrank model summaries.

## Seven-Step Operating Model

1. **Review:** The project manager reviews the task, context, and external facts.
2. **Lane Selection:** The project manager decides the executor lane, model, reasoning level, and validation requirements based on task fit (the rigid lane hierarchy has been removed in favor of task-fit selection).
3. **Assignment:** The project manager provides the human operator with a clear assignment packet (including the bounded prompt and commit/push permissions).
4. **Execution:** The human operator reviews the assignment packet, confirms the browser config, and executes the bounded prompt in the assigned lane.
5. **Implementation:** The selected repository-aware executor inspects files, implements the changes, validates, and generates a final report.
6. **Verification:** The project manager reviews the executor's final report, diffs, and validation evidence.
7. **Ratification:** The human operator acts as the final ratifier, approving commits, blocking force-pushes, and ratifying source-of-truth changes.

## ChatGPT Browser Configuration

The project manager's capabilities in the browser are configured as follows:

- **Instant 5.5:** The normal default for routine coordination, executor selection, prompt writing, straightforward report review, and low-risk governance decisions.
- **Automatic GPT-5.6 Sol Medium:** Allow the default experience to escalate automatically when the task needs more reasoning but does not justify deliberate manual escalation. (Do not manually select Medium; this is an automatic behavior of the Instant experience.)
- **GPT-5.6 Sol High:** Manually select for difficult architecture or workflow decisions, conflicting executor evidence, permissions and governance design, source-of-truth-sensitive changes, complex migrations, or high-consequence review.

*Note: Do not list Extra High or Pro as browser project-manager options.*

When useful in documentation or workflow descriptions, identify the browser configuration as one of:
- `ChatGPT browser: Instant 5.5, with automatic escalation to GPT-5.6 Sol Medium`
- `ChatGPT browser: GPT-5.6 Sol High`

Keep the distinction clear between the ChatGPT browser configuration used by the project manager and the separate model and reasoning configuration assigned to a repository executor.

## Executor Assignment Packets

The project manager must provide the human operator with a clear executor assignment containing:
- Executor lane
- Environment or surface
- Exact model
- Reasoning or thinking level, when available
- Why the configuration fits
- Target repository or directory
- Approval gates
- Expected validation
- Commit and push permissions
- A paste-ready bounded prompt

The project manager should assign the executor configuration explicitly rather than leaving the human operator to infer it.

## Executor Lanes

The four lanes are presented in the normal repository-executor order: Codex, Claude Code, OpenCode, then Gemini. This presentation order is not a rigid escalation hierarchy; select the lane that best fits the task.

### Executor Lane A — Codex
Codex is a full repository-aware engineering lane capable of end-to-end implementation rather than only a debugging/code-surgery lane.

**Preferred Codex Models:**
- **GPT-5.6 Sol**
- **GPT-5.6 Terra**
- **GPT-5.6 Luna**

Codex model selection and reasoning effort are treated as separate choices.

**Codex Reasoning Levels:**
- **Low**
- **Medium**
- **High**
- **Extra High**
- **Max:** The strongest single-agent reasoning level.
- **Ultra:** Potentially uses subagents and is only appropriate when useful decomposition or parallel investigation exists.

**Codex Selection Guidance:**
The project manager must explicitly provide the exact Codex model and reasoning level before the prompt. Use the following guidance based on task fit:
- *GPT-5.6 Luna (Low/Medium):* Quick code surgery, straightforward mechanical refactors.
- *GPT-5.6 Terra (Medium/High):* Standard feature implementation, disciplined patches, debugging.
- *GPT-5.6 Sol (High/Extra High/Max):* Difficult architecture problems, messy implementations, subtle bug fixing, complex refactoring.
- *GPT-5.6 Sol (Ultra):* Large-scale tasks or deep investigations where parallel subagent decomposition adds value.

**Codex Assignment and Final Report Requirements:**
- The project manager must explicitly assign the exact model and reasoning level in the assignment packet.
- The executor's final report must document: exactly what was changed, execution of validation commands, and whether the implementation meets the assignment's explicit approval gates.

### Executor Lane B — Claude Code
Claude Code provides selectable models that the project manager may assign directly:

- **Sonnet 5**: Normal repository inspection, implementation, testing, and iterative coding.
- **Opus 4.8**: Difficult implementation, complex debugging, architecture-aware work, or stronger sustained reasoning.
- **Fable 5**: High-level judgment, strategy, architecture critique, source-of-truth-sensitive review, or a focused second opinion.

**Fable 5 Instructions:**
When Fable 5 is selected, the project manager must write a concise and tightly bounded prompt that tells it to:
- avoid unnecessary repository reading;
- inspect only the files and evidence needed to answer the question;
- remain token-conscious during investigation and output;
- avoid restating large amounts of repository context;
- return only material conclusions, risks, disagreements, and recommendations.

Include this reusable instruction in the Fable 5 prompt:
```text
Keep this review concise and token-conscious. Do not perform unnecessary repository reading. Inspect only the files and evidence required to answer the question, avoid restating context, and report only material conclusions, risks, disagreements, and recommendations.
```

### Executor Lane C — OpenCode
OpenCode is best when the task benefits from explicit specialist routing, a cheap factual Scout handoff, separate implementation and review roles, bounded escalation, and structured final reporting.

*Note: The browser project manager does not directly route OpenCode specialists; it writes the routing contract, and the OpenCode manager performs the internal routing.*

**Internal Model Roles:**
- `manager` — Qwen3.7 Plus
- `scout` — DeepSeek V4 Flash
- `escalation-planner` — GLM 5.2
- `architect` — GLM 5.2
- `consultant` — Qwen3.7 Max (Manual opt-in only)
- `implementer` — Kimi K2.7 Code
- `fixer` — DeepSeek V4 Pro
- `reviewer` — DeepSeek V4 Pro
- `docs` — MiniMax M3

**Routing Model:**
- Default route: `manager -> scout -> implementer -> reviewer`
- Downstream specialist briefs depending on discovery must include a section titled: `Context from Scout`
- Keep escalation-planner before architect when both may be needed.

### Executor Lane D — Gemini
Gemini is a direct IDE lane suited to clear, bounded implementation, documentation changes, test updates, mechanical refactors, low- to moderate-risk repository work, and tasks where direct IDE execution is more useful than multi-agent routing.

Available configurations:
- **Gemini 3.5 Flash** (Low, Medium, High): Use when speed and economy matter and the task is straightforward.
- **Gemini 3.1 Pro** (Low, High): Use High for more demanding direct IDE work requiring stronger judgment, broader context, or careful document restructuring.
