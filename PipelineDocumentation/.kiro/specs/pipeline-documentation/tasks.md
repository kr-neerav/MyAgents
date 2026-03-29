# Implementation Plan: Pipeline Documentation Agent

## Overview

Create the Pipeline Documentation Agent — a two-agent LLM workflow (Analyst + Reviewer) that analyzes SQL/PySpark pipeline code and produces structured transformation logic documentation. The deliverables are three markdown prompt files in `PipelineDocumentation/` and a README update. The Analyst Agent is the primary user-facing agent; it orchestrates a review loop by invoking the Reviewer Agent via Kiro's `invokeSubAgent`. The implementation follows the same conventions as existing agents in the MyAgents repository.

## Tasks

- [x] 1. Create the Analyst Agent core prompt
  - [x] 1.1 Create `PipelineDocumentation/pipeline-analyst-agent-prompt.md` with the `<identity>` section
    - Define the Analyst persona: role as a pipeline documentation specialist for SQL and PySpark
    - Communication style: warm, Socratic, domain-adaptive (SQL terminology for SQL code, PySpark terminology for PySpark code)
    - Domain scope: SQL and PySpark pipeline analysis only
    - Off-topic handling: acknowledge warmly and redirect to pipeline analysis
    - Supported input: SQL, PySpark, or mixed (PySpark with embedded `spark.sql()`)
    - Unsupported input handling: inform user only SQL and PySpark are supported, ask for supported code
    - _Requirements: 1.1, 1.2, 1.3, 1.5, 13.1, 13.3, 13.4_

  - [x] 1.2 Add the `<methodology>` section to the Analyst Agent prompt
    - Multi-pass analysis strategy with three passes: (a) structural pass — identify tables, CTEs, aliases, overall data flow; (b) detailed pass — extract transformation logic per step (joins, filters, aggregations, window functions, CASE logic, UDFs, type casts, set operations); (c) validation pass — cross-reference lineage, verify output schema completeness, flag gaps
    - Large pipeline segmentation: for code exceeding 300 lines, segment into logical sections before synthesizing
    - Source table extraction: identify all tables, views, CTEs, temp views, subqueries; resolve aliases; note each distinct usage context; flag ambiguous references
    - Transformation logic extraction: document input columns, operation, output columns for each step; decompose nested/chained transformations; document conditional branches; flag opaque UDFs as external dependencies
    - Business rule identification: label business rules distinctly from technical operations; provide plain-language descriptions; flag hardcoded values as potential configurable parameters; present best interpretation and ask user to confirm when ambiguous
    - Data lineage: map each output column to source columns and transformation path; handle generated/constant values; flag gaps from dynamic SQL, runtime params, or opaque UDFs
    - Output schema: list column name, inferred type, derivation; separate schemas for multiple outputs; mark unknown types
    - Edge cases and assumptions: document assumed non-null columns, assumed unique join keys, assumed ordering, NULL handling strategies, deduplication strategies; flag missing handling for common edge cases
    - Review loop orchestration: after completing Transformation_Documentation, invoke Reviewer Agent via `invokeSubAgent` passing only Pipeline_Code + Transformation_Documentation; parse Reviewer's response for findings and Confidence_Score; make corrections; re-invoke Reviewer until convergence (Confidence_Score ≥ 95% with zero errors/omissions) or 5-round cap
    - Review_Log maintenance: markdown table with columns Round, Role, Feedback Summary, Change Summary, Outcome, Confidence_Score, Score_Rationale; Analyst rows leave Confidence_Score and Score_Rationale blank; entries are summary-level (one sentence or less)
    - Disagreement handling: escalate to user rather than silently dropping findings; flag contradictions between Analyst assumptions and Reviewer findings
    - Convergence failure: stop loop after 5 rounds, present current documentation + Review_Log, inform user
    - Confidence_Score regression: flag if score decreases between consecutive rounds with explanation
    - _Requirements: 1.4, 2.1, 2.2, 2.3, 2.4, 3.1, 3.2, 3.3, 3.4, 3.5, 4.1, 4.2, 4.3, 4.4, 5.1, 5.2, 5.3, 5.4, 6.1, 6.2, 6.3, 7.1, 7.2, 7.3, 7.4, 8.1, 8.2, 8.3, 14.1, 14.4, 14.5, 14.6, 15.1, 15.2, 15.3, 15.5, 15.6, 16.1, 16.2, 16.3, 16.5, 16.6, 17.1, 17.2, 17.3, 17.4, 17.5, 18.1, 18.2, 18.3, 18.4, 18.5, 18.6_

  - [x] 1.3 Add the `<session_protocol>` section to the Analyst Agent prompt
    - Session initialization: greet user, ask for pipeline code
    - Pipeline code ingestion: accept pasted code, confirm receipt, begin analysis
    - Analysis phase progression: use conversational state markers to signal phase transitions, confirm each major section with user before proceeding
    - Session summary generation: produce `[SUMMARY]` block with pipeline name, analysis progress, key findings, review status, open questions, restoration note
    - Session restoration: when user pastes a previous summary, restore context and continue
    - Context window management: proactively offer session summary when conversation grows long
    - Conversational state markers: `[PIPELINE OVERVIEW]`, `[TRANSFORMATION ANALYSIS]`, `[LINEAGE]`, `[DOCUMENTATION]`, `[ANALYST PHASE]`, `[REVIEWER PHASE]`, `[REVIEW ROUND N]`, `[VALIDATION REVIEW]`, `[REVIEW LOG]`, `[SUMMARY]`
    - Iterative refinement: incorporate user corrections, answer questions referencing documentation sections, re-analyze specific sections on request
    - _Requirements: 10.1, 10.2, 10.3, 10.4, 11.1, 11.2, 11.3, 13.2_

  - [x] 1.4 Add the `<interaction_patterns>` section to the Analyst Agent prompt
    - Socratic questioning for ambiguities: ask clarifying questions rather than making silent assumptions
    - Incremental presentation: present analysis section by section, confirm with user before proceeding
    - Vague input handling: ask for clarification when pipeline code or user instructions are unclear
    - Off-scope redirection: acknowledge warmly and redirect to pipeline analysis
    - Iterative refinement: support follow-up questions, corrections, and re-analysis requests
    - Domain-adaptive vocabulary: SQL terminology for SQL code, PySpark terminology for PySpark code
    - _Requirements: 13.1, 13.2, 13.3, 13.4_

  - [x] 1.5 Add the `<formatting>` section to the Analyst Agent prompt
    - Markdown-only output: headers, numbered lists, bullet lists, bold, italic, fenced code blocks, tables — no HTML, LaTeX, or Mermaid
    - Transformation_Documentation template: fixed section order — Pipeline Overview, Source Tables, Transformation Steps (in execution order), Business Rules, Data Lineage, Output Schema, Edge Cases and Assumptions, Open Questions, Review Log
    - Include full templates for each section as defined in the design document (Source Tables table, Transformation Steps format, Business Rules format, Data Lineage table, Output Schema table, Edge Cases format, Review_Log table)
    - Review report format: `[VALIDATION REVIEW]` marker, findings categorized as error/omission/inconsistency/suggestion, Confidence_Score, Score_Rationale
    - Session summary template: `[SUMMARY]` marker with pipeline name, language, analysis progress, key findings, review status, open questions, restoration note
    - Terminal compatibility notes
    - _Requirements: 9.1, 9.2, 9.3, 9.4, 14.3, 14.5, 15.4, 16.4, 17.1, 17.6_

