# Core Hybrid Model Workflow for Agentic Coding

This workflow defines a simple operating model for repository work using three primary tools:

1. **ChatGPT in the browser** as the project/model manager.
2. **Codex** as the default repository-aware executor.
3. **Claude Code** as an alternate executor when a Claude model is the better fit.

OpenCode is not part of the default path. It may be introduced as a specialist workflow when explicit routing, role separation, or independent review would materially improve the task.

---

## 1. Operating Roles

### Human operator

The human operator is the final ratifier.

The human operator:

- defines the goal and supplies available context;
- launches the selected executor with the assigned configuration and prompt;
- reviews important diffs and validation evidence;
- approves source-of-truth changes;
- controls commits, pushes, dependency changes, migrations, and other high-impact actions.

### ChatGPT project/model manager

ChatGPT in the browser is the strategic project/model manager.

It:

- clarifies goals, constraints, and approval boundaries;
- researches current external facts when needed;
- inspects supplied files, diffs, logs, command output, and executor reports;
- chooses the executor, exact model, and reasoning level;
- writes a bounded, paste-ready mission prompt;
- compares outputs and identifies missing evidence or contradictions;
- reviews implementation reports and recommends the next step;
- may keep work with the same executor or switch executors when the evidence warrants it.

The project manager must not claim live repository knowledge unless that state is available through uploaded files, connected tools, command output, diffs, logs, or executor reports.

### Repository-aware executor

Codex or Claude Code performs the live repository work.

The executor:

- inspects the repository;
- reads the relevant files;
- plans within the assigned scope;
- edits files;
- runs commands, tests, and validation;
- reviews its diff;
- reports exactly what changed and what remains uncertain.

Source files, runtime behavior, tests, and git evidence outrank model summaries.

---

## 2. ChatGPT Browser Configuration

The normal browser configuration is:

- **Instant 5.5** by default.
- Instant 5.5 may automatically switch to **GPT-5.6 Sol Medium** when the task benefits from additional reasoning.
- The human operator may manually select **GPT-5.6 Sol High** for demanding project-management work.

Use the configurations as follows:

### Instant 5.5

Use for routine coordination, executor selection, prompt writing, straightforward report review, and low-risk governance decisions.

### Automatic GPT-5.6 Sol Medium

Allow the default experience to escalate automatically when the task needs more reasoning but does not justify deliberate manual escalation.

### GPT-5.6 Sol High

Select manually for difficult architecture or workflow decisions, conflicting executor evidence, permission and governance design, source-of-truth-sensitive work, complex migrations, or high-consequence review.

The project manager's browser configuration is separate from the model and reasoning configuration assigned to the repository executor.

---

## 3. Default Execution Policy

Use **Codex as the normal first consideration whenever live repository access is required**.

Start with the lightest Codex configuration likely to complete the task reliably. Escalate only when complexity, ambiguity, risk, or likely rework justifies it.

Use Claude Code instead when a particular Claude model is a materially better fit.

The default policy is:

- **ChatGPT** manages the task.
- **Codex** performs most repository work.
- **Claude Code** is selected for a specific Claude strength.
- **OpenCode** is introduced only for tasks that genuinely benefit from its specialist routing model.

There is no rigid executor hierarchy. The project manager may select a stronger configuration immediately when the task warrants it.

---

## 4. Codex: Default Repository Executor

Codex is the primary general-purpose repository-aware engineering executor.

It can own a complete bounded mission, including:

- repository orientation;
- targeted file inspection;
- implementation planning;
- code and documentation edits;
- command execution;
- tests and validation;
- diff inspection;
- iterative fixes;
- final reporting.

Codex should not be treated only as a debugging, refactoring, or code-surgery tool.

### Codex models

- **GPT-5.6 Luna** — fast and economical; best for small or mechanical work.
- **GPT-5.6 Terra** — balanced everyday engineering model.
- **GPT-5.6 Sol** — strongest option for complex, ambiguous, or high-consequence engineering work.

### Codex reasoning levels

- **Low**
- **Medium**
- **High**
- **Extra High**
- **Max**
- **Ultra**

Model choice and reasoning level are separate decisions.

- **Max** is the strongest single-agent reasoning level.
- **Ultra** may delegate work to subagents and should be used only when useful decomposition or parallel investigation exists.

The strongest settings should not be selected automatically.

### Recommended Codex configurations

| Configuration | Typical use |
|---|---|
| **Luna + Low** | Formatting, renames, tiny documentation edits, simple test updates, mechanical one-file changes |
| **Luna + Medium** | Small bounded tasks requiring limited inspection or judgment |
| **Terra + Medium** | Normal default for routine repository engineering |
| **Terra + High** | Tricky but contained implementation |
| **Sol + Medium** | Substantial cross-file work |
| **Sol + High** | Difficult debugging, broad refactors, or architecture-aware implementation |
| **Sol + Extra High** | Highly ambiguous or high-consequence work |
| **Sol + Max** | The hardest single-agent repository tasks |
| **Sol + Ultra** | Large, decomposable work that clearly benefits from subagents |

For a typical non-trivial repository task, start by considering **Terra + Medium**.

For genuinely mechanical work, prefer **Luna + Low** rather than moving the task to another executor solely because it is small.

### Required Codex assignment

Before giving the prompt, the project manager must tell the human operator:

- the Codex surface or environment;
- the exact model;
- the exact reasoning level;
- why that pairing fits;
- the target repository or working directory;
- commit and push permissions.

