# Implementation Plan: Mythology Story Narrator Agent

## Overview

Build the Mythology Story Narrator Agent by creating the core agent prompt file and three platform setup guides. The agent prompt defines a storyteller persona that narrates mythology stories sequentially from a RAG-backed corpus, with backstory context, real-world lessons, and a continuation marker system. After all deliverables are created, update the main README to include the new agent.

## Tasks

- [x] 1. Create the core agent prompt file
  - [x] 1.1 Create `MythologyStories/mythology-story-narrator-prompt.md` with the `<identity>` section
    - Define the narrator persona: a warm, engaging storyteller of mythology
    - Establish the agent's role as a sequential corpus narrator (not a scholar or analyst)
    - Define communication style: vivid, accessible, entertaining language
    - Define off-topic handling: redirect to narration domain
    - _Requirements: 3.1, 3.4, 3.5, 6.1, 6.2, 6.3_

  - [x] 1.2 Add the `<methodology>` section to the prompt file
    - Define RAG retrieval protocol: retrieve story content from RAG system before narrating
    - Define narration structure: how to build a complete, engaging narration from source material
    - Define backstory summary generation: condensed recap of prior events, characters, context when current story references earlier content
    - Define real-world lesson derivation: practical, present-day insight grounded in the story's specific themes
    - Define continuation marker system: format `[CONTINUE:story_index:corpus_id]`, parsing rules, emission rules, and next-index computation
    - _Requirements: 1.1, 1.2, 3.2, 3.3, 4.1, 4.2, 4.3, 5.1, 5.2_

  - [x] 1.3 Add the `<session_protocol>` section to the prompt file
    - Define session initialization: start from first story when no marker is provided
    - Define marker-based resumption: parse continuation marker and resume from next story
    - Define per-story narration flow: RAG retrieval → backstory (if needed) → narration → lesson → marker
    - Define end-of-corpus handling: inform user corpus is complete, no marker emitted
    - Define follow-up handling: answer questions in context of most recent story, remind user of continuation marker
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 5.3, 5.5_

  - [x] 1.4 Add the `<interaction_patterns>` section to the prompt file
    - Define invalid marker handling: inform user marker is not recognized, offer to restart or jump to specific point
    - Define missing RAG results handling: inform user content is unavailable, suggest alternatives, do not fabricate
    - Define mid-narration question handling: answer in context, then remind of continuation marker
    - Define skip/go-back request handling
    - _Requirements: 1.3, 5.4_

  - [x] 1.5 Add the `<formatting>` section to the prompt file
    - Define output structure markers: `[BACKSTORY]`, `[NARRATION]`, `[LESSON]`, `[CONTINUE:story_index:corpus_id]`
    - Define markdown rules: standard markdown only, no HTML/LaTeX/Mermaid
    - Define terminal compatibility guidelines: reasonable line lengths, no wide tables, clear visual separation
    - _Requirements: 6.2, 6.3_

- [x] 2. Checkpoint - Verify core prompt structure
  - Ensure the prompt file at `MythologyStories/mythology-story-narrator-prompt.md` exists and contains all five required sections: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
  - Ask the user if questions arise.

- [x] 3. Create platform setup guides
  - [x] 3.1 Create `MythologyStories/platform-kiro.md`
    - Provide Kiro CLI deployment instructions using the `.kiro/steering/` file mechanism
    - Reference `mythology-story-narrator-prompt.md` as the source of all agent behavior
    - Document terminal-specific notes: output markers display as plain text, keep narrations readable in terminal
    - Document limitations: context window for long narration sessions, session persistence, scrollback
    - Explicitly state the guide does NOT modify agent logic
    - Follow the structure and conventions of `Mythology/platform-kiro.md`
    - _Requirements: 7.1, 7.4_

  - [x] 3.2 Create `MythologyStories/platform-claude.md`
    - Provide two setup options: Projects (persistent) and direct system message (quick start)
    - Reference `mythology-story-narrator-prompt.md` as the source of all agent behavior
    - Document platform notes: instruction-following, formatting support for output markers
    - Document limitations: context window, session persistence, direct system message reliability
    - Explicitly state the guide does NOT modify agent logic
    - Follow the structure and conventions of `Mythology/platform-claude.md`
    - _Requirements: 7.2, 7.4_

  - [x] 3.3 Create `MythologyStories/platform-gemini.md`
    - Provide two setup options: Custom Instructions (quick start) and Gems (dedicated agent)
    - Reference `mythology-story-narrator-prompt.md` as the source of all agent behavior
    - Document platform notes: formatting support for output markers
    - Document limitations: context window, custom instructions length, session persistence
    - Explicitly state the guide does NOT modify agent logic
    - Follow the structure and conventions of `Mythology/platform-gemini.md`
    - _Requirements: 7.3, 7.4_

- [x] 4. Checkpoint - Verify all deliverables exist
  - Ensure all four files exist: `mythology-story-narrator-prompt.md`, `platform-kiro.md`, `platform-claude.md`, `platform-gemini.md` in `MythologyStories/`
  - Ask the user if questions arise.

- [x] 5. Update main README
  - Read `README.md` at the project root to understand the current structure and listing of agents
  - Add an entry for the Mythology Story Narrator Agent in the appropriate section
  - Include a brief description: RAG-backed sequential mythology story narrator with real-world lessons and continuation marker system
  - Link to `MythologyStories/mythology-story-narrator-prompt.md` and the platform guides
  - Follow the formatting conventions used for existing agent entries in the README

## Notes

- Each task references specific requirements for traceability
- The core prompt file follows workspace conventions established by the Mythology Scholar Agent
- Platform guides are thin deployment wrappers that reference the core prompt without duplicating logic
- Checkpoints ensure incremental validation of deliverables
