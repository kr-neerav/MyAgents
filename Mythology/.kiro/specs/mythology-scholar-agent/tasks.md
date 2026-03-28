# Implementation Plan: Mythology Scholar Agent

## Overview

Create the core prompt file and three platform deployment guides for the Mythology Scholar Agent. All deliverables are markdown files placed in the `Mythology/` folder. The core prompt follows the Learning Agent's structural pattern (XML-tagged sections, conversational state markers) adapted for mythology scholarship.

## Tasks

- [x] 1. Create the core prompt file — `mythology-scholar-prompt.md`
  - [x] 1.1 Write the `<identity>` section
    - Define the agent's persona as a mythology scholar with expertise across world mythological traditions (Greek, Norse, Egyptian, Hindu, Mesopotamian, Celtic, Japanese, Indigenous, African, Mesoamerican, and others)
    - Define communication style: scholarly but accessible, warm, intellectually curious, introduces terms before using them
    - Define the agent's role: analyze mythology texts, connect themes to personal situations and current events, draw cross-tradition comparisons
    - Define off-topic handling: acknowledge non-mythology questions and redirect to the mythological domain
    - Follow the Learning Agent's `<identity>` section as a structural reference
    - _Requirements: 5.1, 5.2_

  - [x] 1.2 Write the `<methodology>` section
    - Define Source Text Processing: how the agent receives, identifies, and acknowledges mythology texts — recognizing tradition, culture of origin, historical period; handling unidentified texts and fragmentary sources
    - Define Scholarly Analysis Framework: structured approach covering origin/cultural context, key symbols and archetypes, narrative structure, thematic meaning; citing interpretive frameworks; presenting multiple scholarly perspectives
    - Define Personal Interpretation Approach: connecting mythology themes to user's personal situation; framing as reflective perspectives not prescriptive advice; protocol for offering and respecting decline
    - Define Real-World Connection Approach: drawing parallels to current events and modern phenomena; grounding in thematic content; multi-viewpoint presentation without political/ideological bias
    - Define Cross-Tradition Comparative Analysis: identifying shared themes, parallel structures, divergent interpretations across traditions; attribution without overgeneralization
    - Define Terminology Glossary Protocol: inline definitions on first use, consistent usage throughout session
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 2.1, 2.2, 2.3, 2.4, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3, 4.4, 7.1, 7.2, 7.3_

  - [x] 1.3 Write the `<session_protocol>` section
    - Define Session Initialization: greet the user, ask what mythology text or topic they want to explore, handle cases where user provides text immediately vs. needs guidance
    - Define Analysis Flow: structured sequence when a Source_Text is provided — (1) Acknowledge and identify, (2) `[ANALYSIS]`, (3) Offer `[PERSONAL INTERPRETATION]`, (4) Offer `[REAL-WORLD CONNECTION]`, (5) `[COMPARATIVE]` where relevant
    - Define Follow-Up Handling: provide targeted detail on specific aspects without repeating full analysis
    - Define Session Summary: on request or at natural endpoints, produce `[SUMMARY]` covering texts analyzed, key themes, and connections drawn
    - _Requirements: 1.1, 2.5, 5.3_

  - [x] 1.4 Write the `<interaction_patterns>` section
    - Define Non-Mythology Text Handling: inform user and ask for clarification when text isn't identifiable as mythology
    - Define Tradition Inference: infer tradition from textual cues and state inference explicitly when user doesn't specify
    - Define Fragmentary Text Handling: work with available material and note limitations
    - Define Personal Context Decline: continue without requesting personal details again in the same exchange
    - Define Multiple Scholarly Perspectives: present major interpretive perspectives rather than selecting one
    - Define Focused Real-World Connection: focus on user-specified current event or topic when provided
    - _Requirements: 1.2, 1.3, 1.4, 2.3, 3.4, 4.4_

  - [x] 1.5 Write the `<formatting>` section
    - Define conversational state markers: `[ANALYSIS]`, `[PERSONAL INTERPRETATION]`, `[REAL-WORLD CONNECTION]`, `[COMPARATIVE]`, `[SUMMARY]`
    - Define markdown rules: standard markdown only (headers, bold, italic, lists), no HTML/LaTeX/Mermaid/platform-specific features
    - Define terminal compatibility: reasonable line lengths, avoid wide tables or deep nesting, prefer numbered lists and short paragraphs
    - _Requirements: 5.3, 5.4, 5.5_

- [x] 2. Checkpoint — Review core prompt
  - Ensure the core prompt file is complete with all five XML sections
  - Verify all requirements from 1.x, 2.x, 3.x, 4.x, 5.x, and 7.x are addressed
  - Verify the file is platform-agnostic with no references to specific deployment platforms
  - Ensure all tests pass, ask the user if questions arise.

- [x] 3. Create platform deployment guides
  - [x] 3.1 Write `platform-claude.md`
    - Include Option A (Projects) and Option B (Direct System Message) setup instructions
    - Reference `mythology-scholar-prompt.md` by filename without duplicating prompt content
    - Document platform notes: instruction-following characteristics, formatting support
    - Document limitations: context window, session persistence, direct system message trade-offs
    - Include explicit non-scope statement: guide contains no behavioral logic
    - Follow the Learning Agent's `platform-claude.md` as a structural reference
    - _Requirements: 6.1, 6.4, 6.5, 6.6_

  - [x] 3.2 Write `platform-gemini.md`
    - Include Option A (Custom Instructions) and Option B (Gems) setup instructions
    - Reference `mythology-scholar-prompt.md` by filename without duplicating prompt content
    - Document platform notes: formatting support, marker rendering
    - Document limitations: context window, custom instructions length, session persistence
    - Include explicit non-scope statement: guide contains no behavioral logic
    - Follow the Learning Agent's `platform-gemini.md` as a structural reference
    - _Requirements: 6.2, 6.4, 6.5, 6.6_

  - [x] 3.3 Write `platform-kiro.md`
    - Include steering file setup: copy core prompt to `.kiro/steering/mythology-scholar.md`
    - Reference `mythology-scholar-prompt.md` by filename without duplicating prompt content
    - Document platform notes: terminal-based output considerations, marker rendering
    - Document limitations: context window, session persistence, scrollback
    - Include explicit non-scope statement: guide contains no behavioral logic
    - Follow the Learning Agent's `platform-kiro.md` as a structural reference
    - _Requirements: 6.3, 6.4, 6.5, 6.6_

- [x] 4. Final checkpoint — Review all deliverables
  - Ensure all four files exist in `Mythology/`: `mythology-scholar-prompt.md`, `platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`
  - Verify platform guides reference the core prompt by filename and contain no behavioral logic
  - Verify all seven requirements are covered across the deliverables
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- This is a prompt engineering project — all deliverables are markdown files, not code
- The Learning Agent files in `Learning/` serve as the structural pattern to follow
- The core prompt uses XML-tagged sections and conversational state markers, matching the Learning Agent's architecture
- Platform guides are thin wrappers containing only deployment instructions
- Each task references specific requirements for traceability
