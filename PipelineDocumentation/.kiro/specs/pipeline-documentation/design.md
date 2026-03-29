# Design Document: Pipeline Documentation Agent

## Overview

This design describes a Pipeline Documentation system — a two-agent LLM workflow that analyzes SQL and PySpark data pipeline code and produces structured transformation logic documentation. The system follows the same conventions as other agents in the MyAgents repository, with prompt files and platform wrappers living in the `PipelineDocumentation/` directory.

The system's distinguishing feature is a two-agent validation workflow. The Analyst Agent (`pipeline-analyst-agent-prompt.md`) is the primary agent the user interacts with — it handles pipeline code ingestion, multi-pass analysis, and documentation generation. After producing the Transformation_Documentation, the Analyst Agent orchestrates a review loop by programmatically invoking the Reviewer Agent (`pipeline-reviewer-agent-prompt.md`) via Kiro's `invokeSubAgent` mechanism, passing only the Pipeline_Code and the Transformation_Documentation. The Reviewer Agent performs an independent review, assigns a Confidence_Score, and returns findings. The Analyst processes the findings, makes corrections, and invokes the Reviewer again until convergence (up to 5 rounds). A Review_Log table maintained by the Analyst provides the user with a summary-level audit trail across rounds.

The invocation mechanism is automated — the Analyst Agent uses `invokeSubAgent` to call the Reviewer Agent as a subagent, passing Pipeline_Code and Transformation_Documentation via the `prompt` and `contextFiles` parameters. The Reviewer runs as a separate agent execution with its own context, and the Analyst parses the Reviewer's response to continue the loop. No manual user intervention is needed for the handoff.

Key design priorities:
- **Accuracy over efficiency** — large token budget assumed; thoroughness is paramount
- **Context isolation by architecture** — the Reviewer is a separate agent invoked via `invokeSubAgent`, so it physically cannot see the Analyst's intermediate reasoning. This is a stronger guarantee than prompt-based persona switching, and the handoff is fully automated.
- **Repository consistency** — follows the same identity/methodology/session_protocol/interaction_patterns/formatting structure as Design Agent, Learning Agent, etc.
- **Structured output** — documentation follows a fixed section order with standard markdown formatting

## Architecture

The system uses a two-agent architecture with the Analyst Agent as the orchestrator and the Reviewer Agent as a stateless validation service. Each agent is a separate prompt file. The Analyst manages the review loop, programmatically invokes the Reviewer via Kiro's `invokeSubAgent` mechanism, and processes the Reviewer's returned findings. Context isolation is enforced by architecture — the Reviewer runs as a separate agent execution with its own context and no access to the Analyst's conversation history. The entire review loop is automated with no manual user intervention required.

```
┌─────────────────────────────────────────────────────────┐
│              Analyst Agent                                │
│  pipeline-analyst-agent-prompt.md                         │
│                                                           │
│  ┌─────────────────────────────────────────────────┐     │
│  │ - Pipeline code ingestion                        │     │
│  │ - Multi-pass analysis (structural/detailed/val)  │     │
│  │ - Documentation generation                       │     │
│  │ - Review loop orchestration                      │     │
│  │ - Review_Log maintenance                         │     │
│  │ - Correction processing                          │     │
│  └─────────────────────────────────────────────────┘     │
│         │                                                 │
│         │ invokeSubAgent({                                │
│         │   name: "Reviewer",                             │
│         │   prompt: Pipeline_Code +                       │
│         │           Transformation_Documentation,         │
│         │   contextFiles: [reviewer prompt file]          │
│         │ })                                              │
│         ▼                                                 │
│  ┌─────────────────────────────────────────────────┐     │
│  │ Analyst parses Reviewer response, makes          │     │
│  │ corrections, re-invokes if needed                │     │
│  └─────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────┘
         │                          ▲
         │ (invokeSubAgent)         │ (subagent response)
         ▼                          │
┌─────────────────────────────────────────────────────────┐
│              Reviewer Agent                               │
│  pipeline-reviewer-agent-prompt.md                        │
│                                                           │
│  ┌─────────────────────────────────────────────────┐     │
│  │ - Receives ONLY Pipeline_Code + Documentation    │     │
│  │ - Independent review against code                │     │
│  │ - Finding categorization                         │     │
│  │ - Confidence_Score assignment                    │     │
│  │ - Stateless per invocation (no memory of prior   │     │
│  │   rounds — true context isolation by design)     │     │
│  └─────────────────────────────────────────────────┘     │
└─────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────┐
│              Platform Wrapper                              │
│  platform-kiro.md                                         │
│  (deployment instructions for BOTH agents as Kiro          │
│   steering files, with invokeSubAgent configuration)      │
└─────────────────────────────────────────────────────────┘
```

