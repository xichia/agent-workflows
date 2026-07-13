# Cross-Model Exchange Workflow for Agentic Coding

This workflow defines an experimental operating model for repository work using three primary tools:

1. **GPT-5.6 Sol in the browser** as the strategic project/model manager.
2. **Claude Code** as the normal repository-aware executor.
3. **Codex** as a targeted third lane for selective, bounded work.

This is a managed exchange between two model families, not model switching for its own sake. The purpose is productive separation between project management and repository execution, between initial implementation and independent challenge, and between model families with potentially different strengths and blind spots.

```text
GPT manages. Claude executes by default. GPT reviews the evidence. Claude continues or Codex performs a targeted intervention. The human operator ratifies what becomes part of the project.
```

---

## 1. Operating Roles

### Human operator

The human operator is the final ratifier.

The human operator:

- defines the desired outcome and supplies available context;
- launches the assigned executor with the assigned configuration and prompt;
- reviews important diffs and validation evidence;
- approves source-of-truth changes;
- controls commits, pushes, dependency changes, migrations, permissions, and other high-impact actions.

### GPT-5.6 Sol project/model manager

GPT-5.6 Sol in the browser is the normal strategic project/model manager for this workflow.

It:

- clarifies goals, scope, risks, and approval boundaries;
- chooses the repository executor and exact model;
- writes bounded, paste-ready executor prompts;
- reviews reports, diffs, tests, logs, and command output supplied by the executor or human operator;
- identifies contradictions, unsupported claims, missing validation, and governance risks;
- decides whether Claude should continue, another Claude model should take over, or Codex should receive a targeted assignment;
- preserves continuity across execution passes.

The project manager must not claim live repository knowledge unless that state is available through uploaded files, connected tools, command output, diffs, logs, or executor reports.

### Repository-aware executor

Claude Code or, selectively, Codex performs the live repository work.

The executor may:

- inspect files and repository structure;
- plan within the assigned scope;
- edit files;
- run commands and tests;
- inspect its diff;
- correct defects;
- report exact evidence and remaining uncertainty.

Source files, runtime behavior, tests, diffs, and git state outrank model summaries.

---

## 2. GPT-5.6 Sol Browser Configuration

The normal browser configuration is:

- **GPT-5.6 Sol High** — the normal default for project management in this workflow. Use it for executor selection, assignment design, evidence review, cross-model reconciliation, and normal project-management decisions.
- **GPT-5.6 Sol Medium** — a deliberate efficiency mode, not the normal default. Step down to Medium for routine, low-ambiguity coordination, such as mechanical follow-ups, where extended reasoning is unlikely to add material value.
- **GPT-5.6 Sol Extra High** — a rare escalation for unusually consequential, ambiguous, or conflicting decisions, including source-of-truth, architecture, migration, permissions, and governance issues.

High is the default because this workflow deliberately places substantial coordination and review responsibility on the GPT project manager: it continuously selects executors, reconciles evidence across model families, and decides when a targeted Codex intervention is warranted. Medium remains available as an efficiency step-down, not a default. Extra High should not become automatic — select it deliberately, not routinely.

The project manager's browser configuration is separate from the model configuration assigned to the repository executor.

---

## 3. Default Execution Policy

Use **Claude Code as the normal repository-aware executor** whenever live repository work is required. The project manager chooses positively among Sonnet 5, Opus 4.8, and Fable 5 rather than treating them as a rigid escalation ladder.

Use **Codex as a targeted third lane** when a bounded intervention by a different model family would add material value: independent verification, a surgical fix, a challenge to a Claude implementation or diagnosis, or a task that specifically suits Codex's tools or execution environment.

The default policy is:

- **GPT-5.6 Sol** manages the task.
- **Claude Code** performs repository work by default.
- **Codex** is assigned selectively, for a concrete reason tied to the task.

Avoid unnecessary handoffs, duplicate repository reading, context loss, and model changes that do not add material value.

---

## 4. Claude Code: Primary Repository Executor

Claude Code is the normal first consideration whenever live repository work is required.

Available models:

- **Sonnet 5**
- **Opus 4.8**
- **Fable 5**

### Sonnet 5

Use as the normal default for:

- routine repository inspection;
- bounded implementation;
- documentation and configuration work;
- normal debugging;
- tests and validation;
- iterative coding;
- clear cross-file changes.

### Opus 4.8

Use when the task materially benefits from stronger sustained reasoning, including:

- difficult implementation;
- complex debugging;
- architecture-aware refactoring;
- broad interactions across subsystems;
- ambiguous requirements;
- work where a shallow implementation is likely to create rework.

A task may start with Opus 4.8 immediately when its complexity clearly warrants it. It need not fail with Sonnet 5 first.

### Fable 5

Use selectively for unusually demanding repository missions, including:

- long-horizon agentic work;
- high-consequence architecture or implementation;
- source-of-truth-sensitive changes;
- complex migrations or governance changes;
- difficult review requiring strong judgment;
- cases where the project manager needs the strongest available Claude model.

Fable prompts should remain bounded and token-conscious. Instruct it to inspect only the evidence needed, avoid unnecessary repository reading, and report material conclusions rather than restating context:

```text
Keep this review concise and token-conscious. Do not perform unnecessary repository reading. Inspect only the files and evidence required to answer the question, avoid restating context, and report only material conclusions, risks, disagreements, and recommendations.
```

Fable 5 is not the routine implementation default.

---

## 5. Codex: Targeted Third Lane

Codex is not the normal first executor in this workflow. It is a deliberate alternate execution lane with a narrower role, not a fallback for when Claude fails.

Use it when a bounded intervention by a GPT repository executor would add material value, such as:

