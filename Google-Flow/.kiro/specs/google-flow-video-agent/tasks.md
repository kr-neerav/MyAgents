# Implementation Plan: Google Flow Video Agent

## Overview

Build the Google Flow Video Agent as a prompt-based AI assistant with three deliverable files: a core prompt file and two platform deployment guides (Kiro and Gemini). All files are authored in Markdown and placed in the `Google-Flow/` directory. Implementation follows the design's component structure — core prompt first (identity, methodology, knowledge, templates, interaction patterns), then platform guides, with structural validation throughout.

## Tasks

- [x] 1. Create core prompt file with identity and methodology sections
  - [x] 1.1 Create `Google-Flow/google-flow-video-agent-prompt.md` with the Identity Section
    - Define agent name, role, and communication style (collaborative creative director tone)
    - Declare domain expertise: video production, Google Flow capabilities, storytelling
    - _Requirements: 1.1, 1.4_

  - [x] 1.2 Write the Methodology Section defining all four workflow phases
    - Phase 1 — Story Intake & Analysis: accept story, summarize narrative arc, estimate duration, recommend adjustments for stories outside 1–2 minute target
    - Phase 2 — Sequence Breakdown: decompose story into ordered sequences with timing (8-second segments), narrative purpose, transition recommendations
    - Phase 3 — Character & Scene Identification: extract characters and locations, produce Character Sheets and Scene Descriptions formatted for Google Flow prompts
    - Phase 4 — Prompt Generation: write detailed 200–500 word prompts per segment using video extension, incorporating character/scene consistency references, Hindi narration text written for 2x speed playback by an invisible narrator (allowing more narrative content per 8-second clip), background music suggestions, and positive visual tone guidance (negative characters convey negativity through expressions, not scary visuals)
    - Include instructions for handling duration adjustments (too long: recommend cuts per Req 1.2; too short: suggest expansions per Req 1.3)
    - Include instructions for handling ambiguous input (ask clarifying questions per Req 1.4)
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 2.1, 2.2, 2.3, 2.4, 2.5, 3.1, 3.2, 3.3, 3.4, 3.5, 4.1, 4.2, 4.3, 4.4, 4.5, 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8, 5.9_

- [x] 2. Add Google Flow knowledge and output format templates to core prompt
  - [x] 2.1 Write the Google Flow Knowledge Section
    - Video extension (extend) capability reference and usage patterns
    - Known limitations and workarounds (text rendering, complex multi-character interactions, action consistency)
    - Prompt optimization patterns for Google Flow video extension
    - Reference image best practices
    - _Requirements: 5.1, 5.2_

  - [x] 2.2 Write the Output Format Templates section
    - Sequence Breakdown table format (columns: #, description, duration, segments, narrative purpose, transition)
    - Character Sheet template (appearance, clothing, distinguishing features, props, sequences, appearance changes, Google Flow prompt reference)
    - Scene Description template (setting, lighting, color palette, time of day, weather, key elements, sequences, environmental changes, Google Flow prompt reference)
    - Segment Prompt template (capability, reference image, prompt text 200–500 words with visual content focus, Hindi narration text written for 2x speed playback, background music suggestion, positive visual tone guidance)
    - _Requirements: 2.1, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3, 4.4_

  - [x] 2.3 Write the Interaction Patterns section
    - Handling user feedback and revision requests at each phase (Req 2.5)
    - Handling ambiguous or incomplete story input (Req 1.4)
    - Handling user-provided reference images and visual preferences (Req 3.5, 4.5)
    - Session summary and context restoration patterns
    - _Requirements: 1.4, 2.5, 3.5, 4.5_

- [x] 3. Checkpoint — Review core prompt completeness
  - Ensure all tests pass, ask the user if questions arise.
  - Verify the core prompt file contains all sections: Identity, Methodology, Google Flow Knowledge, Output Format Templates, Interaction Patterns
  - Verify the core prompt contains NO platform-specific references (no mentions of Kiro, Claude, or Gemini)

- [x] 4. Create platform deployment guides
  - [x] 4.1 Create `Google-Flow/platform-kiro.md`
    - Deployment instructions using the steering file approach (`.kiro/steering/` directory)
    - Terminal output considerations (markdown rendering, long document handling)
    - Document known limitations: context window management, session persistence
    - Reference the core prompt file by name (`google-flow-video-agent-prompt.md`)
    - _Requirements: 6.2, 6.6_

  - [x] 4.2 Create `Google-Flow/platform-gemini.md`
    - Deployment instructions via Custom Instructions and Gems
    - Formatting support notes
    - Document known limitations: custom instructions length limits, context window, session persistence
    - Reference the core prompt file by name (`google-flow-video-agent-prompt.md`)
    - _Requirements: 6.4, 6.6_

- [x] 5. Checkpoint — Verify all deliverables exist and structural integrity
  - Ensure all tests pass, ask the user if questions arise.
  - Verify all three files exist: `google-flow-video-agent-prompt.md`, `platform-kiro.md`, `platform-gemini.md`
  - Verify each platform guide contains setup instructions and a limitations section
  - Verify the core prompt is platform-agnostic (no platform names in the file)

