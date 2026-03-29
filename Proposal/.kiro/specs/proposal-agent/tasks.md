# Implementation Plan: Proposal Agent

## Overview

Build the Proposal Agent as a core prompt file with platform wrappers, following the established repo pattern (Design/, Requirements/, Memories/, Mentor/). The core prompt defines all agent logic across five XML-style sections. Platform wrappers provide deployment instructions only. All files go in the `Proposal/` folder.

## Tasks

- [x] 1. Create the core prompt file with identity section
  - [x] 1.1 Create `Proposal/proposal-agent-prompt.md` with the `<identity>` section
    - Define the agent persona: experienced proposal strategist, domain-agnostic, collaborative
    - Set communication style: clear, precise, supportive, one question at a time, Socratic by default
    - Define domain agnosticism: adapt vocabulary and examples to the user's domain (product, process, code, organizational, policy, budget, or other)
    - Define off-topic handling: acknowledge warmly, redirect to proposal work, note connections to later phases if relevant
    - Follow the XML-style tag structure used by existing agents (see `design-agent-prompt.md`, `requirements-agent-prompt.md`)
    - _Requirements: 1.1, 3.1, 3.2, 3.3, 3.4, 11.1, 11.2, 11.3_

- [x] 2. Add methodology section with six-phase proposal drafting flow
  - [x] 2.1 Add the `<methodology>` section with Problem Framing phase
    - Define Problem Framing: help user articulate the problem or opportunity
    - Use Socratic questioning to surface unstated assumptions and sharpen the problem statement
    - Present problem summary using `[PROBLEM FRAMING]` state marker
    - _Requirements: 1.2, 5.1, 5.2, 6.1, 7.1, 7.2_

  - [x] 2.2 Add Context and Stakeholders phase to `<methodology>`
    - Prompt user to identify who is affected, who decides, constraints, and current state
    - Present context using `[CONTEXT]` state marker
    - _Requirements: 5.1, 5.3, 6.2_

  - [x] 2.3 Add Solution Proposal phase to `<methodology>`
    - Help user articulate proposed solution, key components, and expected outcomes
    - Present solution using `[SOLUTION]` state marker
    - _Requirements: 5.1, 5.4, 6.3_

  - [x] 2.4 Add Alternatives Analysis phase to `<methodology>`
    - Prompt user to identify at least two alternatives and articulate tradeoffs
    - Present alternatives using `[ALTERNATIVES]` state marker
    - _Requirements: 5.1, 5.5, 6.4_

  - [x] 2.5 Add Impact and Risks phase to `<methodology>`
    - Prompt user to identify benefits, risks, mitigation strategies, and success criteria
    - Present impact analysis using `[IMPACT]` state marker
    - _Requirements: 5.1, 5.6, 6.5_

  - [x] 2.6 Add Document Generation phase to `<methodology>`
    - Produce structured proposal document with: executive summary, problem statement, proposed solution, alternatives considered, impact analysis, success criteria, next steps
    - Present document using `[PROPOSAL DOCUMENT]` state marker
    - Use standard markdown formatting only (no HTML, LaTeX, Mermaid)
    - Handle partial completion: produce document with incompleteness notes when phases are incomplete
    - Handle edit requests: regenerate only affected sections
    - _Requirements: 5.1, 5.7, 5.9, 6.6, 10.1, 10.2, 10.3, 10.4_

- [x] 3. Add session protocol section
  - [x] 3.1 Add the `<session_protocol>` section to the core prompt
    - Define session initialization: warm greeting, ask what proposal to draft
    - Handle user who doesn't know where to start: offer prompts to surface a topic
    - Handle user who provides topic in first message: skip introductory questions, proceed to Problem Framing
    - Handle vague or overly broad topics: ask clarifying questions to narrow scope
    - Define phase progression through all six phases in order
    - Allow free navigation between phases at any time without blocking
    - Define new problem handling: offer to summarize previous proposal, restart from Problem Framing
    - Define session summary using `[SUMMARY]` state marker: proposal topic, phases completed, key decisions, open threads, restoration note
    - Define context window management: proactively offer summaries before context is lost
    - _Requirements: 1.3, 4.1, 4.2, 4.3, 4.4, 5.1, 5.8, 6.7, 9.1, 9.2, 9.3, 9.4_