### Design Decisions

1. **Two-agent architecture vs. single-prompt persona switching**: The previous design used a single prompt with persona switching (`[ANALYST PHASE]` / `[REVIEWER PHASE]` markers) to implement both roles within one conversation. The updated design uses two separate agent prompts — the Analyst Agent and the Reviewer Agent. The Analyst programmatically invokes the Reviewer via Kiro's `invokeSubAgent` mechanism, passing only the Pipeline_Code and Transformation_Documentation. The Reviewer runs as a separate agent execution with its own context — providing context isolation by architecture AND full automation. The Analyst parses the Reviewer's response, makes corrections, and re-invokes the Reviewer — all without user intervention. This gives us both the strongest isolation guarantee (separate agent execution) and an automated review loop (no manual copy/paste handoff).

2. **Analyst as orchestrator**: The Analyst Agent owns the review loop — it decides when to invoke the Reviewer via `invokeSubAgent`, processes the Reviewer's findings from the subagent response, makes corrections, maintains the Review_Log, and decides when to invoke the Reviewer again. The Reviewer Agent is stateless per invocation — each `invokeSubAgent` call is a fresh review with no memory of prior rounds. This means the Analyst is the only agent that needs session management, context window handling, and user interaction logic.

3. **Markdown-only output**: Consistent with all other agents in the repo, the documentation output uses standard markdown only — no Mermaid, no HTML, no LaTeX. This ensures portability across terminals, web UIs, and markdown renderers.

4. **Fixed documentation section order**: The Transformation_Documentation follows a prescribed section order (Pipeline Overview → Source Tables → Transformation Steps → Business Rules → Data Lineage → Output Schema → Edge Cases and Assumptions → Open Questions → Review Log). This makes the output predictable and navigable.

## Components and Interfaces

### Component 1: Analyst Agent Prompt (`pipeline-analyst-agent-prompt.md`)

The Analyst Agent is the primary agent the user interacts with. It handles pipeline code ingestion, multi-pass analysis, documentation generation, and orchestrates the review loop by instructing the user to invoke the Reviewer Agent. It is structured using the same XML-like section markers as other agents in the repo:

| Section | Purpose |
|---|---|
| `<identity>` | Agent persona, role description, communication style, domain scope (SQL/PySpark), off-topic handling |
| `<methodology>` | Multi-pass analysis strategy, documentation generation workflow, review loop orchestration (using `invokeSubAgent` to call Reviewer, parsing subagent response, correction logic, convergence detection), Review_Log maintenance |
| `<session_protocol>` | Session initialization, pipeline code ingestion, analysis phase progression, session summary/restoration, context window management |
| `<interaction_patterns>` | Socratic questioning for ambiguities, iterative refinement, vague input handling, off-scope redirection |
| `<formatting>` | Conversational state markers, markdown rules, Transformation_Documentation template, Review_Log table format, terminal compatibility |

**Interfaces:**
- **Input**: User-provided Pipeline_Code (SQL, PySpark, or mixed) pasted into the conversation
- **Output**: Structured Transformation_Documentation in markdown, Review_Log table, session summaries
- **Subagent Invocation**: Uses `invokeSubAgent` to call the Reviewer Agent, passing Pipeline_Code + Transformation_Documentation via `prompt` parameter and the Reviewer prompt file via `contextFiles`. Parses the Reviewer's subagent response to extract findings and Confidence_Score.

### Component 2: Analyst_Role (within Analyst Agent Prompt)

The Analyst_Role is the primary analysis persona within the Analyst Agent. It executes the multi-pass analysis:

| Pass | Purpose | Output |
|---|---|---|
| Pass 1: Structural | Identify tables, CTEs, aliases, overall data flow, pipeline stages | Source table inventory, pipeline structure outline |
| Pass 2: Detailed | Extract transformation logic per step — joins, filters, aggregations, window functions, CASE logic, UDFs, type casts | Transformation steps in execution order, business rule annotations |
| Pass 3: Validation | Cross-reference lineage, verify output schema completeness, flag gaps and inconsistencies | Data lineage map, output schema, edge cases, open questions |

