# Status Update Agent — Claude Setup Guide

This guide walks you through deploying the Status Update Agent on Claude. The core prompt lives in `status-update-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Projects (Persistent Agent)

1. Open [Claude](https://claude.ai/).
2. In the left sidebar, click **Projects** → **Create a Project**.
3. Give it a name, e.g. "Status Update Agent".
4. Open `status-update-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Project Instructions** field (under the project's settings).
6. Save the project.
7. To start a session, open the project and begin a new conversation.

> Project instructions apply to every conversation within that project. Your regular Claude conversations remain unaffected.

### Option B: Direct System Message (Quick Start)

1. Open [Claude](https://claude.ai/) and start a new conversation.
2. Open `status-update-agent-prompt.md` and copy the entire contents.
3. Paste the full prompt as your first message, prefixed with: "Use the following as your system instructions for this conversation:"
4. Wait for Claude to acknowledge, then begin your status update session.

> This approach works for one-off sessions. For repeated use, the Projects approach is more convenient.

## Platform Notes

### Instruction-Following

- Claude has strong instruction-following capabilities. The structured output format — section headers, owner fields, writing standards enforcement — is reliably followed.
- The three update types (Weekly Status Report, Quick Update, Sprint Summary) and iterative refinement patterns work well without additional reinforcement.

### Formatting Support

- Claude renders full markdown: headers, bold, italic, bullet lists, and fenced code blocks all work correctly.
- The structured status update output renders cleanly and is easy to scan.

### Limitations

- **Context window**: Long sessions with many rounds of refinement may approach Claude's context limit. If responses start losing coherence, ask for a session summary and start a new conversation with the recap.
- **Session persistence**: Each conversation is independent. Project instructions carry over, but conversation history does not. Use session summaries to restore context.
- **Direct system message approach**: Pasting the prompt as a user message is less reliable than using Project instructions, since it occupies part of the context window.

### What This Wrapper Does NOT Do

This setup guide does not modify the Status Update Agent's behavior, methodology, output formatting, or interaction patterns. All agent logic lives in the core prompt (`status-update-agent-prompt.md`). This file only provides deployment instructions for the Claude platform.
