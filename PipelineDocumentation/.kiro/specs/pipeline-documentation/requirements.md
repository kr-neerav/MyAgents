# Requirements Document

## Introduction

This document defines the requirements for a Pipeline Documentation Agent — an LLM agent (or multi-agent workflow) that analyzes data pipeline code written in SQL and PySpark and produces structured transformation logic documentation. The documentation captures enough detail about source tables, transformations, business logic, data flow, output schemas, edge cases, and assumptions to enable a senior data engineer to recode the pipeline in a different framework or platform. The agent assumes a large token budget and prioritizes accuracy over efficiency. The agent includes a role-based validation phase where it adopts a separate Reviewer persona to independently verify the documentation produced during analysis, with strict context isolation between roles to prevent self-confirming bias. The validation phase supports iterative multi-round exchanges between the Analyst and Reviewer roles until the Reviewer reaches confidence in documentation accuracy, with a configurable round cap to prevent infinite loops. Both roles update a shared Review_Log table that provides the user with a summary-level audit trail of feedback given, changes made, and outcomes for each round. The agent follows the same conventions as other agents in the MyAgents repository: a core prompt file with a platform wrapper for Kiro.

## Glossary

- **Pipeline_Code**: SQL or PySpark source code that defines a data pipeline, potentially hundreds or thousands of lines long.
- **Transformation_Logic**: The set of operations applied to data as it moves through a pipeline — filters, joins, aggregations, derivations, type casts, window functions, conditional logic, and any other data manipulation.
- **Documentation_Agent**: The LLM agent (or orchestrating agent in a multi-agent workflow) responsible for analyzing Pipeline_Code and producing Transformation_Documentation.
- **Transformation_Documentation**: The structured output document that captures all transformation logic, source tables, output schemas, business rules, edge cases, and assumptions extracted from Pipeline_Code.
- **Source_Table**: Any table, view, CTE, subquery, or external data source read by the Pipeline_Code.
- **Output_Schema**: The structure (column names, types, derivation logic) of the final output produced by the Pipeline_Code.
- **Business_Rule**: Domain-specific logic embedded in the Pipeline_Code — conditional transformations, filtering criteria, default values, fallback logic, or derived calculations that reflect business decisions rather than purely technical operations.
- **Data_Lineage**: The tracing of each output column back through the chain of transformations to its original Source_Table column(s).
- **Core_Prompt**: The main markdown file containing the agent's identity, methodology, session protocol, interaction patterns, and formatting rules.
- **Platform_Wrapper**: A platform-specific deployment guide (Gemini, Kiro CLI, Claude) that provides setup instructions without modifying the Core_Prompt behavior.
- **Multi_Pass_Analysis**: An analysis strategy where the agent makes multiple passes over the Pipeline_Code — first for structure, then for detailed transformation logic, then for cross-referencing and validation — to maximize accuracy.
- **Analyst_Role**: The persona adopted by the Documentation_Agent during the analysis phase — responsible for ingesting Pipeline_Code, extracting transformation logic, and producing the initial Transformation_Documentation.
- **Reviewer_Role**: A distinct persona adopted by the Documentation_Agent during the validation phase — responsible for critically reviewing the Transformation_Documentation produced by the Analyst_Role, checking for accuracy, completeness, and consistency against the original Pipeline_Code.
- **Role_Context**: The working state (findings, intermediate notes, assumptions, and reasoning) accumulated by a specific role during its execution phase.
- **Context_Boundary**: The separation mechanism that prevents Role_Context from one role from leaking into or influencing the execution of a different role, ensuring independent analysis and review.
- **Review_Round**: A single iteration of the Analyst–Reviewer feedback loop — the Reviewer reviews the current Transformation_Documentation, produces findings, and the Analyst incorporates corrections. Each Review_Round is numbered sequentially starting from 1.
- **Convergence**: The state reached when the Reviewer_Role assigns a Confidence_Score of 95% or higher and determines that the Transformation_Documentation contains no remaining errors or omissions — ending the iterative review loop.
- **Review_Log**: A shared markdown table maintained across Review_Rounds that records a summary-level audit trail of each round's feedback, changes, and outcome, enabling the user to understand the validation history without reading detailed review reports.
- **Confidence_Score**: A numeric accuracy/confidence rating (0–100%) assigned by the Reviewer_Role at the end of each Review_Round, reflecting the Reviewer's assessment of the Transformation_Documentation's overall correctness and completeness. The Confidence_Score is recorded in the Review_Log alongside a brief rationale explaining the factors that contributed to the score.

