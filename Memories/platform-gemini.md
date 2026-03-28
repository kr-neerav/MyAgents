# Memory Email Agent — Gemini Chat Setup Guide

This guide walks you through deploying the Memory Email Agent on Google Gemini. The core prompt lives in `memory-email-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Custom Instructions (Quick Start)

1. Open [Gemini](https://gemini.google.com/).
2. Click your profile icon (top-right) → **Settings** → **Extensions & Custom Instructions** → **Custom Instructions**.
3. Open `memory-email-agent-prompt.md` and copy the entire contents.
4. Paste the full prompt into the custom instructions text field.
5. Save your changes.
6. Start a new chat — the Memory Email Agent persona is now active for all conversations.

> To disable, return to Settings and clear the custom instructions field.

### Option B: Gems (Dedicated Agent)

1. Open [Gemini](https://gemini.google.com/).
2. In the left sidebar, click **Gem manager** → **New Gem**.
3. Give it a name, e.g. "Memory Email Agent".
4. Open `memory-email-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Instructions** field of the Gem.
6. Click **Save**.
7. To start a session, select your "Memory Email Agent" Gem from the sidebar.

> Gems keep the Memory Email Agent isolated from your regular Gemini conversations.

## Platform Notes

### Formatting Support

- Gemini renders standard markdown well: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all work.
- The conversational state markers (`[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`) display as plain text, which is the intended behavior.
- HTML email drafts are rendered inline within the conversation, giving the parent a visual preview of the final email formatting.
- The multiple-choice options for follow-up questions, subject line selection, and edit suggestions are labeled with letters (a–e/f) and work well in Gemini's chat interface.
- Mermaid diagrams and LaTeX are not used in the core prompt, keeping output portable across platforms.

### Limitations

- **Context window**: Long sessions — especially those involving multiple memories, extended follow-up rounds, or several edit iterations — may approach Gemini's context limit. If responses start losing coherence or referencing earlier memories incorrectly, finalize the current email and start a new chat for the next memory.
- **Custom Instructions length**: Gemini may impose a character limit on custom instructions. If the core prompt exceeds this limit, use the Gems approach instead, which typically allows longer instructions.
- **Session persistence**: Gemini does not carry custom instructions or Gem context across separate chat sessions. Each new chat starts fresh. Each memory email session is self-contained, so starting a new chat for a new memory is natural.

### What This Wrapper Does NOT Do

This setup guide does not modify the Memory Email Agent's conversation behavior, persona, methodology, or interaction patterns. All agent logic lives in the core prompt (`memory-email-agent-prompt.md`). This file only provides deployment instructions for the Gemini platform.