- [x] 2. Checkpoint — Review Analyst Agent prompt
  - Ensure the Analyst Agent prompt file is complete with all five sections (`<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`)
  - Verify all 18 requirements are addressed across the sections
  - Verify all conversational state markers are defined
  - Verify the `invokeSubAgent` call pattern is documented in the methodology
  - Verify the Review_Log table template has exactly 7 columns
  - Verify the Transformation_Documentation template follows the fixed section order
  - Ensure all tests pass, ask the user if questions arise.

- [x] 3. Create the Reviewer Agent core prompt
  - [x] 3.1 Create `PipelineDocumentation/pipeline-reviewer-agent-prompt.md` with the `<identity>` section
    - Define the Reviewer persona: critical, independent reviewer of pipeline transformation documentation
    - Domain scope: SQL and PySpark pipeline documentation review
    - Communication style: precise, evidence-based, references specific code lines and documentation sections
    - Stateless per invocation: no memory of prior review rounds — each invocation is a fresh review
    - Input scope: receives ONLY Pipeline_Code and Transformation_Documentation — no Analyst reasoning, no conversation history
    - _Requirements: 14.2, 15.2_

  - [x] 3.2 Add the `<methodology>` section to the Reviewer Agent prompt
    - Review checklist: missing transformations, incorrect lineage mappings, undocumented source tables, inaccurate business rule descriptions, gaps in edge case coverage, internal inconsistencies
    - Finding categorization: each finding categorized as exactly one of error (factual inaccuracy), omission (missing information), inconsistency (contradictory statements), or suggestion (improvement opportunity)
    - Confidence_Score assignment: 0–100% rating reflecting overall accuracy and completeness of the documentation; criteria for scoring (number of errors, types of omissions, areas of strength)
    - Score_Rationale: one-sentence explanation of key factors contributing to the score
    - Convergence criteria awareness: score ≥ 95% with zero errors/omissions signals convergence
    - _Requirements: 14.2, 14.3, 14.6, 18.1, 18.2, 18.3_

  - [x] 3.3 Add the `<interaction_patterns>` section to the Reviewer Agent prompt
    - How to handle ambiguous findings: document the ambiguity rather than guessing
    - How to structure the review report: findings grouped by category, then Confidence_Score and Score_Rationale
    - Evidence-based findings: each finding references specific code lines or documentation sections
    - _Requirements: 14.2, 14.3_

  - [x] 3.4 Add the `<formatting>` section to the Reviewer Agent prompt
    - Review report template: `[VALIDATION REVIEW]` marker, sections for Errors, Omissions, Inconsistencies, Suggestions, then Confidence_Score and Score_Rationale
    - Markdown-only output
    - _Requirements: 14.3, 14.5, 9.2_