## Requirements

### Requirement 1: Pipeline Code Ingestion

**User Story:** As a senior data engineer, I want to provide my SQL or PySpark pipeline code to the agent, so that the agent can analyze it and produce transformation documentation.

#### Acceptance Criteria

1. WHEN the user provides SQL pipeline code, THE Documentation_Agent SHALL accept the code and begin analysis.
2. WHEN the user provides PySpark pipeline code, THE Documentation_Agent SHALL accept the code and begin analysis.
3. WHEN the user provides pipeline code that mixes SQL and PySpark (e.g., PySpark with embedded SQL expressions via `spark.sql()`), THE Documentation_Agent SHALL analyze both the PySpark and embedded SQL components.
4. WHEN the user provides pipeline code exceeding 500 lines, THE Documentation_Agent SHALL process the full code without truncation or omission.
5. IF the user provides code in a language other than SQL or PySpark, THEN THE Documentation_Agent SHALL inform the user that only SQL and PySpark are supported and ask for supported code.

### Requirement 2: Source Table Extraction

**User Story:** As a senior data engineer, I want the agent to identify every source table, view, CTE, and external data source referenced in the pipeline, so that I know exactly what data the pipeline depends on.

#### Acceptance Criteria

1. WHEN analyzing Pipeline_Code, THE Documentation_Agent SHALL identify and list every Source_Table referenced in the code, including tables, views, CTEs, temporary views, and subqueries used as data sources.
2. WHEN a Source_Table is referenced multiple times in the Pipeline_Code, THE Documentation_Agent SHALL note each distinct usage context (e.g., used in join on line X, used in filter on line Y).
3. WHEN the Pipeline_Code references a table through an alias, THE Documentation_Agent SHALL resolve the alias to the original Source_Table name and document both.
4. IF a Source_Table reference is ambiguous or cannot be resolved from the code alone, THEN THE Documentation_Agent SHALL flag the ambiguity and ask the user for clarification.

### Requirement 3: Transformation Logic Extraction

**User Story:** As a senior data engineer, I want the agent to extract and document every transformation applied to the data, so that I can understand exactly what the pipeline does at each step.

#### Acceptance Criteria

1. WHEN analyzing Pipeline_Code, THE Documentation_Agent SHALL identify and document every transformation operation including: filters (WHERE/HAVING/filter), joins (type, keys, conditions), aggregations (GROUP BY, agg functions), window functions, column derivations, type casts, CASE/WHEN logic, UDFs, and set operations (UNION, INTERSECT, EXCEPT).
2. FOR EACH transformation, THE Documentation_Agent SHALL document the input columns, the operation performed, and the resulting output columns.
3. WHEN the Pipeline_Code contains nested or chained transformations, THE Documentation_Agent SHALL decompose the chain into individual steps and document each step in execution order.
4. WHEN the Pipeline_Code contains conditional logic (CASE statements, if/else in PySpark, COALESCE, NULLIF), THE Documentation_Agent SHALL document each branch and its triggering condition.
5. IF a transformation references a UDF or external function whose logic is not visible in the provided code, THEN THE Documentation_Agent SHALL flag the UDF as an external dependency and document its name, parameters, and return type as visible from the call site.

### Requirement 4: Business Rule Identification

**User Story:** As a senior data engineer, I want the agent to distinguish business rules from purely technical operations, so that I can understand the domain logic embedded in the pipeline.

#### Acceptance Criteria

1. WHEN analyzing Pipeline_Code, THE Documentation_Agent SHALL identify transformations that encode Business_Rules and label them distinctly from purely technical operations (e.g., type casts, repartitioning).
2. FOR EACH identified Business_Rule, THE Documentation_Agent SHALL provide a plain-language description of what the rule does and why it likely exists based on the code context.
3. WHEN a Business_Rule involves hardcoded values (magic numbers, string literals, date thresholds), THE Documentation_Agent SHALL flag each hardcoded value and note that it may represent a configurable business parameter.
4. IF the intent of a Business_Rule is ambiguous from the code alone, THEN THE Documentation_Agent SHALL present its best interpretation and ask the user to confirm or correct.

