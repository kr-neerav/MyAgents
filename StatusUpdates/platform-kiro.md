# Status Update Agent — Kiro CLI Setup Guide

This guide walks you through deploying the Status Update Agent on Kiro CLI. The core prompt lives in `status-update-agent-prompt.md` — this file only covers platform-specific setup.

## Setup

### Deploy via Steering File

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `status-update-agent-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/status-update-agent.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the Status Update Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/status-update-agent.md` file.

## Platform Notes

### Terminal-Based Output

Kiro CLI runs in a terminal environment. Keep these considerations in mind:

- Output is rendered as plain text in a terminal. Standard markdown (headers, bold, lists, code blocks) is supported, but complex visual formatting will not render well.
- The structured status update output (section headers, bullet lists, owner fields) displays well in a terminal context and remains scannable.
- Weekly Status Reports are designed as inline text — no tables or wide formatting that would break in a terminal.
- Keep follow-up refinement requests concise for easier terminal readability.

### Limitations

- **Context window**: Long sessions with many rounds of refinement, type switches, and draft reviews may approach the model's context limit. If responses start losing coherence or referencing earlier updates incorrectly, ask for a session summary and start a new session with the recap.
- **Session persistence**: Each Kiro CLI chat session is independent. The steering file is loaded fresh each time. Use session summaries to carry context across sessions.
- **Session summaries**: When starting a new session, paste the session summary from your previous session at the beginning of the conversation. This restores context about the project, update type, and any refinements made.

### What This Wrapper Does NOT Do

This setup guide does not modify the Status Update Agent's behavior, methodology, output formatting, or interaction patterns. All agent logic lives in the core prompt (`status-update-agent-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