- [x] 4. Checkpoint — Review Reviewer Agent prompt
  - Ensure the Reviewer Agent prompt file is complete with all four sections (`<identity>`, `<methodology>`, `<interaction_patterns>`, `<formatting>`)
  - Verify the Reviewer prompt does NOT contain `<session_protocol>` (stateless agent)
  - Verify finding categorization uses exactly the four categories: error, omission, inconsistency, suggestion
  - Verify Confidence_Score criteria and Score_Rationale format are defined
  - Verify the review report template matches the design document format
  - Ensure all tests pass, ask the user if questions arise.

- [x] 5. Create the Kiro platform wrapper
  - [x] 5.1 Create `PipelineDocumentation/platform-kiro.md` with setup instructions for both agents
    - Setup instructions for deploying the Analyst Agent as a Kiro steering file at `.kiro/steering/pipeline-analyst-agent.md`
    - Setup instructions for deploying the Reviewer Agent as a Kiro steering file (used by `invokeSubAgent`, not directly by user)
    - `invokeSubAgent` configuration: how the Analyst calls the Reviewer programmatically, passing Pipeline_Code + Transformation_Documentation via `prompt` parameter and Reviewer prompt file via `contextFiles`
    - Platform Notes section: terminal-based output considerations, context window notes, session persistence
    - Limitations section: context window limits, session independence, scrollback
    - "What This Wrapper Does NOT Do" section: does not modify core prompt behavior for either agent
    - Follow the same structure and conventions as existing platform wrappers (e.g., `Design/platform-kiro.md`)
    - _Requirements: 12.1, 12.2, 12.3, 12.4, 15.5_

- [x] 6. Update the README
  - [x] 6.1 Add the Pipeline Documentation Agent entry to `README.md`
    - Add a new section following the same format as existing agent entries
    - Include links to `pipeline-analyst-agent-prompt.md`, `pipeline-reviewer-agent-prompt.md`, and `platform-kiro.md`
    - Brief description: two-agent workflow for analyzing SQL/PySpark pipeline code and producing structured transformation documentation with automated review loop
    - _Requirements: 12.1, 12.2_

- [x] 7. Final checkpoint — Verify all deliverables
  - Verify `PipelineDocumentation/pipeline-analyst-agent-prompt.md` exists and contains all five sections
  - Verify `PipelineDocumentation/pipeline-reviewer-agent-prompt.md` exists and contains all four sections
  - Verify `PipelineDocumentation/platform-kiro.md` exists and contains Setup (both agents + invokeSubAgent config), Platform Notes, Limitations, and "What This Wrapper Does NOT Do"
  - Verify `README.md` contains the new Pipeline Documentation Agent entry
  - Verify all 18 requirements are covered across the deliverables
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- All deliverables are markdown prompt files — no executable code
- The Analyst Agent prompt (task 1) is the largest deliverable, covering analysis methodology, review loop orchestration, and all output templates
- The Reviewer Agent prompt (task 3) is narrower in scope — review methodology, finding categorization, and Confidence_Score only
- The platform wrapper (task 5) covers both agents and the `invokeSubAgent` configuration
- Checkpoints at tasks 2, 4, and 7 ensure incremental validation
- Each task references specific requirements for traceability
