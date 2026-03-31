# Implementation Plan: Status Update Agent

## Overview

Build the Status Update Agent prompt and platform deployment files following the workspace's established agent structure. The core prompt is a markdown file with four XML-like sections (`<identity>`, `<methodology>`, `<output_formatting>`, `<interaction_patterns>`). Platform files follow the existing agent template (Perspective, PaperBuddy, etc.).

Each XML section is written as a separate file first, then merged into the final prompt. This avoids context issues with generating the entire prompt in one pass.

## Tasks

- [x] 1. Create the `<identity>` section as a standalone file
  - [x] 1.1 Create `StatusUpdates/_identity.md` containing the full `<identity>` section
    - Define agent persona as an opinionated writing partner who knows org status update conventions — direct, efficient, allergic to fluff
    - Include role definition: accepts raw project information, produces polished status updates in three formats (Weekly Status Report, Quick Update, Sprint Summary), supports iterative refinement
    - Include communication style: direct and efficient, uses the same writing standards it enforces (active voice, short sentences, no weasel words), confirms inputs concisely, asks one clarifying question at a time
    - Include company writing expertise: active voice always, weasel word list ("believe", "in general", "planning on", "hope to", "trying to", "soon", "some", "most", "quickly", "adequate", "reasonable"), short sentences, specific metrics, recommendations alongside problems
    - Include off-topic handling: redirect non-status-update requests warmly, sensitive content warning, reject non-project content with explanation, handle edge cases
    - _Requirements: 1.1, 5.1, 5.2, 5.7, 7.1, 7.4, 8.1, 8.2, 8.3_

- [x] 2. Create the `<methodology>` section as a standalone file
  - [x] 2.1 Create `StatusUpdates/_methodology.md` containing the full `<methodology>` section
    - Input processing flow: validation rules for each accepted input form (free-text bullet points, pasted Slack messages, pasted meeting notes, structured lists, conversational descriptions), acknowledgment step (project name, update type, time period), single clarifying question for ambiguous input, empty input rejection, default to Weekly Status Report when type not specified
    - Weekly Status Report generation method (Kurt Kufeld VP format): subject line construction, status color determination (GREEN/YELLOW/RED with distinct criteria), executive summary generation (4-5 sentences max, must stand alone), section population (Red/Yellow Flags, Missed This Week, Upcoming This Week, Everything Else), owner assignment (specific human, not team name), delivery date trajectory
    - Quick Update generation method: 2-5 sentences, strong opening sentence, audience adaptation (executive/technical/cross-functional), default to executive framing
    - Sprint Summary generation method: Sprint Goal Outcome (met/partially met/not met), Completed Work, Carried-Over Items (with reason and owner), Key Blockers or Risks, Next Sprint Goals (specific and measurable)
    - Writing standards enforcement method: active voice pass, weasel word elimination, sentence structure pass, specificity pass, recommendations pass, jargon check per audience type
    - Audience adaptation method: executive (lead with the ask, business impact), technical (include technical context, methodology), cross-functional (define terms, map dependencies)
    - Draft review method: identify weasel words, passive voice, missing owners, vague timelines — present as marked-up list, not rewritten draft
    - _Requirements: 1.1, 1.2, 1.3, 1.4, 1.5, 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 2.7, 2.8, 3.1, 3.2, 3.3, 3.4, 4.1, 4.2, 4.3, 4.4, 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8, 6.5_

- [x] 3. Create the `<output_formatting>` section as a standalone file
  - [x] 3.1 Create `StatusUpdates/_output_formatting.md` containing the full `<output_formatting>` section
    - Weekly Status Report template: Subject line format `[Status Report] [RED/YELLOW/GREEN] [Project Name] [MM/DD/YYYY]`, Executive Summary, Red/Yellow Flags, Missed This Week, Upcoming This Week, Everything Else — each with owner fields and specific formatting
    - Quick Update template: 2-5 sentences of clean prose, no headers or bullet points, suitable for Slack or email body
    - Sprint Summary template: Sprint Goal Outcome, Completed, Carried Over (with reason and owner), Key Blockers/Risks (with owner and next step), Next Sprint Goals (with owner)
    - Draft review output format: marked-up list of weasel words, passive voice, missing owners, vague timelines, and other violations with suggested corrections
    - Writing standards formatting rules: active voice, no weasel words, short sentences, named human owners, specific metrics/dates, recommendations with every problem, inline text (not attachments)
    - _Requirements: 2.1, 2.2, 2.3, 2.5, 2.6, 3.1, 3.2, 4.1, 4.2, 4.3, 4.4, 5.1, 5.2, 5.3, 5.5, 5.6, 6.5_

