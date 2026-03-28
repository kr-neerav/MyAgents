# Implementation Plan: Requirements Agent

## Overview

Create the Requirements Agent — a system prompt agent that guides users through structured requirements elicitation, organization, refinement, and document generation. Deliverables are markdown files: a core system prompt, three platform wrappers, and a README update. All content follows the established repository patterns (Learning Agent, Mentor Agent, etc.).

## Tasks

- [x] 1. Create the core system prompt file
  - [x] 1.1 Create `Requirements/requirements-agent-prompt.md` with the `<identity>` section
    - Define persona as experienced requirements engineer and systems analyst with cross-domain expertise (software, hardware, process, product, organizational, mechanism design)
    - Include role statement, communication style rules (clear, precise, calibrated to user's domain, direct and supportive, avoids jargon until introduced)
    - Include domain agnosticism statement — agent works across all problem domains, not just software
    - Include off-topic handling rules — acknowledge warmly and redirect to requirements domain
    - Follow the same structure as `Learning/learning-agent-prompt.md` `<identity>` section
    - _Requirements: 1.1, 2.1, 2.2, 2.3, 2.4, 2.5_

  - [x] 1.2 Add the `<methodology>` section to the core prompt
    - **Problem Scoping Flow**: Clarifying questions for domain/stakeholders/goals/constraints, Socratic questioning for unconsidered aspects, scope narrowing for vague problems, `[PROBLEM SCOPE]` marker with problem statement summary, fast-track for well-defined problems
    - **Requirements Elicitation**: `[ELICIT]` marker, prompts for functional/non-functional/constraints/assumptions/dependencies, Socratic questioning for implicit requirements, follow-up for vague requirements, stakeholder perspective prompting, edge case and failure mode prompting
    - **Requirements Organization**: Categorization into functional/non-functional/constraints/assumptions/dependencies/out-of-scope, `[ORGANIZE]` marker, unique identifier assignment (FR-001, NFR-001, CON-001), grouping related requirements, clarification for ambiguous categorization
    - **Requirements Refinement**: Quality review for clarity/testability/completeness/consistency, `[REFINE]` marker, specific improvement suggestions with explanations, user confirmation before incorporating changes
    - **Document Generation**: `[DOCUMENT]` marker, sections for problem statement/glossary/categorized requirements with IDs/assumptions/constraints/dependencies/out-of-scope, standard markdown formatting, incremental regeneration on feedback, early generation with incompleteness notes
    - _Requirements: 1.2, 3.1, 3.2, 3.3, 3.4, 3.5, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 5.1, 5.2, 5.3, 5.4, 5.5, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7, 7.1, 7.2, 7.3, 7.4, 7.5_

  - [x] 1.3 Add the `<session_protocol>` section to the core prompt
    - **Session initialization**: Warm greeting, ask what problem to define requirements for, offer prompts if user is unsure (pain points, upcoming projects, decisions)
    - **Phase progression**: Problem scoping → Elicitation → Organization → Refinement → Document generation, with free navigation between phases
    - **New problem handling**: Acknowledge transition, restart from problem scoping
    - **Session summary**: `[SUMMARY]` marker, includes problems scoped/requirements count and categories/refinements made/open threads, formatted for paste-to-restore-context
    - **Context window management**: Proactive summary offers when conversation grows long
    - _Requirements: 1.3, 10.1, 10.2, 10.3, 10.4, 10.5, 11.1, 11.2, 11.3, 11.4_

  - [x] 1.4 Add the `<interaction_patterns>` section to the core prompt
    - **Socratic questioning**: Ask before telling, surface implicit requirements through questions, give user time to think
    - **Vague input handling**: Targeted follow-up questions to make requirements specific and testable
    - **Stakeholder prompting**: "Who else is affected?" questions to broaden perspective
    - **Edge case prompting**: "What happens when X fails?" questions to surface failure modes and unwanted scenarios
    - **Domain adaptation**: Adjust elicitation questions and categories based on detected domain — software (performance/security/APIs), process (inputs/outputs/triggers/roles), product (UX/market/regulatory)
    - _Requirements: 1.4, 4.3, 4.4, 4.5, 4.6, 8.1, 8.2, 8.3, 8.4, 8.5_

  - [x] 1.5 Add the `<formatting>` section to the core prompt
    - Define conversational state markers: `[PROBLEM SCOPE]`, `[ELICIT]`, `[ORGANIZE]`, `[REFINE]`, `[DOCUMENT]`, `[SUMMARY]`
    - Markdown rules: Standard markdown only (headers, lists, code blocks, bold, italic), no HTML/LaTeX/platform-specific features
    - Terminal compatibility: Reasonable line lengths, no wide tables, no deep nesting beyond 2-3 levels
    - Unique identifier format for requirements (FR-001, NFR-001, CON-001, ASM-001, DEP-001)
    - _Requirements: 1.5, 9.1, 9.2, 9.3, 9.4, 9.5, 9.6, 15.4_

- [x] 2. Checkpoint — Review core prompt completeness
  - Ensure the core prompt file exists at `Requirements/requirements-agent-prompt.md`
  - Verify all five XML sections are present with opening and closing tags: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
  - Verify all six conversational state markers are referenced
  - Verify all six requirement categories are referenced
  - Verify domain-specific elicitation guidance for software, process, and product domains
  - Ensure all tests pass, ask the user if questions arise.

- [x] 3. Create platform wrapper for Claude
  - Create `Requirements/platform-claude.md` following the structure of `Learning/platform-claude.md`
  - Title: "Requirements Agent — Claude Setup Guide"
  - Intro paragraph referencing `requirements-agent-prompt.md` as the core prompt
  - Setup Option A: Projects (persistent agent) — step-by-step instructions
  - Setup Option B: Direct System Message (quick start) — step-by-step instructions
  - Platform Notes: Instruction-following characteristics, formatting support, limitations (context window, session persistence, direct message approach)
  - "What This Wrapper Does NOT Do" section — states wrapper does not modify agent behavior, all requirements engineering logic lives in the core prompt
  - _Requirements: 12.1, 12.2, 12.3, 12.4, 12.5_

- [x] 4. Create platform wrapper for Gemini
  - Create `Requirements/platform-gemini.md` following the structure of `Learning/platform-gemini.md`
  - Title: "Requirements Agent — Gemini Chat Setup Guide"
  - Intro paragraph referencing `requirements-agent-prompt.md` as the core prompt
  - Setup Option A: Custom Instructions (quick start) — step-by-step instructions
  - Setup Option B: Gems (dedicated agent) — step-by-step instructions
  - Platform Notes: Formatting support, limitations (context window, custom instructions length, session persistence)
  - "What This Wrapper Does NOT Do" section — states wrapper does not modify agent behavior
  - _Requirements: 13.1, 13.2, 13.3, 13.4, 13.5_

- [x] 5. Create platform wrapper for Kiro CLI
  - Create `Requirements/platform-kiro.md` following the structure of `Learning/platform-kiro.md`
  - Title: "Requirements Agent — Kiro CLI Setup Guide"
  - Intro paragraph referencing `requirements-agent-prompt.md` as the core prompt
  - Setup: Deploy via steering file at `.kiro/steering/requirements-agent.md` — step-by-step instructions
  - Platform Notes: Terminal-based output considerations, limitations (context window, session persistence, scrollback)
  - "What This Wrapper Does NOT Do" section — states wrapper does not modify agent behavior
  - _Requirements: 14.1, 14.2, 14.3, 14.4, 14.5_

- [x] 6. Checkpoint — Review platform wrappers
  - Verify all three platform wrapper files exist at correct paths
  - Verify each wrapper contains setup instructions, Platform Notes section, and "What This Wrapper Does NOT Do" section
  - Verify wrappers reference `requirements-agent-prompt.md` as the core prompt file
  - Verify wrappers do not contain any behavioral logic — all agent behavior lives in the core prompt
  - Ensure all tests pass, ask the user if questions arise.

- [x] 7. Update README.md with Requirements Agent entry
  - Add a new section to `README.md` following the existing pattern for other agents
  - Entry should link to `Requirements/requirements-agent-prompt.md` as the main link
  - Include a brief description: requirements engineering companion, structured elicitation/organization/refinement/document generation, domain-agnostic, Socratic questioning, conversational state markers
  - Include platform setup guide links for [Gemini](Requirements/platform-gemini.md), [Kiro CLI](Requirements/platform-kiro.md), and [Claude](Requirements/platform-claude.md) — matching the link order used by existing agents
  - _Requirements: 15.1, 15.2_

- [x] 8. Final checkpoint — Verify all deliverables
  - Verify `Requirements/requirements-agent-prompt.md` exists and contains all five XML sections
  - Verify `Requirements/platform-claude.md`, `Requirements/platform-gemini.md`, `Requirements/platform-kiro.md` exist with correct structure
  - Verify `README.md` contains the Requirements Agent entry with correct links
  - Verify file naming follows the `{agent-name}-prompt.md` convention (Requirement 15.4)
  - Verify `Requirements/.kiro/` subfolder exists (Requirement 15.3)
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- All deliverables are markdown files — there is no code to compile or deploy
- The core prompt is the largest deliverable; sub-tasks 1.1–1.5 break it into manageable sections by XML tag
- Platform wrappers follow a strict template from existing agents — no behavioral logic allowed in wrappers
- Each task references specific requirements for traceability back to `requirements.md`
- Checkpoints ensure incremental validation of deliverables before moving forward
