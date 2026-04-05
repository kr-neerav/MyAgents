# Blog Evaluator Agent — Gemini Chat Setup Guide

This guide walks you through deploying the Blog Evaluator Agent on Google Gemini. The core prompt lives in `blog-evaluator-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Custom Instructions (Quick Start)

1. Open [Gemini](https://gemini.google.com/).
2. Click your profile icon (top-right) → **Settings** → **Extensions & Custom Instructions** → **Custom Instructions**.
3. Open `blog-evaluator-agent-prompt.md` and copy the entire contents.
4. Paste the full prompt into the custom instructions text field.
5. Save your changes.
6. Start a new chat — the Blog Evaluator Agent persona is now active for all conversations.

> To disable, return to Settings and clear the custom instructions field.

### Option B: Gems (Dedicated Agent)

1. Open [Gemini](https://gemini.google.com/).
2. In the left sidebar, click **Gem manager** → **New Gem**.
3. Give it a name, e.g. "Blog Evaluator Agent".
4. Open `blog-evaluator-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Instructions** field of the Gem.
6. Click **Save**.
7. To start a session, select your "Blog Evaluator Agent" Gem from the sidebar.

> Gems keep the Blog Evaluator Agent isolated from your regular Gemini conversations.

## Platform Notes

### Formatting Support

- Gemini renders standard markdown well: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all work.
- The conversational state markers (`[SITUATION]`, `[REFRAME]`, `[GROWTH DIMENSIONS]`, `[TAKEAWAYS]`, `[SUMMARY]`) display as plain text, which is the intended behavior.
- Mermaid diagrams and LaTeX are not reliably supported — the core prompt avoids these by design.

### Limitations

- **Context window**: Long mentoring sessions may approach Gemini's context limit. If responses start losing coherence or referencing earlier situations incorrectly, ask for a `[SUMMARY]` and start a new chat with the recap.
- **Custom Instructions length**: Gemini may impose a character limit on custom instructions. If the core prompt exceeds this limit, use the Gems approach instead, which typically allows longer instructions.
- **Session persistence**: Gemini does not carry custom instructions or Gem context across separate chat sessions. Each new chat starts fresh — use session summaries to restore context.

### What This Wrapper Does NOT Do

This setup guide does not modify the Blog Evaluator Agent's writing behavior, persona, methodology, or interaction patterns. All pedagogical logic lives in the core prompt (`blog-evaluator-agent-prompt.md`). This file only provides deployment instructions for the Gemini platform.
