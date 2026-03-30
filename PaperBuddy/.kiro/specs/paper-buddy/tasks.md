# Implementation Plan: PaperBuddy Agent

## Overview

Build the PaperBuddy agent prompt and platform deployment files following the workspace's established agent structure. The core prompt is a markdown file with four XML-like sections (`<identity>`, `<methodology>`, `<output_formatting>`, `<interaction_patterns>`). Platform files follow the Perspective agent template.

## Tasks

- [x] 1. Create the core prompt file with identity and methodology sections
  - [x] 1.1 Create `PaperBuddy/paper-buddy-agent-prompt.md` with the `<identity>` section
    - Define agent persona as a research paper comprehension companion for senior data engineers
    - Include role definition, communication style, domain awareness (pipelines, orchestration, distributed systems, data modeling, streaming/batch, schema evolution, data quality)
    - Include off-topic handling: redirect non-paper requests, sensitive content warning, reject non-research content (fiction, opinion), handle incomplete papers
    - Include input acceptance scope: full text, excerpts, URLs with context, titles with abstracts, copied sections
    - _Requirements: 1.1, 7.1, 7.4, 8.1, 8.2, 8.3_

  - [x] 1.2 Add the `<methodology>` section to the core prompt
    - Input processing flow: validation rules for each input form, acknowledgment step (title, authors, topic), single clarifying question for ambiguous input, URL-only rejection, empty input rejection
    - Breakdown generation method for all four pillars: Core Problem, Why It Matters, How It Solves It, Takeaways for You
    - Jargon translation method: inline translations, dedicated Key Terms section threshold (5+ terms), no-unexplained-jargon rule
    - Quality assessment method: Strengths & Limitations in every breakdown, full assessment structure (methodology rigor, evidence quality, novelty, practical applicability)
    - Familiarity adaptation method: default low familiarity, increase/decrease signals, gradual per-conversation adaptation
    - Incomplete/redacted paper handling: generate from available content, state which pillars are incomplete
    - _Requirements: 1.2, 1.3, 1.4, 1.5, 2.1, 2.2, 2.3, 2.4, 2.5, 3.1, 3.2, 3.4, 5.1, 5.2, 5.3, 5.4, 6.1, 6.2, 6.3, 6.4, 8.4_

- [x] 2. Add output formatting and interaction patterns sections to the core prompt
  - [x] 2.1 Add the `<output_formatting>` section to the core prompt
    - Breakdown template with emoji headers: 📄 Paper Title, 🔍 Core Problem, 💡 Why It Matters, ⚙️ How It Solves It, 🎯 Takeaways for You, ⚖️ Strengths & Limitations
    - Authors and Field metadata under paper title
    - Jargon presentation rules: inline format and Key Terms section format
    - Quality assessment format for full evaluations (methodology rigor, evidence quality, novelty, practical applicability)
    - General formatting rules: markdown headers, bullet lists, concise pillars (3-8 sentences), takeaways as bullet points
    - _Requirements: 2.1, 2.6, 3.1, 6.1, 6.3_

  - [x] 2.2 Add the `<interaction_patterns>` section to the core prompt
    - Five follow-up conversation types: pillar deep dive, section/claim exploration, application questions, critical evaluation, beyond-paper questions
    - Source attribution rules: distinguish paper content from agent inference in all responses
    - Depth adaptation signals: increase triggers (domain terminology, advanced follow-ups, explicit statements) and decrease triggers (confusion, simpler explanation requests)
    - Session flow: greeting, post-breakdown invitation, multi-paper handling, context window management
    - _Requirements: 4.1, 4.2, 4.3, 4.4, 4.5, 5.2, 5.3, 5.4_

- [x] 3. Checkpoint — Review core prompt completeness
  - Review the full core prompt for consistency across all four sections, ask the user if questions arise.

- [x] 4. Create platform deployment files
  - [x] 4.1 Create `PaperBuddy/platform-kiro.md`
    - Follow the Perspective agent's `platform-kiro.md` template structure
    - Steering file setup at `.kiro/steering/paper-buddy-agent.md`
    - Platform notes for terminal-based output
    - Limitations section (context window, session persistence, session summaries)
    - "What This Wrapper Does NOT Do" disclaimer referencing `paper-buddy-agent-prompt.md`
    - _Requirements: 7.2, 7.3_

  - [x] 4.2 Create `PaperBuddy/platform-claude.md`
    - Follow the Perspective agent's `platform-claude.md` template structure
    - Option A: Projects (persistent agent) setup
    - Option B: Direct system message (quick start) setup
    - Platform notes for instruction-following and formatting
    - Limitations section (context window, session persistence)
    - "What This Wrapper Does NOT Do" disclaimer referencing `paper-buddy-agent-prompt.md`
    - _Requirements: 7.2, 7.3_

  - [x] 4.3 Create `PaperBuddy/platform-gemini.md`
    - Follow the Perspective agent's `platform-gemini.md` template structure
    - Option A: Custom Instructions (quick start) setup
    - Option B: Gems (dedicated agent) setup
    - Platform notes for formatting support
    - Limitations section (context window, custom instructions length, session persistence)
    - "What This Wrapper Does NOT Do" disclaimer referencing `paper-buddy-agent-prompt.md`
    - _Requirements: 7.2, 7.3_

- [x] 5. Final review — Verify all deliverables are complete
  - Confirm core prompt has all four XML sections with proper opening/closing tags
  - Confirm all three platform files exist and reference the core prompt
  - Confirm platform files contain no behavioral instructions
  - Ask the user if questions arise.

## Notes

- The core deliverables are markdown files, not software code — the "implementation" is prompt engineering
- Platform files follow the exact template from `Perspective/platform-*.md`
