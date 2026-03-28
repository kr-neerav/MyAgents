# Requirements Agent — Kiro CLI Setup Guide

This guide walks you through deploying the Requirements Agent on Kiro CLI. The core prompt lives in `requirements-agent-prompt.md` — this file only covers platform-specific setup.

## Setup

### Deploy via Steering File

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `requirements-agent-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/requirements-agent.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the Requirements Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/requirements-agent.md` file.

## Platform Notes

### Terminal-Based Output

Kiro CLI runs in a terminal environment. Keep these considerations in mind:

- Output is rendered as plain text in a terminal. Standard markdown (headers, bold, lists, code blocks) is supported, but complex visual formatting will not render well.
- Avoid relying on wide tables, nested indentation beyond 2–3 levels, or horizontal rules for visual structure. Prefer numbered lists and short paragraphs.
- The conversational state markers (`[PROBLEM SCOPE]`, `[ELICIT]`, `[ORGANIZE]`, `[REFINE]`, `[DOCUMENT]`, `[SUMMARY]`) display as plain text, which works well in a terminal context.
- Keep code block examples concise — long code blocks are harder to read in a terminal without scrollback.

### Limitations

- **Context window**: Long requirements sessions may approach the model's context limit. If responses start losing coherence or referencing earlier requirements incorrectly, ask for a `[SUMMARY]` and start a new session with the recap.
- **Session persistence**: Each Kiro CLI chat session is independent. The steering file is loaded fresh each time. Use session summaries to carry context across sessions.
- **Scrollback**: Terminal scrollback buffers vary. If you need to reference earlier parts of the conversation, request a `[SUMMARY]` rather than scrolling back through the full history.

### What This Wrapper Does NOT Do

This setup guide does not modify the Requirements Agent's requirements engineering behavior, persona, methodology, or interaction patterns. All requirements engineering logic lives in the core prompt (`requirements-agent-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