- [x] 4. Create the `<interaction_patterns>` section as a standalone file
  - [x] 4.1 Create `StatusUpdates/_interaction_patterns.md` containing the full `<interaction_patterns>` section
    - Refinement interactions: edit requests, additional information incorporation (highlight what changed), audience switch, type switch (e.g., Weekly to Quick Update), draft review workflow
    - Multi-project handling: generate separate status sections per project, clearly delineate project boundaries
    - Contradiction handling: flag contradictory information, ask for clarification before generating
    - Session flow: new session greeting, post-generation refinement invitation, multiple updates in one session support
    - _Requirements: 6.1, 6.2, 6.3, 6.4, 6.5, 8.4, 8.5_

- [x] 5. Merge all sections into the final core prompt file
  - [x] 5.1 Create `StatusUpdates/status-update-agent-prompt.md` by combining all four section files in order: `_identity.md`, `_methodology.md`, `_output_formatting.md`, `_interaction_patterns.md`
    - Add a top-level heading `# Status Update Agent`
    - Concatenate the four section files in order, preserving all content and XML tags
    - Verify all four XML sections have proper opening and closing tags
    - Verify writing standards enforcement is consistent across methodology and output formatting
    - Verify all three update types have both generation methods and output templates
  - [x] 5.2 Delete the four temporary section files (`_identity.md`, `_methodology.md`, `_output_formatting.md`, `_interaction_patterns.md`)

- [x] 6. Checkpoint — Review core prompt completeness
  - Review the full core prompt for consistency across all four sections
  - Verify all four XML sections have proper opening and closing tags
  - Verify all requirements are addressed in the prompt content
  - Ensure writing standards enforcement is consistent across methodology and output formatting
  - Ensure all three update types have both generation methods and output templates
  - Ask the user if questions arise.

- [x] 7. Create platform deployment files
  - [x] 7.1 Create `StatusUpdates/platform-kiro.md`
    - Follow the existing agent `platform-kiro.md` template structure (Perspective, PaperBuddy)
    - Steering file setup at `.kiro/steering/status-update-agent.md`
    - Platform notes for terminal-based output
    - Limitations section (context window, session persistence)
    - "What This Wrapper Does NOT Do" disclaimer referencing `status-update-agent-prompt.md`
    - _Requirements: 7.2, 7.3_

  - [x] 7.2 Create `StatusUpdates/platform-claude.md`
    - Follow the existing agent `platform-claude.md` template structure
    - Option A: Projects (persistent agent) setup
    - Option B: Direct system message (quick start) setup
    - Platform notes for instruction-following and formatting
    - Limitations section (context window, session persistence)
    - "What This Wrapper Does NOT Do" disclaimer referencing `status-update-agent-prompt.md`
    - _Requirements: 7.2, 7.3_

  - [x] 7.3 Create `StatusUpdates/platform-gemini.md`
    - Follow the existing agent `platform-gemini.md` template structure
    - Option A: Custom Instructions (quick start) setup
    - Option B: Gems (dedicated agent) setup
    - Platform notes for formatting support
    - Limitations section (context window, custom instructions length, session persistence)
    - "What This Wrapper Does NOT Do" disclaimer referencing `status-update-agent-prompt.md`
    - _Requirements: 7.2, 7.3_

- [x] 8. Final review — Verify all deliverables are complete
  - Confirm core prompt has all four XML sections (`<identity>`, `<methodology>`, `<output_formatting>`, `<interaction_patterns>`) with proper opening/closing tags
  - Confirm all three platform files exist in `StatusUpdates/` and reference the core prompt
  - Confirm platform files contain no behavioral instructions (no XML section tags, no agent logic)
  - Ask the user if questions arise.

## Notes

- The core deliverables are markdown files, not software code — the "implementation" is prompt engineering
- Platform files follow the exact template from existing agents (Perspective, PaperBuddy, etc.)
- The core prompt is the single source of truth for agent behavior; platform files contain only deployment instructions
- Each XML section is created as a separate temporary file first, then merged — this prevents context issues with generating the full prompt in one pass