**Interfaces:**
- **Input**: Raw Pipeline_Code from user
- **Output**: Draft Transformation_Documentation (all sections)
- **Transition**: Invokes the Reviewer Agent via `invokeSubAgent`, passing only Pipeline_Code + Transformation_Documentation

### Component 3: Reviewer Agent Prompt (`pipeline-reviewer-agent-prompt.md`)

The Reviewer Agent is a separate agent prompt file, invoked programmatically by the Analyst Agent via Kiro's `invokeSubAgent` mechanism. It is stateless per invocation — each call is a fresh review with no memory of prior rounds. It receives only the Pipeline_Code and the Transformation_Documentation (passed via the `invokeSubAgent` `prompt` parameter). It has no access to the Analyst's conversation history or reasoning. Context isolation is enforced by architecture — the Reviewer runs as a separate agent execution.

The Reviewer Agent prompt is structured with the same XML-like section markers but with a narrower scope:

| Section | Purpose |
|---|---|
| `<identity>` | Reviewer persona, role description (critical reviewer of pipeline documentation), communication style, domain scope (SQL/PySpark) |
| `<methodology>` | Review checklist, finding categorization rules, Confidence_Score assignment criteria, output format |
| `<interaction_patterns>` | How to handle ambiguous findings, how to structure the review report |
| `<formatting>` | Review report template, finding categorization format, Confidence_Score format |

**Review checklist:**
- Missing transformations
- Incorrect lineage mappings
- Undocumented source tables
- Inaccurate business rule descriptions
- Gaps in edge case coverage
- Internal inconsistencies within the documentation

**Interfaces:**
- **Input**: Pipeline_Code + Transformation_Documentation (passed via `invokeSubAgent` — no Analyst reasoning, no conversation history)
- **Output**: Structured review report (findings categorized as error/omission/inconsistency/suggestion), Confidence_Score (0–100%), Score_Rationale (returned as subagent response)

### Component 4: Review_Log (maintained by Analyst Agent)

A markdown table maintained by the Analyst Agent across review rounds. The Analyst appends rows for both roles — Reviewer rows based on the Reviewer's returned output, and Analyst rows based on corrections made.

**Columns:**
| Column | Description |
|---|---|
| Round | Review_Round number (1–5) |
| Role | Analyst or Reviewer |
| Feedback Summary | One-sentence summary of feedback given |
| Change Summary | One-sentence summary of changes made |
| Outcome | corrected / no change needed / partially addressed / escalated to user |
| Confidence_Score | Reviewer's 0–100% rating (blank for Analyst rows) |
| Score_Rationale | One-sentence explanation of score factors (blank for Analyst rows) |

### Component 5: Platform Wrapper

One platform wrapper file covering setup for BOTH the Analyst Agent and the Reviewer Agent on Kiro:

| File | Platform | Setup |
|---|---|---|
| `platform-kiro.md` | Kiro | Both agent prompts deployed as steering files. The Analyst steering file is configured to invoke the Reviewer via `invokeSubAgent`, passing Pipeline_Code + Transformation_Documentation via the `prompt` parameter and pointing `contextFiles` to the Reviewer prompt file. The review loop runs automatically within the Analyst's execution. |

The wrapper includes: Setup instructions (for both agents), Platform Notes, Limitations, `invokeSubAgent` Configuration (how the Analyst calls the Reviewer programmatically), and a "What This Wrapper Does NOT Do" section. The wrapper does not modify the core prompt behavior.

### Component 6: Conversational State Markers

The Analyst Agent uses markers to structure output and signal phase transitions. The Reviewer Agent uses only `[VALIDATION REVIEW]` in its output.

| Marker | When Used | Agent |
|---|---|---|
| `[PIPELINE OVERVIEW]` | After structural pass, presenting pipeline summary | Analyst |
| `[TRANSFORMATION ANALYSIS]` | During detailed pass, presenting transformation steps | Analyst |
| `[LINEAGE]` | When presenting data lineage mappings | Analyst |
| `[DOCUMENTATION]` | When presenting the full Transformation_Documentation | Analyst |
| `[ANALYST PHASE]` | When operating in Analyst_Role (analysis or correction) | Analyst |
| `[REVIEWER PHASE]` | When preparing handoff to Reviewer Agent | Analyst |
| `[REVIEW ROUND N]` | At the start of each review round | Analyst |
| `[VALIDATION REVIEW]` | When presenting review findings | Reviewer |
| `[REVIEW LOG]` | When presenting the Review_Log table | Analyst |
| `[SUMMARY]` | When presenting session summaries | Analyst |