Example:

```text
Executor assignment

Environment: Codex IDE
Model: GPT-5.6 Terra
Reasoning: Medium
Repository: <repository or working directory>
Reason for selection: Routine bounded repository work requiring inspection, edits, tests, and diff review.
Commit permission: Not allowed
Push permission: Not allowed

Paste the bounded mission prompt below.
```

Every Codex prompt should define:

- desired outcome;
- files or areas to inspect first;
- source-of-truth and edit boundaries;
- planning expectations;
- required tests and validation;
- dependency, permission, migration, and public-contract boundaries;
- commit permission;
- push restrictions;
- final-report requirements.

The final report should include:

- files changed;
- commands run;
- validation results;
- unresolved risks;
- subagent use, if any;
- commit status;
- final concise `git status --short`.

---

## 5. Claude Code: Alternate Executor

Claude Code is used when one of its available models is a better fit than Codex.

Available models:

- **Sonnet 5**
- **Opus 4.8**
- **Fable 5**

### Sonnet 5

Use for normal repository inspection, implementation, testing, and iterative coding when Claude Code is preferred.

### Opus 4.8

Use for difficult implementation, complex debugging, architecture-aware work, or tasks requiring stronger sustained reasoning.

### Fable 5

Use for high-level judgment, strategy, architecture critique, source-of-truth-sensitive review, or a focused second opinion.

Fable 5 should not be used for routine repository scanning or ordinary implementation when Sonnet 5, Opus 4.8, or Codex is the better fit.

When Fable 5 is selected, the project manager must write a concise and tightly bounded prompt. Include instructions like:

```text
Keep this review concise and token-conscious. Do not perform unnecessary repository reading. Inspect only the files and evidence required to answer the question, avoid restating context, and report only material conclusions, risks, disagreements, and recommendations.
```

A Fable 5 assignment should:

- identify the exact judgment required;
- provide only the necessary context;
- limit reading to the relevant files or evidence;
- request material conclusions rather than broad summaries;
- avoid repeated advisory passes unless new evidence changes the question.

---

## 6. Choosing Between Codex and Claude Code

Choose **Codex** when:

- the task requires normal repository discovery, editing, commands, tests, and review;
- the work is mechanical, routine, or substantial but bounded;
- explicit model and reasoning selection is useful;
- the task benefits from Codex Ultra subagents.

Choose **Claude Code** when:

- Sonnet 5 is the preferred direct coding executor;
- Opus 4.8 is better suited to sustained complex reasoning;
- Fable 5 is needed for a concise strategic or architectural judgment;
- prior task context or continuity makes Claude Code the more effective environment.

Choosing Claude Code over Codex should be based on a positive task-specific reason, not because Codex appears later in a list.

---

## 7. Assignment Packet

For every repository task, the project manager should provide a complete assignment packet containing:

```text
Executor assignment

Executor: <Codex or Claude Code>
Environment: <IDE, CLI, or other surface>
Model: <exact model>
Reasoning level: <exact level, when available>
Repository: <repository or working directory>
Reason for selection: <brief task-specific rationale>
Approval gates: <what requires human approval>
Expected validation: <tests, commands, or checks>
Commit permission: <allowed or not allowed>
Push permission: <allowed or not allowed>

Mission prompt

<bounded, paste-ready executor prompt>
```

The prompt should make clear:

- what success looks like;
- what is in scope;
- what is out of scope;
- what evidence should be inspected first;
- what may be edited;
- what must not be changed without approval;
- what validation is required;
- what the final report must contain.

---

## 8. Standard Operating Flow

1. The human operator defines the goal and supplies available context.
2. ChatGPT evaluates the task and identifies risks, unknowns, and approval boundaries.
3. ChatGPT selects the executor, exact model, and reasoning level.
4. ChatGPT gives the human operator a complete assignment packet and paste-ready prompt.
5. The human operator launches the repository-aware executor.
6. The executor inspects, edits, validates, reviews its diff, and reports.
7. ChatGPT reviews the evidence and recommends the next step.
8. The human operator ratifies source-of-truth changes and controls commits and pushes.

ChatGPT may issue a follow-up prompt to the same executor or switch executors when the result exposes new complexity, contradictions, or risk.

---

## 9. Governance

Always preserve these rules:

- Review the diff before committing.
- Keep unrelated cleanup separate.
- Do not change dependencies, permissions, migrations, public contracts, or source-of-truth files without the required approval.
- Do not commit unless the assignment explicitly allows it.
- Do not push unless the human operator explicitly instructs it.
- Never force-push.
- Do not expose private paths, secrets, tokens, or machine-specific configuration in public documentation.
- Report exactly what changed and what validation was performed.
- Source files and runtime evidence beat acceleration maps and model summaries.

---

## 10. Optional OpenCode Escalation

OpenCode is not part of the normal core workflow.

Introduce it when the task materially benefits from:

- a factual Scout handoff;
- explicit separation between discovery, implementation, debugging, and review;
- bounded escalation to planning or architecture specialists;
- an independent final reviewer;
- structured multi-agent routing.

When OpenCode is selected, use the dedicated OpenCode router workflow rather than duplicating its full role and model configuration here.

---

## Short Rule

```text
ChatGPT manages. Codex executes by default. Claude Code is selected for a specific Claude advantage. OpenCode is optional specialist escalation. The human operator ratifies what becomes part of the project.
```
