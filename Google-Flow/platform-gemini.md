# Google Flow Video Agent — Gemini Setup Guide

This guide walks you through deploying the Google Flow Video Agent on Google Gemini. The core prompt lives in `google-flow-video-agent-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Custom Instructions (Quick Start)

1. Open [Gemini](https://gemini.google.com/).
2. Click your profile icon (top-right) → **Settings** → **Extensions & Custom Instructions** → **Custom Instructions**.
3. Open `google-flow-video-agent-prompt.md` and copy the entire contents.
4. Paste the full prompt into the custom instructions text field.
5. Save your changes.
6. Start a new chat — the Google Flow Video Agent persona is now active for all conversations.

> **Note:** Gemini imposes a character limit on custom instructions. The full core prompt may exceed this limit. If it does, use the Gems approach (Option B) instead, or condense the prompt by removing the Output Format Templates section and asking the Agent to produce formats on demand during conversation.

> To disable, return to Settings and clear the custom instructions field.

### Option B: Gems (Dedicated Agent)

1. Open [Gemini](https://gemini.google.com/).
2. In the left sidebar, click **Gem manager** → **New Gem**.
3. Give it a name, e.g. "Google Flow Video Agent".
4. Open `google-flow-video-agent-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Instructions** field of the Gem.
6. Click **Save**.
7. To start a session, select your "Google Flow Video Agent" Gem from the sidebar.

> Gems keep the Agent isolated from your regular Gemini conversations and typically allow longer instructions than Custom Instructions.

## Platform Notes

### Formatting Support

- Gemini renders standard markdown well: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all display correctly.
- The Agent produces structured tables (Sequence Breakdowns, Character Sheets, Scene Descriptions). Gemini handles markdown tables, though very wide tables may not render perfectly. If a table is hard to read, ask the Agent to switch to a list-based format for that section.
- The Production Guide is a long, structured document. Gemini handles long outputs reasonably well, but for the best experience, request it in sections (e.g., "Show me the Pre-Production section" then "Now show the Production section") rather than all at once.
- Prompt templates for Google Flow segments can be copied directly from Gemini's output into Google Flow.

### Limitations

- **Custom Instructions length limits**: Gemini's Custom Instructions field has a character cap. The full core prompt — which includes Identity, Methodology, Google Flow Knowledge, Output Format Templates, Interaction Patterns, Visual Consistency, and Stitching Guidance sections — may exceed this limit. If it does, use the Gems approach, which supports longer instructions. As a fallback, you can condense the prompt by removing the Output Format Templates section (the Agent can reproduce these formats on request during conversation).
- **Context window management**: The full Production Guide — including Sequence Breakdowns, Character Sheets, Scene Descriptions, all segment prompts, and stitching guidance — can be substantial for longer stories. Combined with the conversation history from the planning phases, this may approach Gemini's context limits. If responses start losing detail or referencing earlier decisions incorrectly, ask the Agent for a session summary and start a new chat with that summary to restore context.
- **Session persistence**: Gemini conversations persist within a single chat thread, but context may degrade over very long sessions as the conversation grows. Each new chat starts fresh with no memory of previous sessions. Before ending a session or if you notice quality degradation, ask the Agent to produce a session summary capturing the current state of your production plan. Paste this summary at the start of your next chat to pick up where you left off.

### What This Wrapper Does NOT Do

This setup guide does not modify the Google Flow Video Agent's production methodology, persona, workflow phases, or interaction patterns. All agent logic lives in the core prompt (`google-flow-video-agent-prompt.md`). This file only provides deployment instructions for the Gemini platform.