## Data Models

### Transformation_Documentation Structure

The final output document follows this fixed section order:

```markdown
# Pipeline Documentation: [Pipeline Name]

## 1. Pipeline Overview
- Pipeline name/description
- Language (SQL / PySpark / Mixed)
- High-level data flow summary
- Number of source tables, transformation steps, output tables

## 2. Source Tables
| Table Name | Alias(es) | Type | Usage Contexts |
|---|---|---|---|
| schema.table_a | a, t1 | Physical table | Join on line 12, filter on line 45 |
| cte_orders | — | CTE | Defined line 5–20, used in join line 30 |

## 3. Transformation Steps (in execution order)
### Step N: [Description]
- **Operation**: JOIN / FILTER / AGGREGATE / WINDOW / DERIVATION / etc.
- **Input columns**: [list]
- **Operation detail**: [SQL/PySpark expression or plain-language description]
- **Output columns**: [list]
- **Notes**: [conditional logic branches, NULL handling, etc.]

## 4. Business Rules
### BR-N: [Plain-language description]
- **Code reference**: [line numbers or expression]
- **Hardcoded values**: [flagged magic numbers/strings]
- **Interpretation**: [why this rule likely exists]
- **Confidence**: [high/medium/low — based on code context]

## 5. Data Lineage
| Output Column | Source Column(s) | Transformation Path |
|---|---|---|
| total_revenue | orders.amount, returns.refund | SUM(amount) - SUM(refund) via join on order_id |
| created_date | — | Generated: current_timestamp() |

## 6. Output Schema
| Column Name | Inferred Type | Derivation |
|---|---|---|
| customer_id | INT | Direct from customers.id |
| total_revenue | DECIMAL | Calculated: SUM(orders.amount) - SUM(returns.refund) |

## 7. Edge Cases and Assumptions
- **Assumed non-null**: [columns]
- **Assumed unique join keys**: [columns]
- **NULL handling**: [strategy per column]
- **Deduplication**: [strategy and columns]
- **Missing handling**: [flagged risks]

## 8. Open Questions
- [Ambiguities requiring user clarification]
- [Unresolved references]
- [UDFs with opaque logic]

## 9. Review Log
| Round | Role | Feedback Summary | Change Summary | Outcome | Confidence_Score | Score_Rationale |
|---|---|---|---|---|---|---|
| 1 | Reviewer | ... | ... | ... | 75% | ... |
| 1 | Analyst | ... | ... | corrected | — | — |
| 2 | Reviewer | ... | ... | ... | 96% | ... |
```

### Review Report Structure (Reviewer_Role output per round)

```markdown
[VALIDATION REVIEW]

## Review Round N Findings

### Errors
- [Finding]: [description with code reference]

### Omissions
- [Finding]: [description]

### Inconsistencies
- [Finding]: [description]

### Suggestions
- [Finding]: [description]

**Confidence_Score**: XX%
**Score_Rationale**: [one sentence]
```

### Session Summary Structure

```markdown
[SUMMARY]

**Session: Pipeline Documentation for [Pipeline Name]**

**Pipeline**: [brief description]
**Language**: SQL / PySpark / Mixed
**Analysis Progress**: [sections completed / sections pending]
**Key Findings**: [notable transformations, business rules, or edge cases discovered]
**Review Status**: [Round N of 5, Confidence_Score: XX%] or [Not yet started]
**Open Questions**: [unresolved items]

**To resume:** Paste this summary into a new session and say "Let us pick up where we left off."
```

### Agent Handoff Protocol

Context isolation between Analyst and Reviewer is enforced by architecture — the Reviewer runs as a separate agent execution via `invokeSubAgent`. The handoff is fully automated with no manual user intervention required.

1. **Analyst → Reviewer invocation**:
   - Analyst completes (or updates) Transformation_Documentation
   - Analyst outputs `[REVIEWER PHASE]` marker
   - Analyst uses `invokeSubAgent` to call the Reviewer Agent, passing: (a) the original Pipeline_Code and (b) the current Transformation_Documentation via the `prompt` parameter, with `contextFiles` pointing to the Reviewer Agent prompt file
   - The `invokeSubAgent` call contains ONLY Pipeline_Code + Transformation_Documentation — no Analyst reasoning, working notes, or conversation history