### Requirement 5: Data Lineage Documentation

**User Story:** As a senior data engineer, I want the agent to trace each output column back to its source, so that I can understand the full derivation path of every field in the output.

#### Acceptance Criteria

1. THE Documentation_Agent SHALL produce a Data_Lineage section that maps each column in the Output_Schema back to its originating Source_Table column(s) and the transformations applied along the path.
2. WHEN an output column is derived from multiple source columns (e.g., a concatenation, a calculation, a join key), THE Documentation_Agent SHALL list all contributing source columns.
3. WHEN an output column is a constant or generated value (e.g., `current_timestamp()`, a literal, a row number), THE Documentation_Agent SHALL document the generation logic rather than a source column.
4. IF the lineage of an output column cannot be fully traced due to dynamic SQL, runtime parameters, or opaque UDFs, THEN THE Documentation_Agent SHALL document the traceable portion and flag the gap.

### Requirement 6: Output Schema Documentation

**User Story:** As a senior data engineer, I want the agent to document the final output schema of the pipeline, so that I know exactly what the pipeline produces.

#### Acceptance Criteria

1. THE Documentation_Agent SHALL produce an Output_Schema section listing every column in the pipeline's final output, including column name, inferred data type, and a description of how the column is derived.
2. WHEN the Pipeline_Code produces multiple outputs (e.g., writes to multiple tables, returns multiple DataFrames), THE Documentation_Agent SHALL document each output separately with its own schema.
3. WHEN data types can be inferred from the code (explicit casts, function return types, source column types if stated), THE Documentation_Agent SHALL include the inferred type. WHERE the type cannot be inferred, THE Documentation_Agent SHALL note the type as unknown.

### Requirement 7: Edge Cases and Assumptions Documentation

**User Story:** As a senior data engineer, I want the agent to surface edge cases, implicit assumptions, and potential data quality issues in the pipeline, so that I can handle them when recoding.

#### Acceptance Criteria

1. WHEN analyzing Pipeline_Code, THE Documentation_Agent SHALL identify and document implicit assumptions including: assumed non-null columns, assumed uniqueness of join keys, assumed ordering, assumed data types, and assumed referential integrity between tables.
2. WHEN the Pipeline_Code contains NULL handling logic (COALESCE, IFNULL, IS NOT NULL filters, NVL), THE Documentation_Agent SHALL document the NULL handling strategy for each affected column.
3. WHEN the Pipeline_Code contains deduplication logic (DISTINCT, ROW_NUMBER partitioning, GROUP BY without aggregation), THE Documentation_Agent SHALL document the deduplication strategy and the columns used to determine uniqueness.
4. IF the Pipeline_Code lacks explicit handling for a common edge case (e.g., empty input, NULL join keys, duplicate records, division by zero), THEN THE Documentation_Agent SHALL flag the missing handling as a potential risk.

### Requirement 8: Multi-Pass Analysis Strategy

**User Story:** As a senior data engineer, I want the agent to use a multi-pass analysis approach, so that the documentation is thorough and cross-referenced rather than a single-pass summary.

#### Acceptance Criteria

1. THE Documentation_Agent SHALL use a Multi_Pass_Analysis strategy with at least three passes: (a) structural pass to identify tables, CTEs, and overall data flow; (b) detailed pass to extract transformation logic for each step; (c) validation pass to cross-reference lineage, check for gaps, and verify consistency.
2. WHEN the validation pass identifies inconsistencies (e.g., a column referenced in lineage but not in the output schema, a source table listed but not used in any transformation), THE Documentation_Agent SHALL flag the inconsistency and attempt to resolve it.
3. WHEN the Pipeline_Code is large (exceeding 300 lines), THE Documentation_Agent SHALL segment the code into logical sections (CTEs, subqueries, pipeline stages) and analyze each section before synthesizing the full documentation.

### Requirement 9: Structured Documentation Output

**User Story:** As a senior data engineer, I want the documentation to follow a consistent, structured format, so that I can navigate it efficiently and use it as a reference when recoding.

#### Acceptance Criteria

