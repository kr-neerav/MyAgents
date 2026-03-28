# Implementation Plan: Design Agent

## Overview

Build the Design Agent core prompt section by section, then the three platform wrappers. Each task builds incrementally — the core prompt is assembled section by section following the XML structure pattern established by existing agents (Requirements, Mentor, Learning, etc.). Platform wrappers follow the established pattern from `Requirements/platform-claude.md`, `Requirements/platform-gemini.md`, and `Requirements/platform-kiro.md`.

## Tasks

- [x] 1. Create core prompt file with `<identity>` section
  - Create `Design/design-agent-prompt.md`
  - Write the `<identity>` section defining the Design Agent persona: role as a domain-agnostic design companion, communication style, domain agnosticism rules, and off-topic handling
  - Use the Requirements Agent (`Requirements/requirements-agent-prompt.md`) and Mentor Agent (`Mentor/mentor-agent-prompt.md`) as structural references for tone, depth, and XML section formatting
  - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 10.1, 10.2_

- [x] 2. Add `<methodology>` section to core prompt
  - [x] 2.1 Write the five-phase design methodology
    - Add the `<methodology>` section to `Design/design-agent-prompt.md`
    - Define the five ordered phases: Understand (clarifying questions, produce `[DESIGN CONTEXT]`), Decompose (components, responsibilities, relationships), Detail (interfaces, behaviors, internal structure, constraints), Validate (completeness, consistency, unresolved tradeoffs), Document (structured design output)
    - Include Socratic questioning guidance within each phase
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 2.7, 2.8_

  - [x] 2.2 Add tradeoff analysis subsection
    - Define the `[TRADEOFF]` marker usage: competing qualities, options, consequences, optional recommendation
    - Include rules for proactive tradeoff surfacing and recording decisions with rationale
    - _Requirements: 3.1, 3.2, 3.3, 3.4_

  - [x] 2.3 Add domain adaptation subsection
    - Define domain-specific design concerns: software (API design, data modeling, concurrency, error handling, deployment, observability), business process (roles, decision points, escalation, handoffs, metrics), product (user journeys, interaction patterns, information architecture, feedback loops), hardware (material constraints, manufacturing, tolerances, safety, failure modes)
    - Include multi-domain blending and domain ambiguity handling
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 4.6_

  - [x] 2.4 Add Requirements Agent handoff subsection
    - Define how the agent accepts structured requirements documents (FR-xxx, NFR-xxx, CON-xxx identifiers)
    - Include rules for adopting established terminology and handling incomplete requirements
    - _Requirements: 11.1, 11.2, 11.3, 11.4_

- [x] 3. Add `<session_protocol>` section to core prompt
  - Add the `<session_protocol>` section to `Design/design-agent-prompt.md`
  - Define session initialization (greet, ask what to design, handle Requirements_Input)
  - Define phase progression with free navigation between phases
  - Define new problem handling (acknowledge transition, offer summary, restart from Understand)
  - Define session summary using `[SUMMARY]` marker with design context, components, decisions, open questions, and restoration note
  - Define context window management with proactive summary offers
  - _Requirements: 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7_

- [x] 4. Add `<interaction_patterns>` section to core prompt
  - Add the `<interaction_patterns>` section to `Design/design-agent-prompt.md`
  - Define Socratic questioning rules: ask before telling, surface assumptions, explore consequences
  - Define constraint surfacing: failure modes, edge cases, boundary conditions
  - Define vague input handling: targeted follow-up questions for specificity
  - Define stakeholder prompting: perspectives beyond the user's own
  - Define direct recommendation handling: clear recommendation with reasoning when asked, then return to Socratic approach
  - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7_

- [x] 5. Add `<formatting>` section to core prompt
  - Add the `<formatting>` section to `Design/design-agent-prompt.md`
  - Define all seven conversational state markers: `[DESIGN CONTEXT]`, `[DECOMPOSE]`, `[DETAIL]`, `[TRADEOFF]`, `[VALIDATE]`, `[DOCUMENT]`, `[SUMMARY]`
  - Define markdown rules: standard markdown only, no HTML/LaTeX/platform-specific features
  - Define terminal compatibility: reasonable line lengths, no wide tables, limited nesting depth
  - _Requirements: 7.1, 7.2, 7.3, 8.3, 10.4_

- [x] 6. Checkpoint — Review core prompt completeness
  - Ensure all five XML-like sections are present in `Design/design-agent-prompt.md`: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
  - Verify the prompt follows the structural conventions of existing agents in the repository

- [x] 7. Create platform wrapper files
  - [x] 7.1 Create `Design/platform-claude.md`
    - Write Claude deployment guide with two setup options: Projects (persistent) and direct system message (quick start)
    - Include Platform Notes section covering instruction-following and formatting support
    - Include Limitations section covering context window, session persistence, and direct system message caveats
    - Include "What This Wrapper Does NOT Do" section stating the wrapper does not modify agent behavior
    - Reference `design-agent-prompt.md` as the core prompt file
    - Use `Requirements/platform-claude.md` as the structural reference
    - _Requirements: 9.1, 9.4, 9.5, 9.6, 10.3, 10.5_

  - [x] 7.2 Create `Design/platform-gemini.md`
    - Write Gemini deployment guide with two setup options: Custom Instructions (quick start) and Gems (dedicated agent)
    - Include Platform Notes section covering formatting support
    - Include Limitations section covering context window, custom instructions length, and session persistence
    - Include "What This Wrapper Does NOT Do" section stating the wrapper does not modify agent behavior
    - Reference `design-agent-prompt.md` as the core prompt file
    - Use `Requirements/platform-gemini.md` as the structural reference
    - _Requirements: 9.2, 9.4, 9.5, 9.6, 10.3, 10.5_

  - [x] 7.3 Create `Design/platform-kiro.md`
    - Write Kiro CLI deployment guide with steering file setup at `.kiro/steering/design-agent.md`
    - Include Platform Notes section covering terminal-based output considerations
    - Include Limitations section covering context window, session persistence, and scrollback
    - Include "What This Wrapper Does NOT Do" section stating the wrapper does not modify agent behavior
    - Reference `design-agent-prompt.md` as the core prompt file
    - Use `Requirements/platform-kiro.md` as the structural reference
    - _Requirements: 9.3, 9.4, 9.5, 9.6, 10.3, 10.5_

- [x] 8. Checkpoint — Review all deliverable files
  - Verify all four files exist: `Design/design-agent-prompt.md`, `Design/platform-claude.md`, `Design/platform-gemini.md`, `Design/platform-kiro.md`
  - Verify platform wrappers each reference `design-agent-prompt.md`
  - Verify platform wrappers each contain setup options, platform notes, limitations, and "What This Wrapper Does NOT Do" sections

## Notes

- The core prompt is built incrementally section by section (tasks 1–5) to allow validation at each step
- Existing agents in the repository (`Requirements/`, `Mentor/`, `Learning/`, `Memories/`, `Mythology/`) serve as reference implementations for both the core prompt structure and platform wrapper format
- Each task references specific requirements for traceability
