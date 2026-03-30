# PaperBuddy Agent — Kiro CLI Setup Guide

This guide walks you through deploying the PaperBuddy Agent on Kiro CLI. The core prompt lives in `paper-buddy-agent-prompt.md` — this file only covers platform-specific setup.

## Setup

### Deploy via Steering File

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `paper-buddy-agent-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/paper-buddy-agent.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the PaperBuddy Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/paper-buddy-agent.md` file.

## Platform Notes

### Terminal-Based Output

Kiro CLI runs in a terminal environment. Keep these considerations in mind:

- Output is rendered as plain text in a terminal. Standard markdown (headers, bold, lists, code blocks) is supported, but complex visual formatting will not render well.
- Avoid relying on wide tables, nested indentation beyond 2–3 levels, or horizontal rules for visual structure. Prefer numbered lists and short paragraphs.
- The structured breakdown output (emoji headers, pillar sections, bullet-point takeaways, and jargon translations) displays well in a terminal context and remains scannable.
- Long papers may need to be pasted in chunks due to terminal input limits. Paste the abstract and key sections first, then follow up with additional sections as needed.
- Keep follow-up responses concise — long blocks of text are harder to read in a terminal without scrollback.

### Limitations

- **Context window**: Long papers or sessions with many rounds of follow-up questions, deep dives, and jargon explorations may approach the model's context limit. If responses start losing coherence or referencing earlier paper details incorrectly, ask for a session summary and start a new session with the recap.
- **Session persistence**: Each Kiro CLI chat session is independent. The steering file is loaded fresh each time. Use session summaries to carry paper analysis context across sessions.
- **Session summaries**: When starting a new session, paste the session summary from your previous session at the beginning of the conversation. This restores context about the paper analyzed, key breakdown insights, jargon translations, and any follow-up explorations — allowing the agent to pick up where you left off.

### What This Wrapper Does NOT Do

This setup guide does not modify the PaperBuddy Agent's behavior, methodology, output formatting, or interaction patterns. All agent logic lives in the core prompt (`paper-buddy-agent-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
