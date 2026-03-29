# Pipeline Documentation Agent — Kiro CLI Setup Guide

This guide walks you through deploying the Pipeline Documentation Agent on Kiro CLI. The system uses two agents — an Analyst Agent (`pipeline-analyst-agent-prompt.md`) and a Reviewer Agent (`pipeline-reviewer-agent-prompt.md`). This file only covers platform-specific setup; all analysis and review logic lives in the core prompt files.

## Setup

### 1. Deploy the Analyst Agent via Steering File

The Analyst Agent is the primary user-facing agent. It handles pipeline code ingestion, multi-pass analysis, documentation generation, and orchestrates the review loop.

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `pipeline-analyst-agent-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/pipeline-analyst-agent.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the Analyst Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/pipeline-analyst-agent.md` file.

### 2. Deploy the Reviewer Agent Prompt File

The Reviewer Agent is **not** a user-facing steering file. It is invoked programmatically by the Analyst Agent via `invokeSubAgent`. You only need to ensure the prompt file is accessible in your project.

1. Place `pipeline-reviewer-agent-prompt.md` in the `PipelineDocumentation/` directory of your project (it should already be there if you cloned the repository).
2. Verify the file path: `PipelineDocumentation/pipeline-reviewer-agent-prompt.md`.
3. No steering file creation is needed — the Analyst Agent references this file directly when calling `invokeSubAgent`.

> Do **not** place the Reviewer prompt in `.kiro/steering/`. The Reviewer is a subagent, not a user-facing persona.

### 3. `invokeSubAgent` Configuration

The Analyst Agent calls the Reviewer Agent programmatically using Kiro's `invokeSubAgent` mechanism. The review loop runs automatically within the Analyst's execution — no manual user intervention is required for the handoff.

**How it works:**

- The Analyst Agent uses `invokeSubAgent` to call the Reviewer Agent as a subagent.
- The `prompt` parameter carries the Pipeline_Code and the current Transformation_Documentation — these are the **only** inputs the Reviewer receives.
- The `contextFiles` parameter points to the Reviewer Agent prompt file (`PipelineDocumentation/pipeline-reviewer-agent-prompt.md`), which provides the Reviewer's identity, methodology, and formatting instructions.
- The Reviewer runs as a separate agent execution with its own isolated context. It has no access to the Analyst's conversation history, intermediate reasoning, or working notes.
- The Analyst parses the Reviewer's subagent response to extract findings and the Confidence_Score, makes corrections if needed, and re-invokes the Reviewer until convergence (Confidence_Score ≥ 95% with zero errors/omissions) or the 5-round cap is reached.

**Conceptual invocation pattern:**

```
invokeSubAgent({
  name: "pipeline-reviewer",
  prompt: "<Pipeline_Code>\n\n<Transformation_Documentation>",
  contextFiles: [
    { path: "PipelineDocumentation/pipeline-reviewer-agent-prompt.md" }
  ]
})
```

**Context isolation guarantee:** Because the Reviewer runs as a separate subagent execution, it physically cannot see the Analyst's conversation history, draft notes, or assumption rationale. This is enforced by architecture, not by prompt instructions.

## Platform Notes

### Terminal-Based Output

Kiro CLI runs in a terminal environment. Keep these considerations in mind:

- Output is rendered as plain text in a terminal. Standard markdown (headers, bold, lists, code blocks, tables) is supported, but complex visual formatting will not render well.
- The Transformation_Documentation includes tables (Source Tables, Data Lineage, Output Schema, Review_Log). Keep table columns concise to avoid line wrapping in narrow terminals.
- Avoid relying on wide tables, nested indentation beyond 2–3 levels, or horizontal rules for visual structure. Prefer numbered lists and short paragraphs.
- The conversational state markers (`[PIPELINE OVERVIEW]`, `[TRANSFORMATION ANALYSIS]`, `[LINEAGE]`, `[DOCUMENTATION]`, `[ANALYST PHASE]`, `[REVIEWER PHASE]`, `[REVIEW ROUND N]`, `[VALIDATION REVIEW]`, `[REVIEW LOG]`, `[SUMMARY]`) display as plain text, which works well in a terminal context.
- Keep code block examples concise — long code blocks are harder to read in a terminal without scrollback.

### Context Window Considerations

- Pipeline analysis sessions can grow large, especially for pipelines exceeding 300 lines. The Analyst Agent will proactively offer session summaries when the conversation grows long.
- The review loop adds to context usage — each round involves invoking the Reviewer and processing its response. For very large pipelines, the 5-round cap helps bound total context consumption.
- If responses start losing coherence or referencing earlier analysis incorrectly, ask for a `[SUMMARY]` and start a new session with the recap.

### Session Persistence

- Each Kiro CLI chat session is independent. The steering file is loaded fresh each time.
- Use the `[SUMMARY]` session summary to carry context across sessions. Paste a previous summary into a new session to restore context and continue analysis.
- The Reviewer Agent is stateless per invocation — it has no memory of prior review rounds. The Analyst Agent manages round continuity through the Review_Log.

## Limitations

- **Context window**: Long analysis sessions with large pipelines and multiple review rounds may approach the model's context limit. Use session summaries to manage this.
- **Session independence**: Each Kiro CLI chat session starts fresh. The Analyst steering file is reloaded, and the Reviewer has no cross-session memory. Session summaries are the only mechanism for continuity.
- **Scrollback**: Terminal scrollback buffers vary. For long documentation outputs, request a `[SUMMARY]` or specific sections rather than scrolling back through the full history.
- **Review loop automation**: The `invokeSubAgent` mechanism handles the Analyst–Reviewer handoff automatically, but the review loop's effectiveness depends on the model's ability to parse and act on the Reviewer's findings within the Analyst's context.

## What This Wrapper Does NOT Do

This setup guide does not modify the Analyst Agent's analysis behavior, persona, methodology, or interaction patterns. It does not modify the Reviewer Agent's review behavior, finding categorization, or Confidence_Score logic. All analysis logic lives in the Analyst core prompt (`pipeline-analyst-agent-prompt.md`), and all review logic lives in the Reviewer core prompt (`pipeline-reviewer-agent-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
