# Google Flow Video Agent — Kiro CLI Setup Guide

This guide walks you through deploying the Google Flow Video Agent on Kiro CLI. The core prompt lives in `google-flow-video-agent-prompt.md` — this file only covers platform-specific setup.

## Setup

### Deploy via Steering File

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `google-flow-video-agent-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/google-flow-video-agent.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the Google Flow Video Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/google-flow-video-agent.md` file.

## Platform Notes

### Terminal-Based Output

Kiro CLI renders markdown in the terminal. Keep these considerations in mind:

- Standard markdown (headers, bold, lists, code blocks) is supported, but complex visual formatting will not render well.
- The Agent produces structured tables (Sequence Breakdowns, Character Sheets, Scene Descriptions). Simple tables render acceptably in most terminals, but wide tables with many columns may wrap awkwardly. If table output is hard to read, ask the Agent to switch to a list-based format for that section.
- The Production Guide is a long, structured document. Rather than requesting the full guide in one message, ask the Agent to output it in sections (e.g., "Show me the Pre-Production section" then "Now show Production"). This keeps each response readable and avoids hitting output length limits.
- Prompt templates for Google Flow segments can be long. Copy them directly from the terminal output into Google Flow — the plain-text rendering works well for this purpose.

### Limitations

- **Context window management**: The full Production Guide — including Sequence Breakdowns, Character Sheets, Scene Descriptions, all segment prompts, and stitching guidance — can be substantial for longer stories. Combined with the conversation history from the planning phases, this may approach context limits. If responses start losing detail or referencing earlier decisions incorrectly, ask the Agent for a session summary and start a new session with that summary to restore context.
- **Session persistence**: Kiro CLI sessions do not persist between restarts. If you close the terminal or start a new session, the conversation history is lost. Before ending a session, ask the Agent to produce a session summary capturing the current state of your production plan. Paste this summary at the start of your next session to pick up where you left off.
- **Scrollback**: Terminal scrollback buffers vary. For long production sessions, request individual sections of the Production Guide rather than the full document. If you need to revisit earlier output, ask the Agent to regenerate the specific section rather than scrolling back through the full history.

### What This Wrapper Does NOT Do

This setup guide does not modify the Google Flow Video Agent's production methodology, persona, workflow phases, or interaction patterns. All agent logic lives in the core prompt (`google-flow-video-agent-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
