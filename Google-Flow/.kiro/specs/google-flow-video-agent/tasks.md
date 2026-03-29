# Implementation Plan: Google Flow Video Agent

## Overview

Build the Google Flow Video Agent as a prompt-based AI assistant with three deliverable files: a core prompt file and two platform deployment guides (Kiro and Gemini). All files are authored in Markdown and placed in the `Google-Flow/` directory. Implementation follows the design's component structure — core prompt first (identity, methodology, knowledge, templates, interaction patterns), then platform guides, with structural validation throughout.

## Tasks

- [x] 1. Create core prompt file with identity and methodology sections
  - [x] 1.1 Create `Google-Flow/google-flow-video-agent-prompt.md` with the Identity Section
    - Define agent name, role, and communication style (collaborative creative director tone)
    - Declare domain expertise: video production, Google Flow capabilities, storytelling
    - _Requirements: 1.1, 1.4_

  - [x] 1.2 Write the Methodology Section defining all seven workflow phases
    - Phase 1 — Story Intake & Analysis: accept story, summarize narrative arc, estimate duration, recommend adjustments for stories outside 1–2 minute target
    - Phase 2 — Sequence Breakdown: decompose story into ordered sequences with timing, narrative purpose, transition recommendations
    - Phase 3 — Character & Scene Identification: extract characters and locations, produce Character Sheets and Scene Descriptions formatted for Google Flow prompts
    - Phase 4 — Capability Selection: map each segment to Google Flow features (text-to-video, image-to-video, camera controls, style references, video extension)
    - Phase 5 — Prompt Generation: write detailed prompts per segment incorporating character/scene consistency references
    - Phase 6 — Stitching Guidance: assembly order, transition types, audio recommendations, tool suggestions
    - Phase 7 — Production Guide Compilation: compile all outputs into structured checklist document
    - Include instructions for handling duration adjustments (too long: recommend cuts per Req 1.2; too short: suggest expansions per Req 1.3)
    - Include instructions for handling ambiguous input (ask clarifying questions per Req 1.4)
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 2.1, 2.2, 2.3, 2.4, 2.5, 3.1, 3.2, 3.3, 3.4, 3.5, 4.1, 4.2, 4.3, 4.4, 4.5, 5.1, 5.2, 5.3, 5.4, 5.5, 6.1, 6.2, 6.3, 6.4, 6.5, 7.1, 7.2, 7.3, 7.4, 7.5, 8.1, 8.2, 8.3, 8.4, 8.5_

- [x] 2. Add Google Flow knowledge and output format templates to core prompt
  - [x] 2.1 Write the Google Flow Knowledge Section
    - Capabilities reference: text-to-video, image-to-video, video extension, camera controls, style references
    - Known limitations and workarounds (text rendering, complex multi-character interactions, action consistency)
    - Prompt optimization patterns for Google Flow
    - Reference image best practices
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 9.5_

  - [x] 2.2 Write the Output Format Templates section
    - Sequence Breakdown table format (columns: #, description, duration, segments, narrative purpose, transition)
    - Character Sheet template (appearance, clothing, distinguishing features, props, sequences, appearance changes, Google Flow prompt reference)
    - Scene Description template (setting, lighting, color palette, time of day, weather, key elements, sequences, environmental changes, Google Flow prompt reference)
    - Segment Prompt template (capability, camera, reference image, prompt text, visual consistency notes)
    - Production Guide structure with checklist format and time-of-effort estimates
    - _Requirements: 2.1, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3, 4.4, 6.1, 6.2, 6.5, 8.1, 8.2, 8.3, 8.5_

  - [x] 2.3 Write the Interaction Patterns section
    - Handling user feedback and revision requests at each phase (Req 2.5, 6.4, 7.3, 8.4)
    - Handling ambiguous or incomplete story input (Req 1.4)
    - Handling visual inconsistency reports (Req 9.4)
    - Handling user-provided reference images and visual preferences (Req 3.5, 4.5)
    - Session summary and context restoration patterns
    - _Requirements: 1.4, 2.5, 3.5, 4.5, 6.4, 7.3, 8.4, 9.4_

- [x] 3. Add visual consistency and stitching guidance to core prompt
  - [x] 3.1 Write Visual Consistency instructions within the core prompt
    - Instructions to include Character Sheet and Scene Description references in every prompt
    - Instructions to use identical visual descriptors for shared characters/locations across prompts
    - Instructions to recommend consistent art style, color grading, and aspect ratio
    - Instructions to recommend generating reference images for major characters and scenes before segment generation
    - Instructions to diagnose and address visual inconsistency reports
    - _Requirements: 9.1, 9.2, 9.3, 9.4, 9.5_

  - [x] 3.2 Write Stitching Guidance instructions within the core prompt
    - Segment assembly order and transition type recommendations
    - Free/accessible video editing tool recommendations
    - Audio element guidance (narration, music, sound effects)
    - Output format, resolution, and aspect ratio recommendations based on distribution platform
    - Handling pacing/continuity issue reports
    - _Requirements: 7.1, 7.2, 7.3, 7.4, 7.5_

- [x] 4. Checkpoint — Review core prompt completeness
  - Ensure all tests pass, ask the user if questions arise.
  - Verify the core prompt file contains all sections: Identity, Methodology, Google Flow Knowledge, Output Format Templates, Interaction Patterns, Visual Consistency, Stitching Guidance
  - Verify the core prompt contains NO platform-specific references (no mentions of Kiro, Claude, or Gemini)

- [x] 5. Create platform deployment guides
  - [x] 5.1 Create `Google-Flow/platform-kiro.md`
    - Deployment instructions using the steering file approach (`.kiro/steering/` directory)
    - Terminal output considerations (markdown rendering, long document handling)
    - Document known limitations: context window management for long production guides, session persistence
    - Reference the core prompt file by name (`google-flow-video-agent-prompt.md`)
    - _Requirements: 10.2, 10.6_

  - [x] 5.2 Create `Google-Flow/platform-gemini.md`
    - Deployment instructions via Custom Instructions and Gems
    - Formatting support notes
    - Document known limitations: custom instructions length limits, context window, session persistence
    - Reference the core prompt file by name (`google-flow-video-agent-prompt.md`)
    - _Requirements: 10.4, 10.6_

- [x] 6. Checkpoint — Verify all deliverables exist and structural integrity
  - Ensure all tests pass, ask the user if questions arise.
  - Verify all three files exist: `google-flow-video-agent-prompt.md`, `platform-kiro.md`, `platform-gemini.md`
  - Verify each platform guide contains setup instructions and a limitations section
  - Verify the core prompt is platform-agnostic (no platform names in the file)