1. THE Documentation_Agent SHALL produce Transformation_Documentation with the following sections in order: Pipeline Overview, Source Tables, Transformation Steps (in execution order), Business Rules, Data Lineage, Output Schema, Edge Cases and Assumptions, and Open Questions.
2. THE Documentation_Agent SHALL use standard markdown formatting — headers, numbered lists, bullet lists, bold, italic, fenced code blocks, and tables. THE Documentation_Agent SHALL NOT use HTML, LaTeX, or Mermaid diagrams.
3. WHEN presenting transformation steps, THE Documentation_Agent SHALL order them in the logical execution sequence of the pipeline, not in the order they appear in the source code (when these differ).
4. THE Documentation_Agent SHALL include an Open Questions section listing any ambiguities, unresolved references, or areas where user clarification is needed.

### Requirement 10: Iterative Refinement

**User Story:** As a senior data engineer, I want to be able to ask the agent follow-up questions and request corrections, so that the documentation converges on an accurate representation of the pipeline.

#### Acceptance Criteria

1. WHEN the user provides corrections or additional context about the pipeline, THE Documentation_Agent SHALL incorporate the feedback and update the relevant sections of the Transformation_Documentation.
2. WHEN the user asks a question about a specific transformation or business rule, THE Documentation_Agent SHALL reference the relevant section of the documentation and provide a detailed explanation.
3. WHEN the user requests the agent to re-analyze a specific section of the Pipeline_Code, THE Documentation_Agent SHALL re-analyze that section and update the documentation accordingly.
4. THE Documentation_Agent SHALL maintain a conversational state marker system consistent with the MyAgents repository conventions, using markers such as `[PIPELINE OVERVIEW]`, `[TRANSFORMATION ANALYSIS]`, `[LINEAGE]`, `[DOCUMENTATION]`, and `[SUMMARY]`.

### Requirement 11: Session Management

**User Story:** As a senior data engineer, I want the agent to support session summaries and context restoration, so that I can continue analysis across multiple sessions for large pipelines.

#### Acceptance Criteria

1. WHEN the user requests a session summary, THE Documentation_Agent SHALL produce a `[SUMMARY]` block that includes: pipeline name/description, analysis progress (sections completed, sections pending), key findings so far, open questions, and a restoration note.
2. WHEN the user pastes a previous session summary into a new session, THE Documentation_Agent SHALL restore context and continue analysis from where the previous session ended.
3. WHEN the conversation grows long, THE Documentation_Agent SHALL proactively offer to generate a session summary to preserve context.

### Requirement 12: Core Prompt and Platform Wrapper Structure

**User Story:** As the repository maintainer, I want the agent to follow the same file structure conventions as other agents in the MyAgents repository, so that the repository remains consistent.

#### Acceptance Criteria

1. THE agent SHALL have two Core_Prompt files in the `PipelineDocumentation/` directory: `pipeline-analyst-agent-prompt.md` (Analyst Agent) and `pipeline-reviewer-agent-prompt.md` (Reviewer Agent), each containing the full agent identity, methodology, and formatting rules for its respective role.
2. THE agent SHALL have one Platform_Wrapper file: `platform-kiro.md` in the `PipelineDocumentation/` directory.
3. THE Platform_Wrapper SHALL provide deployment instructions specific to Kiro without modifying the Core_Prompt behavior.
4. THE Platform_Wrapper SHALL follow the same structure and conventions as existing Platform_Wrappers in the repository (setup options, platform notes, limitations, and a "What This Wrapper Does NOT Do" section), adapted for Kiro's `invokeSubAgent` capabilities.

### Requirement 13: Agent Interaction Pattern

**User Story:** As a senior data engineer, I want the agent to use a guided, Socratic approach consistent with the other agents in the repository, so that the interaction feels familiar and productive.

#### Acceptance Criteria

1. THE Documentation_Agent SHALL use Socratic questioning to clarify ambiguities in the Pipeline_Code rather than making silent assumptions.
2. THE Documentation_Agent SHALL present its analysis incrementally using conversational state markers, confirming each major section with the user before proceeding.
3. WHEN the user asks a question outside the scope of pipeline documentation, THE Documentation_Agent SHALL acknowledge the question warmly and redirect to pipeline analysis.
4. THE Documentation_Agent SHALL adapt its technical vocabulary to match the user's domain — using SQL terminology when analyzing SQL code and PySpark terminology when analyzing PySpark code.