- independently reproducing a bug;
- challenging a Claude implementation or diagnosis;
- reviewing a risky diff from a different model family;
- performing a surgical code change;
- generating or strengthening targeted tests;
- checking a migration or permissions change;
- investigating contradictory evidence;
- resolving a narrow issue after Claude completed the broader mission;
- completing a task that specifically suits Codex's tools or execution environment.

Codex may also be selected directly for a tightly bounded task when using Claude would add unnecessary handoff overhead.

### Codex model and reasoning selection

Use the same Codex model and reasoning guidance as the [Core Hybrid Model Workflow](core-hybrid-model-workflow.md#4-codex-default-repository-executor) to select the exact configuration for a targeted assignment:

- **GPT-5.6 Luna** — fast and economical; small or mechanical work.
- **GPT-5.6 Terra** — balanced everyday engineering model.
- **GPT-5.6 Sol** — strongest option for complex, ambiguous, or high-consequence engineering work.

Pair the model with a reasoning level (Low, Medium, High, Extra High, Max, or Ultra) matched to the size and risk of the targeted task. For a typical targeted verification or surgical task, start by considering **Terra + Medium**; move to a **Sol** configuration when the task is genuinely complex or high-consequence.

---

## 6. Managed Exchange Loop

The normal flow:

1. The human operator defines the desired outcome and supplies available context.
2. GPT-5.6 Sol identifies risks, unknowns, evidence requirements, and approval gates.
3. GPT-5.6 Sol selects Sonnet 5, Opus 4.8, Fable 5, or targeted Codex.
4. GPT-5.6 Sol provides a complete assignment packet and bounded mission prompt.
5. The human operator launches the repository-aware executor.
6. The executor inspects, edits, validates, reviews its diff, and reports.
7. GPT-5.6 Sol reviews the returned evidence.
8. GPT-5.6 Sol chooses among:
   - accepting the result for human ratification;
   - issuing a follow-up to the same Claude model;
   - switching to another Claude model because the task has changed;
   - assigning Codex a specific independent or surgical task;
   - requesting missing evidence before more editing.
9. The human operator ratifies source-of-truth changes and controls commits and pushes.

A handoff must have a concrete purpose. A downstream executor should receive:

- the current objective;
- relevant findings;
- exact files or evidence already examined;
- decisions already made;
- unresolved questions;
- what it must independently verify;
- what it must not redo unnecessarily.

---

## 7. Cross-Model Review Patterns

Not every task needs every model. These patterns are illustrative, not mandatory.

### Normal implementation

```text
GPT-5.6 Sol manager
→ Claude Code Sonnet 5 implementation
→ GPT-5.6 Sol evidence review
→ human ratification
```

### Difficult implementation

```text
GPT-5.6 Sol manager
→ Claude Code Opus 4.8 implementation
→ GPT-5.6 Sol review
→ focused Opus or Sonnet follow-up
→ human ratification
```

### Independent challenge

```text
GPT-5.6 Sol manager
→ Claude Code implementation
→ GPT-5.6 Sol identifies a material uncertainty
→ Codex receives a narrow independent verification task
→ GPT-5.6 Sol reconciles the evidence
→ human ratification
```

### High-consequence work

```text
GPT-5.6 Sol High or Extra High manager
→ Claude Code Fable 5 bounded mission or review
→ targeted Codex verification where useful
→ GPT-5.6 Sol reconciles conclusions and evidence
→ human ratification
```

---

## 8. Assignment Packet

For every repository task, the project manager should provide a complete assignment packet:

```text
Executor assignment

Project manager: GPT-5.6 Sol <High, Medium, or Extra High>
Executor: <Claude Code or Codex>
Environment: <CLI, IDE, app, or other surface>
Model/configuration: <exact selection>
Repository: <repository or working directory>
Reason for selection: <task-specific reason>
Purpose of this pass: <implementation, investigation, review, or targeted verification>
Context from previous pass: <only when applicable>
Approval gates: <actions requiring human approval>
Expected validation: <tests, checks, or evidence>
Commit permission: <allowed or not allowed>
Push permission: <allowed or not allowed>

Mission prompt

<bounded, paste-ready prompt>
```

Where the executor is Codex, include its exact model and reasoning level, matching the current available interface.

---

## 9. Governance

Always preserve these rules:

- The human operator is the final ratifier.
- Review the diff before committing.
- Keep unrelated cleanup separate.
- Do not change dependencies, permissions, migrations, public contracts, or source-of-truth files without required approval.
- Do not commit unless explicitly authorized.
- Do not push unless explicitly authorized.
- Never force-push.
- Do not amend or rewrite unrelated history.
- Verify the branch, remote, staged files, and intended commit before pushing.
- Public documentation must not expose private paths, secrets, tokens, or local-only configuration.
- Executors must report exact files changed, commands run, validation results, unresolved risks, commit state, and final git status.
- Repository evidence outranks model summaries.

---

## 10. Relationship to Existing Workflows

This is an alternative operating model, not a replacement for the [Core Hybrid Model Workflow](core-hybrid-model-workflow.md).

- **Core Hybrid Model Workflow** — Codex is the normal repository executor; Claude Code is selected for a specific Claude advantage.
- **Cross-Model Exchange Workflow** — Claude Code is the normal repository executor; Codex is a targeted third lane; GPT-5.6 Sol continuously manages and reviews the exchange.

Both workflows share the same governance rules and assignment-packet discipline. Choose the workflow that fits the task and the tooling available, not both at once.

---

## Short Rule

```text
GPT manages. Claude executes by default. GPT reviews the evidence. Claude continues or Codex performs a targeted intervention. The human operator ratifies what becomes part of the project.
```
