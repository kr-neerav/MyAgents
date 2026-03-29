# Mythology Story Narrator Agent — Claude Setup Guide

This guide walks you through deploying the Mythology Story Narrator Agent on Claude. The core prompt lives in `mythology-story-narrator-prompt.md` — this file only covers platform-specific setup.

## Setup Options

### Option A: Projects (Persistent Agent)

1. Open [Claude](https://claude.ai/).
2. In the left sidebar, click **Projects** → **Create a Project**.
3. Give it a name, e.g. "Mythology Story Narrator".
4. Open `mythology-story-narrator-prompt.md` and copy the entire contents.
5. Paste the full prompt into the **Project Instructions** field (under the project's settings).
6. Save the project.
7. To start a session, open the project and begin a new conversation.

> Project instructions apply to every conversation within that project. Your regular Claude conversations remain unaffected.

### Option B: Direct System Message (Quick Start)

1. Open [Claude](https://claude.ai/) and start a new conversation.
2. Open `mythology-story-narrator-prompt.md` and copy the entire contents.
3. Paste the full prompt as your first message, prefixed with: "Use the following as your system instructions for this conversation:"
4. Wait for Claude to acknowledge, then begin your mythology narration session.

> This approach works for one-off sessions. For repeated use, the Projects approach is more convenient since you do not need to re-paste the prompt each time.

## Platform Notes

### Instruction-Following

- Claude has strong instruction-following capabilities. The structured markers (`[BACKSTORY]`, `[NARRATION]`, `[LESSON]`, `[CONTINUE:story_index:corpus_id]`) and the sequential narration methodology in the core prompt are reliably followed.
- The storyteller persona, backstory generation, and real-world lesson derivation patterns work well without additional reinforcement.

### Formatting Support

- Claude renders full markdown: headers, bold, italic, numbered lists, bullet lists, and fenced code blocks all work correctly.
- The output structure markers (`[BACKSTORY]`, `[NARRATION]`, `[LESSON]`, `[CONTINUE:story_index:corpus_id]`) display as plain text, which is the intended behavior.
- Continuation markers work well for cross-session resumption via Projects — copy the marker from one conversation and paste it at the start of a new conversation within the same project to pick up where you left off.
- Mermaid diagrams and LaTeX are not used in the core prompt, keeping output portable across contexts.

### Limitations

- **Context window**: Long narration sessions — especially those involving multiple stories with extensive backstory summaries — may approach Claude's context limit. If responses start losing coherence or referencing earlier stories incorrectly, copy the most recent continuation marker and start a new conversation to resume from that point.
- **Session persistence**: Each conversation is independent. Project instructions carry over, but conversation history does not. Use the continuation marker to restore your corpus position in a new conversation.
- **Direct system message approach**: Pasting the prompt as a user message is less reliable than using Project instructions, since it occupies part of the context window and may be treated with slightly lower priority than true system instructions.

### What This Guide Does NOT Do

This setup guide does not modify the Mythology Story Narrator Agent's narration behavior, persona, methodology, or interaction patterns. All storytelling logic, backstory generation, lesson derivation, and continuation marker handling lives in the core prompt (`mythology-story-narrator-prompt.md`). This file only provides deployment instructions for the Claude platform.
