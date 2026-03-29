# Mythology Story Narrator Agent — Kiro CLI Setup Guide

This guide walks you through deploying the Mythology Story Narrator Agent on Kiro CLI. The core prompt lives in `mythology-story-narrator-prompt.md` — this file only covers platform-specific setup.

## Setup

### Deploy via Steering File

1. In your project root, create the directory `.kiro/steering/` if it does not already exist.
2. Open `mythology-story-narrator-prompt.md` and copy the entire contents.
3. Create a new file at `.kiro/steering/mythology-story-narrator.md`.
4. Paste the full prompt into this steering file.
5. Save the file. Kiro will automatically pick up the steering file for conversations in this project.
6. Open a Kiro CLI chat session — the Mythology Story Narrator Agent persona is now active.

> To disable, remove or rename the `.kiro/steering/mythology-story-narrator.md` file.

## Platform Notes

### Terminal-Based Output

Kiro CLI runs in a terminal environment. Keep these considerations in mind:

- Output is rendered as plain text in a terminal. Standard markdown (headers, bold, lists, code blocks) is supported, but complex visual formatting will not render well.
- Avoid relying on wide tables, nested indentation beyond 2–3 levels, or horizontal rules for visual structure. Prefer short paragraphs and clear section breaks.
- The output structure markers (`[BACKSTORY]`, `[NARRATION]`, `[LESSON]`, `[CONTINUE:story_index:corpus_id]`) display as plain text, which works well in a terminal context.
- Keep narrations readable in the terminal — use reasonable line lengths and separate each marker section with blank lines so backstory, narration, lesson, and continuation marker are visually distinct.
- The continuation marker (`[CONTINUE:story_index:corpus_id]`) is easy to copy-paste in terminal environments. Select the full marker text, paste it back into the chat, and the agent will pick up the next story.

### Limitations

- **Context window**: Long narration sessions may approach the model's context limit. If responses start losing coherence or referencing earlier stories incorrectly, ask for a summary and start a new session with the recap.
- **Session persistence**: Each Kiro CLI chat session is independent. The steering file is loaded fresh each time. Use the continuation marker to carry your corpus position across sessions — copy the last marker from one session and paste it at the start of the next.
- **Scrollback**: Terminal scrollback buffers vary. If you need to reference an earlier narration, request a recap of that story rather than scrolling back through the full history. Long narrations can push earlier content out of the scrollback buffer.

### What This Guide Does NOT Do

This setup guide does not modify the Mythology Story Narrator Agent's narration behavior, persona, methodology, or interaction patterns. All storytelling logic, backstory generation, lesson derivation, and continuation marker handling lives in the core prompt (`mythology-story-narrator-prompt.md`). This file only provides deployment instructions for the Kiro CLI platform.
