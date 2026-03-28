# Implementation Plan: Memory Email Agent

## Overview

Build the Memory Email Agent as a core prompt file with platform wrappers, following the established repo pattern (Learning/, Mentor/, Mythology/). The core prompt defines all agent logic across five XML-style sections. Platform wrappers provide deployment instructions only. Property-based tests (fast-check) validate prompt output structure. All files go in the `Memories/` folder.

## Tasks

- [x] 1. Create the core prompt file with identity and session protocol sections
  - [x] 1.1 Create `Memories/memory-email-agent-prompt.md` with the `<identity>` section
    - Define the agent persona: warm, supportive writing companion for a parent capturing memories
    - Set communication style: conversational, encouraging, concise, never judgmental
    - Define off-topic handling: gentle redirect back to memory capture
    - Follow the XML-style tag structure used by existing agents (see `learning-agent-prompt.md`, `mythology-scholar-prompt.md`)
    - _Requirements: 1.1, 1.2, 4.2_

  - [x] 1.2 Add the `<session_protocol>` section to the core prompt
    - Define session initialization: greeting and asking for a memory
    - Define phase transition logic with state markers: `[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`
    - Intake → Follow-Up: triggered on non-empty memory description
    - Follow-Up → Subject Selection: triggered when all questions answered or skipped
    - Subject Selection → Subject Selection: triggered when parent requests different options
    - Subject Selection → Generation: triggered when parent selects a subject line
    - Generation → Review: triggered immediately after draft produced
    - Review → Generation: triggered on edit request
    - Review → Finalized: triggered on approval
    - Finalized → Intake: triggered when parent starts a new memory
    - Handle empty/blank input: prompt parent to provide a description before proceeding
    - _Requirements: 1.1, 1.3, 3.5, 4.1, 4.3, 4.8, 5.1, 5.4_

- [x] 2. Add methodology and interaction patterns sections
  - [x] 2.1 Add the `<methodology>` section with follow-up question generation logic
    - Define context dimensions to analyze: sensory details, emotions, setting/time, who was present, what happened next, why it matters
    - Generate 1–3 questions based on missing dimensions, each with 3–5 multiple-choice options
    - Options must be contextually derived from the memory description, not generic
    - Include a "Skip" option per question and a "Skip all remaining" escape hatch
    - Define the follow-up question output format (labeled options a–e, `[FOLLOW-UP]` marker)
    - _Requirements: 3.1, 3.2, 3.3, 3.4, 3.5_

  - [x] 2.2 Add email generation rules to the `<methodology>` section
    - Before generating the full email, present 4–5 subject line options derived from the memory context
    - Include a "Suggest different options" choice so the parent can request new subject lines
    - Loop subject line presentation until the parent selects one
    - Use the parent's selected subject line in the generated Memory_Email
    - Define the subject line options output format (labeled options a–e/f, `[SUBJECT SELECTION]` marker)
    - Write in warm, heartfelt tone addressed to the daughter
    - Use HTML formatting: `<h2>` subject line, `<p>` paragraphs, `<em>`/`<strong>` for emphasis, `<br/>` for sign-off
    - Follow the email template structure: selected subject line, greeting, opening, body narrative, closing reflection, sign-off
    - Incorporate all details from intake + follow-up answers into the narrative
    - Reference media attachments inline at narratively appropriate points using `[Photo: ...]` / `[Video: ...]` format
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 2.3_

  - [x] 2.3 Add the `<interaction_patterns>` section for edit loop and media handling
    - Define edit suggestion format: multiple-choice options (approve, more emotional, shorten, add detail, change tone, free-text)
    - Regenerate email incorporating selected change, loop until parent approves
    - Define media handling: acknowledge attachments, associate with memory, support JPEG/PNG/GIF and MP4/MOV
    - Handle unsupported media formats gracefully
    - Handle ambiguous edit requests with clarifying follow-up
    - _Requirements: 5.1, 5.2, 5.3, 5.4, 2.1, 2.2, 2.4_

- [x] 3. Add formatting section and finalize core prompt
  - [x] 3.1 Add the `<formatting>` section to the core prompt
    - Define all conversational state markers: `[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`
    - Specify HTML email formatting rules (email body uses HTML; conversation uses markdown)
    - Set platform-agnostic constraints: no Mermaid, no LaTeX, no platform-specific features
    - Define terminal compatibility rules: reasonable line lengths, no wide tables, clear section separation
    - _Requirements: 4.4, 6.1_

  - [x] 3.2 Review and validate the complete core prompt
    - Verify all five XML-style sections are present: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
    - Verify the prompt references all supported media formats (JPEG, PNG, GIF, MP4, MOV)
    - Verify empty input handling is defined
    - Verify the conversation flow covers all five phases (intake, follow-up, subject selection, generation, review/edit)
    - _Requirements: 1.3, 2.2, 6.1_

- [x] 4. Checkpoint — Core prompt review
  - Ensure the core prompt file is complete and structurally sound, ask the user if questions arise.

