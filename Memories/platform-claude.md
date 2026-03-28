# Memory Email Agent — Claude Setup Guide

This guide walks you through deploying the Memory Email Agent on Claude. The core prompt lives in `memory-email-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Projects (Persistent Agent)

1. Open [Claude](https://claude.ai/).
2. In the left sidebar, click **Projects** → **Create a Project**.
3. Give it a name, e.g. "Memory Email Agent".
4. Open `memory-email-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Project Instructions** field (under the project's settings).
6. Save the project.
7. To start a session, open the project and begin a new conversation.

> Project instructions apply to every conversation within that project. Your regular Claude conversations remain unaffected.

### Option B: Direct System Message (Quick Start)

1. Open [Claude](https://claude.ai/) and start a new conversation.
2. Open `memory-email-agent-prompt.md` and copy the entire contents.
3. Paste the full prompt as your first message, prefixed with: "Use the following as your system instructions for this conversation:"
4. Wait for Claude to acknowledge, then begin your memory capture session.

> This approach works for one-off sessions. For repeated use, the Projects approach is more convenient since you do not need to re-paste the prompt each time.

## Platform Notes

### Instruction-Following

- Claude has strong instruction-following capabilities. The structured markers (`[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`) and the multi-phase conversation flow in the core prompt are reliably followed.
- The persona, multiple-choice follow-up questions, and subject line selection patterns work well without additional reinforcement.
- The agent produces HTML-formatted email drafts within the conversation. Claude renders these inline, making it easy for the parent to preview the email before approving.

### Formatting Support

- Claude renders full markdown: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all work correctly.
- The conversational state markers (`[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`) display as plain text, which is the intended behavior.
- HTML email drafts are rendered inline within the conversation, giving the parent a visual preview of the final email formatting.
- Mermaid diagrams and LaTeX are not used in the core prompt, keeping output portable across contexts.

### Limitations

- **Context window**: Long sessions — especially those involving multiple memories, extended follow-up rounds, or several edit iterations — may approach Claude's context limit. If responses start losing coherence or referencing earlier memories incorrectly, finalize the current email and start a new conversation for the next memory.
- **Session persistence**: Each conversation is independent. Project instructions carry over, but conversation history does not. Each memory email session is self-contained, so starting a new conversation for a new memory is natural.
- **Direct system message approach**: Pasting the prompt as a user message is less reliable than using Project instructions, since it occupies part of the context window and may be treated with slightly lower priority than true system instructions.

### What This Wrapper Does NOT Do

This setup guide does not modify the Memory Email Agent's conversation behavior, persona, methodology, or interaction patterns. All agent logic lives in the core prompt (`memory-email-agent-prompt.md`). This file only provides deployment instructions for the Claude platform.
