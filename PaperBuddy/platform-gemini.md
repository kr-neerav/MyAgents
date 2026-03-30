# PaperBuddy Agent — Gemini Chat Setup Guide

This guide walks you through deploying the PaperBuddy Agent on Google Gemini. The core prompt lives in `paper-buddy-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Custom Instructions (Quick Start)

1. Open [Gemini](https://gemini.google.com/).
2. Click your profile icon (top-right) → **Settings** → **Extensions & Custom Instructions** → **Custom Instructions**.
3. Open `paper-buddy-agent-prompt.md` and copy the entire contents.
4. Paste the full prompt into the custom instructions text field.
5. Save your changes.
6. Start a new chat — the PaperBuddy Agent persona is now active for all conversations.

> To disable, return to Settings and clear the custom instructions field.

### Option B: Gems (Dedicated Agent)

1. Open [Gemini](https://gemini.google.com/).
2. In the left sidebar, click **Gem manager** → **New Gem**.
3. Give it a name, e.g. "PaperBuddy Agent".
4. Open `paper-buddy-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Instructions** field of the Gem.
6. Click **Save**.
7. To start a session, select your "PaperBuddy Agent" Gem from the sidebar.

> Gems keep the PaperBuddy Agent isolated from your regular Gemini conversations.

## Platform Notes

### Formatting Support

- Gemini renders standard markdown well: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all work.
- The structured breakdown output (emoji-headed pillar sections, inline jargon translations, bullet-point takeaways, and the Strengths & Limitations assessment) displays cleanly and is easy to scan.
- Mermaid diagrams and LaTeX are not reliably supported — the core prompt avoids these by design.

### Limitations

- **Context window**: Long papers combined with extensive follow-up conversations — deep dives into specific pillars, jargon explorations, and critical evaluations — may approach Gemini's context limit. If responses start losing coherence or referencing earlier paper details incorrectly, ask for a session summary and start a new chat with the recap.
- **Custom Instructions length**: Gemini may impose a character limit on custom instructions. If the core prompt exceeds this limit, use the Gems approach instead, which typically allows longer instructions.
- **Session persistence**: Gemini does not carry custom instructions or Gem context across separate chat sessions. Each new chat starts fresh — use session summaries to restore context.

### What This Wrapper Does NOT Do

This setup guide does not modify the PaperBuddy Agent's behavior, methodology, output formatting, or interaction patterns. All agent logic lives in the core prompt (`paper-buddy-agent-prompt.md`). This file only provides deployment instructions for the Gemini platform.