- [x] 5. Create platform wrapper files
  - [x] 5.1 Create `Memories/platform-claude.md`
    - Follow the pattern from `Learning/platform-claude.md` and `Mythology/platform-claude.md`
    - Include setup options: Projects (persistent) and Direct System Message (quick start)
    - Reference `memory-email-agent-prompt.md` as the single source of agent logic
    - Include platform notes on formatting support, limitations, and context window considerations
    - Note the agent's state markers (`[INTAKE]`, `[FOLLOW-UP]`, `[SUBJECT SELECTION]`, `[EMAIL DRAFT]`, `[REVIEW]`, `[FINALIZED]`)
    - Include "What This Guide Does NOT Do" section confirming no behavior modification
    - _Requirements: 6.2, 6.3, 6.4, 6.5, 6.6, 6.7_

  - [x] 5.2 Create `Memories/platform-kiro.md`
    - Follow the pattern from `Learning/platform-kiro.md`
    - Include setup via steering file (`.kiro/steering/memory-email-agent.md`)
    - Reference `memory-email-agent-prompt.md` as the single source of agent logic
    - Include terminal-specific notes on output formatting
    - Include "What This Guide Does NOT Do" section confirming no behavior modification
    - _Requirements: 6.2, 6.3, 6.4, 6.5, 6.6, 6.7_

  - [x] 5.3 Create `Memories/platform-gemini.md`
    - Follow the pattern from `Learning/platform-gemini.md`
    - Include setup options: Custom Instructions (quick start) and Gems (dedicated agent)
    - Reference `memory-email-agent-prompt.md` as the single source of agent logic
    - Include platform notes on formatting support, limitations, and context window considerations
    - Include "What This Guide Does NOT Do" section confirming no behavior modification
    - _Requirements: 6.2, 6.3, 6.4, 6.5, 6.6, 6.7_

- [x] 6. Checkpoint — All files created
  - Ensure all four files exist in `Memories/`: `memory-email-agent-prompt.md`, `platform-claude.md`, `platform-kiro.md`, `platform-gemini.md`. Ask the user if questions arise.

- [x] 7. Set up testing infrastructure and write property-based tests
  - [x] 7.1 Set up fast-check test file for the Memory Email Agent
    - Create test file (e.g., `Memories/memory-email-agent.test.ts` or similar)
    - Configure fast-check with minimum 100 iterations per property
    - Define generators for random memory descriptions, media attachment sets, and follow-up selections
    - Tag tests with `Feature: memory-email-agent, Property {N}: {title}`
    - _Requirements: 6.1_

  - [ ]* 7.2 Write property test: Empty input rejection (Property 1)
    - **Property 1: Empty input rejection**
    - For any whitespace-only or empty string as memory description, verify the prompt's output rules would reject it (no `[FOLLOW-UP]` or `[EMAIL DRAFT]` marker, contains a prompt to provide a description)
    - **Validates: Requirements 1.3**

  - [ ]* 7.3 Write property test: Follow-up question structure (Property 2)
    - **Property 2: Follow-up question structure**
    - For any valid memory description, verify the follow-up output format contains 1–3 questions, each with 3–5 labeled options
    - **Validates: Requirements 3.1, 3.2**

  - [ ]* 7.4 Write property test: Skip mechanisms in follow-ups (Property 3)
    - **Property 3: Skip mechanisms in follow-ups**
    - For any follow-up question set, verify each question includes a "Skip" option and the output includes a "skip all" mechanism
    - **Validates: Requirements 3.4, 3.5**

  - [ ]* 7.5 Write property test: Follow-up answer incorporation (Property 4)
    - **Property 4: Follow-up answer incorporation**
    - For any selected follow-up option, verify the generated email body references or reflects the detail from that option
    - **Validates: Requirements 3.3**

  - [ ]* 7.6 Write property test: Media reference inclusion (Property 5)
    - **Property 5: Media reference inclusion**
    - For any set of N media attachments, verify the generated email contains N inline references in `[Photo: ...]` or `[Video: ...]` format
    - **Validates: Requirements 2.3**

  - [ ]* 7.7 Write property test: Email structural completeness (Property 6)
    - **Property 6: Email structural completeness**
    - For any generated email, verify it contains: subject line, greeting, narrative body, closing with sign-off, and HTML formatting tags
    - **Validates: Requirements 4.6, 4.7**

  - [ ]* 7.8 Write property test: Subject line selection option count (Property 7)
    - **Property 7: Subject line selection option count**
    - For any subject line selection output, verify it presents 4–5 subject line options plus a "Suggest different options" choice
    - **Validates: Requirements 4.1, 4.2**

  - [ ]* 7.9 Write property test: Review phase options (Property 8)
    - **Property 8: Review phase options**
    - For any email draft review output, verify it includes options to approve, edit, and regenerate
    - **Validates: Requirements 4.8, 5.1**

  - [ ]* 7.10 Write property test: Edit suggestion structure (Property 9)
    - **Property 9: Edit suggestion structure**
    - For any edit request, verify the agent presents multiple-choice edit suggestions (tone, length, detail) plus a free-text option
    - **Validates: Requirements 5.2**

  - [ ]* 7.11 Write property test: Finalization marking (Property 10)
    - **Property 10: Finalization marking**
    - For any approved email, verify the output contains a `[FINALIZED]` marker and the complete final email
    - **Validates: Requirements 5.4**

  - [ ]* 7.12 Write property test: Platform wrapper core prompt reference (Property 11)
    - **Property 11: Platform wrapper core prompt reference**
    - For each platform wrapper file, verify it contains a reference to `memory-email-agent-prompt.md`
    - **Validates: Requirements 6.5**

- [x] 8. Final checkpoint — All tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation
- Property tests validate structural correctness of prompt output formats using fast-check
- The core prompt and platform wrappers follow the exact patterns established by Learning/, Mentor/, and Mythology/ agents