2. **Reviewer execution** (separate agent execution):
   - Reviewer Agent runs as a subagent with its own isolated context
   - Reviewer reviews the documentation against the code
   - Reviewer produces a structured review report with findings and Confidence_Score
   - Reviewer returns its response via the subagent response mechanism

3. **Reviewer → Analyst return**:
   - Analyst receives the Reviewer's subagent response
   - Analyst parses the Reviewer's findings and Confidence_Score
   - Analyst appends a Reviewer row to the Review_Log
   - If corrections are needed, Analyst makes corrections, appends an Analyst row to the Review_Log, and repeats from step 1

4. **Convergence**:
   - Reviewer assigns Confidence_Score ≥ 95% AND finds no errors/omissions
   - Analyst declares convergence, outputs final documentation with `[DOCUMENTATION]` marker
   - Analyst outputs complete Review_Log with `[REVIEW LOG]` marker

5. **Round cap**:
   - If 5 rounds complete without convergence, Analyst stops the loop
   - Presents current documentation + Review_Log to user
   - Informs user that convergence was not reached

6. **Disagreement handling**:
   - If Analyst disagrees with a Reviewer finding, the finding is escalated to the user rather than silently dropped
   - If Reviewer contradicts an Analyst assumption, the contradiction is flagged to the user for resolution


## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system — essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

Since this feature produces LLM prompt files (not executable code), the testable properties focus on the structural correctness of the prompt files, platform wrappers, and the output format templates. Properties that depend on LLM behavioral judgment (e.g., "identify business rules," "use Socratic questioning") are not computably testable and are excluded.

### Property 1: Source table alias resolution

*For any* SQL or PySpark code snippet containing aliased table references, the Transformation_Documentation's Source Tables section should list both the original table name and all aliases used for that table.

**Validates: Requirements 2.3**

### Property 2: Source table multi-reference tracking

*For any* pipeline code where a Source_Table is referenced in N distinct contexts (joins, filters, subqueries), the Source Tables section should document at least N distinct usage contexts for that table.

**Validates: Requirements 2.2**

### Property 3: Transformation documentation completeness

*For any* pipeline code containing K transformation operations (joins, filters, aggregations, window functions, derivations, type casts, CASE logic, UDFs, set operations), the Transformation Steps section should contain K documented steps, each with input columns, operation description, and output columns.

**Validates: Requirements 3.1, 3.2**

### Property 4: Conditional logic branch coverage

*For any* CASE/WHEN statement or if/else block in the pipeline code with B branches, the documentation should describe all B branches and their triggering conditions.

**Validates: Requirements 3.4**

### Property 5: Hardcoded value flagging

*For any* transformation containing hardcoded literals (magic numbers, string constants, date thresholds) within business rule logic, the Business Rules section should flag each hardcoded value.

**Validates: Requirements 4.3**

### Property 6: Data lineage completeness

*For any* output column in the pipeline, the Data Lineage section should either (a) map it to all contributing source columns with the transformation path, or (b) document the generation logic if it is a constant or generated value (e.g., `current_timestamp()`, a literal, a row number).

**Validates: Requirements 5.1, 5.2, 5.3**

### Property 7: Output schema completeness

*For any* output column produced by the pipeline, the Output Schema section should include the column name, a data type (either inferred from the code or marked as "unknown"), and a derivation description. For pipelines producing multiple outputs, each output should have its own schema section.

**Validates: Requirements 6.1, 6.2, 6.3**

### Property 8: NULL handling and deduplication documentation

*For any* pipeline code containing NULL handling logic (COALESCE, IFNULL, IS NOT NULL, NVL) or deduplication logic (DISTINCT, ROW_NUMBER partitioning, GROUP BY without aggregation), the Edge Cases and Assumptions section should document the strategy used and the columns involved.

**Validates: Requirements 7.2, 7.3**

### Property 9: Documentation section ordering

*For any* Transformation_Documentation output, the sections should appear in this exact order: Pipeline Overview, Source Tables, Transformation Steps, Business Rules, Data Lineage, Output Schema, Edge Cases and Assumptions, Open Questions, Review Log. No section should be missing.

**Validates: Requirements 9.1, 9.4, 17.6**

### Property 10: Markdown-only formatting

