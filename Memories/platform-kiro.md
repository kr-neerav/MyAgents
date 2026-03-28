# Memory Email Agent — Kiro CLI Setup Guide

This guide walks you through deploying the Memory Email Agent on Kiro CLI. The core prompt lives in `memory-email-agent-prompt.md` — this file only covers platform-specific setup.

## Setup

### Deploy via Steering File

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `memory-email-agent-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/memory-email-agent.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the Memory Email Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/memory-email-agent.md` file.

## Platform Notes

### Terminal-Based Output

Kiro CLI runs in a terminal environment. Keep these considerations in mind:

- Output is rendered as plain text in a terminal. Standard markdown (headers, bold, lists, code blocks) is supported, but complex visual formatting will not render well.
- The conversational state markers (`[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`) display as plain text, which works well in a terminal context.
- The multiple-choice options for follow-up questions, subject line selection, and edit suggestions are labeled with letters (a–e/f) and render cleanly in a terminal.
- HTML email drafts will display as raw HTML in the terminal. This is expected — the parent can still read and review the email content. The HTML is the final output format intended for pasting into an email client, not for terminal rendering.
- Avoid relying on wide tables, nested indentation beyond 2–3 levels, or horizontal rules for visual structure. Prefer numbered lists and short paragraphs.

### Limitations

- **Context window**: Long sessions — especially those involving multiple memories, extended follow-up rounds, or several edit iterations — may approach the model's context limit. If responses start losing coherence or referencing earlier memories incorrectly, finalize the current email and start a new session for the next memory.
- **Session persistence**: Each Kiro CLI chat session is independent. The steering file is loaded fresh each time. Each memory email session is self-contained, so starting a new session for a new memory is natural.
- **Scrollback**: Terminal scrollback buffers vary. If you need to reference earlier parts of the conversation (e.g., the original memory description or a previous draft), request a summary rather than scrolling back through the full history.

### What This Wrapper Does NOT Do

This setup guide does not modify the Memory Email Agent's conversation behavior, persona, methodology, or interaction patterns. All agent logic lives in the core prompt (`memory-email-agent-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