- [x] 4. Add interaction patterns and formatting sections
  - [x] 4.1 Add the `<interaction_patterns>` section to the core prompt
    - Define Socratic questioning: ask clarifying question before offering interpretation, surface assumptions, one question at a time
    - Define direct recommendation handling: provide recommendation with reasoning when explicitly asked, then return to Socratic approach
    - Define vague input handling: targeted follow-ups for subjective language, offer concrete interpretations for ambiguous statements
    - Define stakeholder prompting: prompt user to consider perspectives beyond their own
    - Define domain adaptation: adapt vocabulary, examples, and elicitation to the user's domain
    - _Requirements: 1.4, 3.1, 3.2, 3.3, 3.4, 7.1, 7.2, 7.3, 7.4, 8.1, 8.2, 8.3_

  - [x] 4.2 Add the `<formatting>` section to the core prompt
    - Define all conversational state markers: `[PROBLEM FRAMING]`, `[CONTEXT]`, `[SOLUTION]`, `[ALTERNATIVES]`, `[IMPACT]`, `[PROPOSAL DOCUMENT]`, `[SUMMARY]`
    - Specify standard markdown only: headers, bold, italic, lists, fenced code blocks
    - Set platform-agnostic constraints: no HTML, no LaTeX, no Mermaid, no embedded images, no platform-specific formatting
    - Define terminal compatibility: reasonable line lengths, no wide tables, no deeply nested lists, blank lines between sections
    - _Requirements: 1.5, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7, 12.1, 12.2, 12.3, 12.4_

- [x] 5. Checkpoint — Core prompt review
  - Verify all five XML-style sections are present: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
  - Verify all seven state markers are defined
  - Verify six methodology phases appear in correct order
  - Verify the prompt file is stored at `Proposal/proposal-agent-prompt.md`
  - Ask the user if questions arise.

- [x] 6. Create platform wrapper files
  - [x] 6.1 Create `Proposal/platform-claude.md`
    - Follow the pattern from `Design/platform-claude.md`
    - Include setup options: Projects (persistent) and Direct System Message (quick start)
    - Reference `proposal-agent-prompt.md` as the single source of agent logic
    - Include platform notes on formatting support, limitations, and context window considerations
    - Note the agent's state markers (`[PROBLEM FRAMING]`, `[CONTEXT]`, `[SOLUTION]`, `[ALTERNATIVES]`, `[IMPACT]`, `[PROPOSAL DOCUMENT]`, `[SUMMARY]`)
    - Include "What This Wrapper Does NOT Do" section confirming no behavior modification
    - _Requirements: 2.1, 2.4, 2.5_

  - [x] 6.2 Create `Proposal/platform-gemini.md`
    - Follow the pattern from `Design/platform-gemini.md`
    - Include setup options: Custom Instructions (quick start) and Gems (dedicated agent)
    - Reference `proposal-agent-prompt.md` as the single source of agent logic
    - Include platform notes on formatting support, limitations, and context window considerations
    - Include "What This Wrapper Does NOT Do" section confirming no behavior modification
    - _Requirements: 2.2, 2.4, 2.5_

  - [x] 6.3 Create `Proposal/platform-kiro.md`
    - Follow the pattern from `Design/platform-kiro.md`
    - Include setup via steering file (`.kiro/steering/proposal-agent.md`)
    - Reference `proposal-agent-prompt.md` as the single source of agent logic
    - Include terminal-specific notes on output formatting
    - Include "What This Wrapper Does NOT Do" section confirming no behavior modification
    - _Requirements: 2.3, 2.4, 2.5_

- [x] 7. Final checkpoint — All files created
  - Ensure all four files exist in `Proposal/`: `proposal-agent-prompt.md`, `platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`. Ask the user if questions arise.

## Notes

- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation
- The core prompt and platform wrappers follow the exact patterns established by Design/, Requirements/, Memories/, and Mentor/ agents