### Requirement 14: Role-Based Output Validation

**User Story:** As a senior data engineer, I want the agent to validate its own documentation output by adopting a separate Reviewer role, so that the final documentation is independently verified for accuracy and completeness rather than self-confirmed by the same analytical perspective that produced it.

#### Acceptance Criteria

1. WHEN the Documentation_Agent completes the Transformation_Documentation in the Analyst_Role, THE Documentation_Agent SHALL transition to the Reviewer_Role before presenting the documentation as final.
2. WHILE operating in the Reviewer_Role, THE Documentation_Agent SHALL critically review the Transformation_Documentation against the original Pipeline_Code, checking for: missing transformations, incorrect lineage mappings, undocumented source tables, inaccurate business rule descriptions, and gaps in edge case coverage.
3. WHILE operating in the Reviewer_Role, THE Documentation_Agent SHALL produce a structured review report listing each finding as one of: error (factual inaccuracy), omission (missing information), inconsistency (contradictory statements within the documentation), or suggestion (improvement opportunity).
4. WHEN the Reviewer_Role identifies errors or omissions, THE Documentation_Agent SHALL automatically return to the Analyst_Role to correct the Transformation_Documentation and re-invoke the Reviewer via `invokeSubAgent`, without requiring manual user intervention for the handoff.
5. THE Documentation_Agent SHALL present the review findings to the user alongside the final Transformation_Documentation, using a `[VALIDATION REVIEW]` conversational state marker.
6. WHEN the Reviewer_Role finds no errors, omissions, or inconsistencies, THE Documentation_Agent SHALL note that the documentation passed validation and present the final output.

### Requirement 15: Context Isolation Between Roles

**User Story:** As a senior data engineer, I want the analysis and review roles to operate with isolated context, so that the validation is genuinely independent and does not inherit assumptions or reasoning from the analysis phase.

#### Acceptance Criteria

1. WHEN transitioning from the Analyst_Role to the Reviewer_Role, THE Documentation_Agent SHALL establish a Context_Boundary that excludes the Analyst_Role's intermediate reasoning, working notes, and assumption rationale from the Reviewer_Role's input.
2. THE Reviewer_Role SHALL receive only the original Pipeline_Code and the final Transformation_Documentation as inputs — the Reviewer_Role SHALL NOT receive the Analyst_Role's step-by-step reasoning, draft notes, or internal commentary.
3. WHEN the Reviewer_Role produces findings, THE Documentation_Agent SHALL NOT allow the Analyst_Role's prior reasoning to override or dismiss the Reviewer_Role's findings without re-examining the Pipeline_Code.
4. THE Documentation_Agent SHALL log each role transition using conversational state markers: `[ANALYST PHASE]` when operating in the Analyst_Role and `[REVIEWER PHASE]` when operating in the Reviewer_Role.
5. THE Analyst Agent SHALL use Kiro's `invokeSubAgent` mechanism to call the Reviewer Agent as a subagent, passing only the Pipeline_Code and the Transformation_Documentation via the `prompt` and `contextFiles` parameters. The `invokeSubAgent` mechanism ensures the Reviewer has no access to the Analyst's conversation history, enforcing context isolation by architecture.
6. WHEN the Reviewer_Role identifies a finding that contradicts an assumption made by the Analyst_Role, THE Documentation_Agent SHALL flag the contradiction to the user for resolution rather than silently resolving it in favor of either role.


### Requirement 16: Iterative Multi-Round Validation

**User Story:** As a senior data engineer, I want the Analyst and Reviewer roles to go back and forth multiple times until the Reviewer is confident the documentation is accurate, so that the validation is thorough rather than a single-pass rubber stamp.

#### Acceptance Criteria