*For any* Transformation_Documentation output, the content should contain only standard markdown formatting (headers, lists, bold, italic, code blocks, tables). The output should not contain HTML tags, LaTeX notation, or Mermaid diagram syntax.

**Validates: Requirements 9.2**

### Property 11: Context isolation by architecture

*For any* invocation of the Reviewer Agent via `invokeSubAgent`, the input provided to the Reviewer should contain only the original Pipeline_Code and the Transformation_Documentation. The Analyst's intermediate reasoning, working notes, draft iterations, assumption rationale, and conversation history should not appear in the Reviewer's input. This is enforced by the `invokeSubAgent` mechanism — the Reviewer runs as a separate agent execution with only the explicitly passed context.

**Validates: Requirements 14.1, 15.1, 15.2, 15.5**

### Property 12: Review report finding categorization

*For any* review report produced by the Reviewer_Role, each finding should be categorized as exactly one of: error, omission, inconsistency, or suggestion. No finding should be uncategorized or use a category outside this set.

**Validates: Requirements 14.3**

### Property 13: Review loop round cap enforcement

*For any* validation cycle, the number of Review_Rounds should not exceed 5. If 5 rounds complete without convergence, the agent should stop the loop and present the current documentation and Review_Log.

**Validates: Requirements 16.2**

### Property 14: Convergence condition

*For any* Review_Round where the Reviewer assigns a Confidence_Score ≥ 95% and identifies zero errors and zero omissions, the agent should declare Convergence and end the review loop.

**Validates: Requirements 16.3**

### Property 15: Review_Log structure and row correctness

*For any* Review_Log table, it should have exactly these columns: Round, Role, Feedback Summary, Change Summary, Outcome, Confidence_Score, Score_Rationale. For Reviewer rows, Confidence_Score and Score_Rationale should be non-blank with the score between 0% and 100%. For Analyst rows, Confidence_Score and Score_Rationale should be blank. Each Feedback Summary and Change Summary should be one sentence or less.

**Validates: Requirements 17.1, 17.2, 17.3, 17.4, 18.1, 18.2**

### Property 16: Confidence_Score regression flagging

*For any* pair of consecutive Review_Rounds where the Confidence_Score in round N is lower than in round N-1, the agent output should contain an explicit regression flag with an explanation.

**Validates: Requirements 18.5**

### Property 17: Platform wrapper structural consistency

