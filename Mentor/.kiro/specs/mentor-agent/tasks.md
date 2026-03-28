# Tasks — Mentor Agent

## Task 1: Create Core Prompt — Identity Section
- [x] 1.1 Write the `<identity>` section of `Mentor/mentor-agent-prompt.md` including: principal engineer persona definition, role statement (mentoring companion), communication style (direct, honest, supportive), data engineering domain awareness listing all specified domains, and off-topic handling rules with engineering-adjacent distinction
- [x] 1.2 Verify identity section references all six growth dimensions by name
- [x] 1.3 Verify identity section lists all data engineering domains: data pipelines, data modeling, data quality, batch and streaming architectures, data platform ownership, analytics infrastructure

## Task 2: Create Core Prompt — Methodology Section
- [x] 2.1 Write the `<methodology>` section including: Situation Processing Flow (clarify, reframe, explore, identify dimensions, surface takeaways), Reframe Technique definition, Growth Dimension Framework with all six dimensions described, Frameworks and Mental Models catalog (reversibility, blast radius, stakeholder mapping, RACI, tech debt quadrant), and war story usage guidelines
- [x] 2.2 Verify the situation processing flow defines all five steps in order
- [x] 2.3 Verify all six growth dimensions are defined with descriptions

## Task 3: Create Core Prompt — Session Protocol Section
- [x] 3.1 Write the `<session_protocol>` section including: session initialization sequence (warm greeting, situation prompt), situation-driven conversation flow referencing methodology, growth dimension tracking across session, session summary generation with all four elements (situations, dimensions, insights, open threads), and context window management with proactive summary offer
- [x] 3.2 Verify session summary format includes all four required elements

## Task 4: Create Core Prompt — Interaction Patterns Section
- [x] 4.1 Write the `<interaction_patterns>` section including: Socratic questioning for decisions, post-decision exploration (explore reasoning and blind spots), direct answer handling (provide with reasoning then return to mentoring mode), framework offering when user is stuck, and off-topic handling with engineering-adjacent distinction
- [x] 4.2 Verify direct answer handling pattern is defined

## Task 5: Create Core Prompt — Formatting Section
- [x] 5.1 Write the `<formatting>` section including: all five conversational state markers (`[SITUATION]`, `[REFRAME]`, `[GROWTH DIMENSIONS]`, `[TAKEAWAYS]`, `[SUMMARY]`), standard markdown rules (no HTML/LaTeX/Mermaid), and terminal compatibility guidelines (reasonable line lengths, no wide tables, clear section separation)
- [x] 5.2 Verify all five conversational state markers are defined
- [x] 5.3 Verify the file uses standard markdown only — no HTML tags, LaTeX, or Mermaid

## Task 6: Create Platform Guide — Kiro CLI
- [x] 6.1 Write `Mentor/platform-kiro.md` following the structure of `Learning/platform-kiro.md`: title referencing core prompt, setup instructions for deploying as steering file at `.kiro/steering/mentor-agent.md`, platform notes for terminal output, limitations section covering context window management, session persistence, and session summary usage, and "What This Wrapper Does NOT Do" disclaimer
- [x] 6.2 Verify the guide references the correct steering file path `.kiro/steering/mentor-agent.md`
- [x] 6.3 Verify limitations section covers context window, session persistence, and session summaries

## Task 7: Create Platform Guide — Claude
- [x] 7.1 Write `Mentor/platform-claude.md` following the structure of `Learning/platform-claude.md`: title referencing core prompt, setup instructions for both Projects approach and direct system message approach, platform notes on instruction-following and formatting, limitations section covering context window management, session persistence, and session summary usage, and "What This Wrapper Does NOT Do" disclaimer
- [x] 7.2 Verify the guide describes both Projects and direct system message deployment approaches
- [x] 7.3 Verify limitations section covers context window, session persistence, and session summaries

## Task 8: Create Platform Guide — Gemini
- [x] 8.1 Write `Mentor/platform-gemini.md` following the structure of `Learning/platform-gemini.md`: title referencing core prompt, setup instructions for both Custom Instructions approach and Gems approach, platform notes on formatting support, limitations section covering context window management, session persistence, and session summary usage, and "What This Wrapper Does NOT Do" disclaimer
- [x] 8.2 Verify the guide describes both Custom Instructions and Gems deployment approaches
- [x] 8.3 Verify limitations section covers context window, session persistence, and session summaries