1. WHEN the Reviewer_Role identifies errors or omissions in a Review_Round, THE Documentation_Agent SHALL return to the Analyst_Role to correct the Transformation_Documentation and then transition back to the Reviewer_Role for re-review, continuing this loop until Convergence is reached.
2. THE Documentation_Agent SHALL support a maximum of 5 Review_Rounds per validation cycle. IF Convergence is not reached within 5 rounds, THEN THE Documentation_Agent SHALL stop the loop, present the current Transformation_Documentation and Review_Log to the user, and inform the user that Convergence was not reached within the round cap.
3. WHEN the Reviewer_Role assigns a Confidence_Score of 95% or higher at the end of a Review_Round and identifies no remaining errors or omissions, THE Documentation_Agent SHALL declare Convergence and end the review loop.
4. FOR EACH Review_Round, THE Documentation_Agent SHALL log the round number using a conversational state marker: `[REVIEW ROUND N]` where N is the current round number.
5. WHEN Convergence is reached, THE Documentation_Agent SHALL note the total number of Review_Rounds completed, the final Confidence_Score, and present the final Transformation_Documentation to the user.
6. IF the Analyst_Role disagrees with a Reviewer_Role finding during a Review_Round, THE Documentation_Agent SHALL flag the disagreement to the user for resolution rather than silently dropping the finding.

### Requirement 17: Shared Review Log Table

**User Story:** As a senior data engineer, I want both the Analyst and Reviewer roles to update a common log table that summarizes each round of feedback and changes, so that I can understand the validation trail without reading detailed review reports.

#### Acceptance Criteria

1. THE Documentation_Agent SHALL maintain a Review_Log formatted as a markdown table with the following columns: Round (the Review_Round number), Role (the role that acted — Analyst or Reviewer), Feedback Summary (a one-sentence summary of the feedback given), Change Summary (a one-sentence summary of what was changed in response), Outcome (one of: "corrected", "no change needed", "partially addressed", or "escalated to user"), Confidence_Score (the Reviewer's 0–100% accuracy rating for the current state of the Transformation_Documentation), and Score_Rationale (a one-sentence explanation of the factors that contributed to the Confidence_Score).
2. WHEN the Reviewer_Role completes a review in a Review_Round, THE Documentation_Agent SHALL append a row to the Review_Log summarizing the Reviewer's findings, including the Confidence_Score and Score_Rationale for that round.
3. WHEN the Analyst_Role completes corrections in a Review_Round, THE Documentation_Agent SHALL append a row to the Review_Log summarizing the changes made. THE Analyst_Role rows SHALL leave the Confidence_Score and Score_Rationale columns blank, as only the Reviewer_Role assigns scores.
4. THE Review_Log entries SHALL be summary-level — each Feedback Summary and Change Summary SHALL be one sentence or less, not a detailed enumeration of every finding or edit.
5. WHEN the validation cycle ends (either by Convergence or by reaching the round cap), THE Documentation_Agent SHALL present the complete Review_Log to the user alongside the final Transformation_Documentation, using a `[REVIEW LOG]` conversational state marker.
6. THE Review_Log SHALL be included as a section in the final Transformation_Documentation output, placed after the Open Questions section and before any session summary.


### Requirement 18: Confidence Score and Rationale Per Review Round

**User Story:** As a senior data engineer, I want the Reviewer to assign a confidence/accuracy score with a rationale at the end of each review round, so that I can track how documentation quality improves across iterations and understand what drove the Reviewer's assessment.

#### Acceptance Criteria

1. WHEN the Reviewer_Role completes a review in a Review_Round, THE Reviewer_Role SHALL assign a Confidence_Score between 0% and 100% representing the Reviewer's assessment of the Transformation_Documentation's overall accuracy and completeness.
2. FOR EACH Confidence_Score assigned, THE Reviewer_Role SHALL provide a Score_Rationale of one sentence summarizing the key factors that contributed to the score (e.g., number of errors found, types of omissions, areas of strength).
3. THE Confidence_Score SHALL reflect the cumulative state of the Transformation_Documentation at the end of the current Review_Round, accounting for corrections made in prior rounds.
4. WHEN the Confidence_Score reaches 95% or higher and the Reviewer_Role identifies no remaining errors or omissions, THE Documentation_Agent SHALL declare Convergence and end the review loop, as defined in Requirement 16.
5. IF the Confidence_Score decreases between consecutive Review_Rounds, THEN THE Documentation_Agent SHALL flag the regression to the user and include an explanation in the Score_Rationale.
6. THE Confidence_Score and Score_Rationale SHALL be recorded in the Review_Log table as defined in Requirement 17, in the Confidence_Score and Score_Rationale columns of the Reviewer_Role's row for that round.