*For any* Platform_Wrapper file (platform-kiro.md), the file should contain these sections: Setup (with setup instructions for both the Analyst Agent and the Reviewer Agent, including `invokeSubAgent` configuration), Platform Notes, Limitations, and "What This Wrapper Does NOT Do." The wrapper should not contain `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, or `<formatting>` sections.

**Validates: Requirements 12.4**

### Property 18: Session summary structure

*For any* session summary output, it should contain: pipeline name/description, analysis progress (sections completed and pending), key findings, open questions, and a restoration note. The summary should be preceded by the `[SUMMARY]` marker.

**Validates: Requirements 11.1**

## Error Handling

Since this feature produces LLM prompt files rather than executable code, "error handling" refers to how the agent prompt instructs the LLM to handle problematic inputs and edge cases during conversation.

### Unsupported Language Input
- When the user provides code in a language other than SQL or PySpark, the agent informs the user and asks for supported code (Requirement 1.5).
- The agent does not attempt to analyze unsupported languages.

### Ambiguous Source Table References
- When a table reference cannot be resolved from the code alone, the agent flags the ambiguity and asks the user for clarification (Requirement 2.4).
- The agent does not guess — it surfaces the ambiguity explicitly.

### Opaque UDFs and External Dependencies
- When a transformation references a UDF whose logic is not visible in the provided code, the agent flags it as an external dependency and documents the call signature (Requirement 3.5).
- The agent does not fabricate UDF logic.

### Incomplete Lineage
- When lineage cannot be fully traced (dynamic SQL, runtime parameters, opaque UDFs), the agent documents the traceable portion and flags the gap (Requirement 5.4).

### Validation Inconsistencies
- When the validation pass finds inconsistencies (e.g., a column in lineage but not in output schema), the agent flags the inconsistency and attempts resolution (Requirement 8.2).

### Role Disagreements
- When the Analyst disagrees with a Reviewer finding, the disagreement is escalated to the user rather than silently dropped (Requirement 16.6).
- When the Reviewer contradicts an Analyst assumption, the contradiction is flagged to the user (Requirement 15.6).
- Since the Reviewer is a separate agent invoked via `invokeSubAgent` with no memory of prior rounds, each review is genuinely independent — reducing the risk of self-confirming bias.

### Convergence Failure
- If 5 review rounds complete without reaching 95% confidence, the agent stops the loop, presents the current documentation and Review_Log, and informs the user (Requirement 16.2).

### Confidence Score Regression
- If the Confidence_Score decreases between consecutive rounds, the agent flags the regression with an explanation (Requirement 18.5).

### Context Window Exhaustion
- When the conversation grows long, the agent proactively offers to generate a session summary to preserve context (Requirement 11.3).

## Testing Strategy

### Dual Testing Approach

This feature requires both unit tests and property-based tests. Since the deliverables are markdown prompt files (not executable code), testing focuses on:

1. **Structural validation** of the prompt files and output templates
2. **Format compliance** of the documentation output
3. **Protocol correctness** of the review loop logic

### Unit Tests

Unit tests cover specific examples and edge cases:

- **File existence**: Verify `pipeline-analyst-agent-prompt.md`, `pipeline-reviewer-agent-prompt.md`, `platform-kiro.md` exist in `PipelineDocumentation/`
- **Analyst prompt sections**: Verify the Analyst Agent prompt contains `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>` sections
- **Reviewer prompt sections**: Verify the Reviewer Agent prompt contains `<identity>`, `<methodology>`, `<interaction_patterns>`, `<formatting>` sections and does NOT contain `<session_protocol>` (stateless agent)
- **Marker definitions**: Verify all 10 conversational state markers are defined across the two agent prompts (Analyst owns 9, Reviewer owns `[VALIDATION REVIEW]`)
- **Convergence threshold**: Verify the 95% threshold and 5-round cap are specified in the Analyst Agent's methodology
- **Review_Log template**: Verify the Review_Log table template has exactly 7 columns with correct headers
- **Handoff block format**: Verify the Analyst Agent prompt defines an `invokeSubAgent` call pattern that includes Pipeline_Code and Transformation_Documentation parameters
- **Reviewer input scope**: Verify the Reviewer Agent prompt specifies that its only inputs are Pipeline_Code and Transformation_Documentation
- **Edge case: empty pipeline**: Verify the Analyst Agent prompt instructs the agent to handle empty or trivial input gracefully
- **Edge case: SQL-only vs PySpark-only**: Verify the Analyst Agent prompt covers both languages independently

### Property-Based Tests

Property-based tests validate universal properties across generated inputs. Use a property-based testing library (e.g., `fast-check` for TypeScript/JavaScript, `Hypothesis` for Python, or `QuickCheck` for Haskell).

Each property test should run a minimum of 100 iterations and be tagged with a comment referencing the design property.

**Tag format**: `Feature: pipeline-documentation, Property {number}: {property_text}`

Properties to implement as property-based tests:

| Property | Test Strategy |
|---|---|
| Property 9: Documentation section ordering | Generate random subsets of documentation sections, verify ordering is always correct |
| Property 10: Markdown-only formatting | Generate random documentation strings, verify no HTML/LaTeX/Mermaid content |
| Property 12: Review finding categorization | Generate random review findings, verify each is categorized as error/omission/inconsistency/suggestion |
| Property 13: Round cap enforcement | Generate random sequences of review rounds with varying confidence scores, verify loop never exceeds 5 |
| Property 14: Convergence condition | Generate random review rounds with scores ≥ 95% and zero errors, verify convergence is declared |
| Property 15: Review_Log structure | Generate random Review_Log tables, verify column structure, Reviewer rows have scores, Analyst rows have blank scores |
| Property 16: Confidence regression flagging | Generate random sequences of confidence scores, verify regression is flagged when score decreases |
| Property 17: Platform wrapper structure | Parse wrapper file, verify required sections exist (including `invokeSubAgent` configuration), verify both agent setup instructions present, and no core prompt sections leak in |

Properties 1–8 and 11, 18 are best validated through integration testing with actual LLM output or manual review, as they depend on the LLM's ability to parse and analyze pipeline code — not on structural rules that can be checked programmatically.

### Test Configuration

- Minimum 100 iterations per property test
- Each property test must reference its design document property number
- Tests should be runnable via standard test runner (`pytest`, `vitest`, `jest`, etc.)
- Each correctness property must be implemented by a single property-based test
