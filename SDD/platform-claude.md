# SDD Agent — Claude Setup Guide

This guide walks you through deploying the SDD Agent on Claude. The core prompt lives in `sdd-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Projects (Persistent Agent)

1. Open [Claude](https://claude.ai/).
2. In the left sidebar, click **Projects** → **Create a Project**.
3. Give it a name, e.g. "SDD Agent".
4. Open `sdd-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Project Instructions** field (under the project's settings).
6. Save the project.
7. To start a session, open the project and begin a new conversation.

> Project instructions apply to every conversation within that project. Your regular Claude conversations remain unaffected.

### Option B: Direct System Message (Quick Start)

1. Open [Claude](https://claude.ai/) and start a new conversation.
2. Open `sdd-agent-prompt.md` and copy the entire contents.
3. Paste the full prompt as your first message, prefixed with: "Use the following as your system instructions for this conversation:"
4. Wait for Claude to acknowledge, then begin your SDD session.

> This approach works for one-off sessions. For repeated use, the Projects approach is more convenient since you do not need to re-paste the prompt each time.

## Platform Notes

### Instruction-Following

- Claude has strong instruction-following capabilities. The structured markers (`[REQUIREMENTS]`, `[DESIGN]`, `[TASKS]`, `[SESSION SUMMARY]`, `[BUG CONDITION]`) and the three-phase SDD methodology in the core prompt are reliably followed.
- The persona, Socratic questioning, EARS pattern enforcement, and iterative refinement patterns work well without additional reinforcement.

### Formatting Support

- Claude renders full markdown: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all work correctly.
- The conversational state markers (`[REQUIREMENTS]`, `[DESIGN]`, `[TASKS]`, `[SESSION SUMMARY]`, `[BUG CONDITION]`) display as plain text, which is the intended behavior.
- Mermaid diagrams and LaTeX are not used in the core prompt, keeping output portable across contexts.

### Limitations

- **Context window**: Long SDD sessions — especially those spanning multiple phases with extensive refinement — may approach Claude's context limit. If responses start losing coherence or referencing earlier phase documents incorrectly, ask for a `[SESSION SUMMARY]` and start a new conversation with the recap.
- **Session persistence**: Each conversation is independent. Project instructions carry over, but conversation history does not. Use session summaries to restore context in a new conversation.
- **Direct system message approach**: Pasting the prompt as a user message is less reliable than using Project instructions, since it occupies part of the context window and may be treated with slightly lower priority than true system instructions.

### What This Wrapper Does NOT Do

This setup guide does not modify the SDD Agent's behavior, methodology, output formatting, or interaction patterns. All agent logic lives in the core prompt (`sdd-agent-prompt.md`). This file only provides deployment instructions for the Claude platform.
