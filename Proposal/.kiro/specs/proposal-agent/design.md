# Design Document: Proposal Agent

## Overview

The Proposal Agent is a prompt-based AI agent that guides users through structuring and writing compelling proposals across any domain. It follows the established agent architecture in this workspace: a single core prompt file containing all agent logic in XML-like sections, plus three platform wrapper files for Claude, Gemini, and Kiro deployment.

The agent produces four output files:
- `Proposal/proposal-agent-prompt.md` — core prompt with `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, and `<formatting>` sections
- `Proposal/platform-claude.md` — Claude deployment guide
- `Proposal/platform-gemini.md` — Gemini deployment guide
- `Proposal/platform-kiro.md` — Kiro deployment guide

The agent is not a software system with runtime components. It is a set of static markdown files that, when loaded into an LLM platform, produce a conversational agent with a specific persona and methodology. The "architecture" is the prompt structure itself.

## Architecture

### System Context

The Proposal Agent operates within the same ecosystem as the existing agents (Design, Requirements, Learning, Mentor, Memory Email). Each agent is a self-contained prompt that can be deployed independently on any of the three supported platforms.

```
┌─────────────────────────────────────────────────┐
│                  User                           │
│  (wants to draft a proposal)                    │
└──────────────┬──────────────────────────────────┘
               │
               ▼
┌─────────────────────────────────────────────────┐
│           LLM Platform                          │
│  (Claude / Gemini / Kiro)                       │
│                                                 │
│  ┌───────────────────────────────────────────┐  │
│  │  Core Prompt (proposal-agent-prompt.md)   │  │
│  │  loaded as system instructions            │  │
│  └───────────────────────────────────────────┘  │
│                                                 │
│  Platform Wrapper provides setup instructions   │
│  for loading the core prompt into this platform │
└─────────────────────────────────────────────────┘
```

### Design Rationale

The architecture mirrors the existing agents exactly. This is a deliberate choice:
- Consistency across the workspace makes all agents maintainable by the same person
- Platform wrappers are interchangeable — the same core prompt works on all three platforms
- No runtime dependencies, databases, or APIs — the agent is pure prompt engineering

## Components and Interfaces

The system has four components (files), each with a clear responsibility.

### Component 1: Core Prompt (`proposal-agent-prompt.md`)

**Responsibility:** Contains the complete agent logic — identity, methodology, session protocol, interaction patterns, and formatting rules.

**Internal Structure (XML-like sections):**

| Section | Responsibility |
|---|---|
| `<identity>` | Role definition, communication style, domain agnosticism, off-topic handling |
| `<methodology>` | Six-phase proposal drafting flow: Problem Framing → Context & Stakeholders → Solution Proposal → Alternatives Analysis → Impact & Risks → Document Generation |
| `<session_protocol>` | Session initialization, phase progression, new problem handling, session summary, context window management |
| `<interaction_patterns>` | Socratic questioning, vague input handling, stakeholder prompting, domain adaptation |
| `<formatting>` | State markers, markdown rules, terminal compatibility |

**Interfaces:**
- Input: User messages during a conversation
- Output: Structured responses using state markers (`[PROBLEM FRAMING]`, `[CONTEXT]`, `[SOLUTION]`, `[ALTERNATIVES]`, `[IMPACT]`, `[PROPOSAL DOCUMENT]`, `[SUMMARY]`)

### Component 2: Claude Platform Wrapper (`platform-claude.md`)

**Responsibility:** Deployment instructions for Claude (Projects and Direct System Message approaches).

**Interface:** References `proposal-agent-prompt.md` as the source of agent logic. Contains no agent behavior.

### Component 3: Gemini Platform Wrapper (`platform-gemini.md`)

**Responsibility:** Deployment instructions for Gemini (Custom Instructions and Gems approaches).

**Interface:** References `proposal-agent-prompt.md` as the source of agent logic. Contains no agent behavior.

### Component 4: Kiro Platform Wrapper (`platform-kiro.md`)

**Responsibility:** Deployment instructions for Kiro CLI (Steering file approach).

**Interface:** References `proposal-agent-prompt.md` as the source of agent logic. Contains no agent behavior.

### Component Relationships

```
platform-claude.md ──references──▶ proposal-agent-prompt.md
platform-gemini.md ──references──▶ proposal-agent-prompt.md
platform-kiro.md   ──references──▶ proposal-agent-prompt.md
```

The platform wrappers are leaf nodes — they depend on the core prompt but nothing depends on them. The core prompt is self-contained and has no external dependencies.

## Data Models

Since this is a prompt engineering project (not a software system with persistent storage), the "data models" are the structured output formats the agent produces during a session.

### Session State (Conversational)

The agent tracks the following state implicitly through conversation context:

- **Current Phase**: Which of the six methodology phases the user is in (Problem Framing, Context & Stakeholders, Solution Proposal, Alternatives Analysis, Impact & Risks, Document Generation)
- **Gathered Context**: Problem statement, stakeholders, constraints, proposed solution, alternatives, risks, and success criteria accumulated across phases
- **Phase Completion**: Which phases have been visited and what content was gathered in each

### Proposal Document Structure

The final generated document follows this structure:

```markdown
[PROPOSAL DOCUMENT]

## Executive Summary
[Brief overview of the problem and proposed solution]

## Problem Statement
[Detailed articulation of the problem or opportunity]

## Proposed Solution
[Description of the recommended approach and its key components]

## Alternatives Considered
[Each alternative with tradeoffs]

## Impact Analysis
### Benefits
[Expected positive outcomes]
### Risks and Mitigations
[Identified risks with mitigation strategies]

## Success Criteria
[Measurable criteria for evaluating the proposal's success]

## Next Steps
[Concrete actions to move forward]
```

### Session Summary Structure

```markdown
[SUMMARY]

**Session: Proposal for [Topic]**

**Proposal Topic:** [Brief description]
**Phases Completed:** [List of completed phases]
**Key Decisions:** [Major decisions made during drafting]
**Open Threads:** [Unresolved items]

**To resume:** Paste this summary into a new session and say "Let us pick up where we left off."
```

### State Markers

| Marker | Phase | Purpose |
|---|---|---|
| `[PROBLEM FRAMING]` | Problem Framing | Present the problem statement summary |
| `[CONTEXT]` | Context & Stakeholders | Present stakeholder analysis and context |
| `[SOLUTION]` | Solution Proposal | Present the proposed solution |
| `[ALTERNATIVES]` | Alternatives Analysis | Present alternatives and tradeoffs |
| `[IMPACT]` | Impact & Risks | Present benefits, risks, success criteria |
| `[PROPOSAL DOCUMENT]` | Document Generation | Present the full proposal document |
| `[SUMMARY]` | Any | Present session summary for context preservation |


## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system — essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

This project is a prompt engineering project — the deliverables are static markdown files, not a running software system. Most acceptance criteria describe runtime LLM conversational behavior (e.g., "the agent SHALL ask clarifying questions"), which cannot be verified through automated testing of the static files. The testable properties focus on structural correctness of the output files.

### Property 1: Platform wrappers reference the core prompt

*For any* platform wrapper file (`platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`), the file content SHALL contain a reference to `proposal-agent-prompt.md` as the source of agent logic.

**Validates: Requirements 2.4**

### Property 2: Platform wrappers contain no agent behavior logic

*For any* platform wrapper file, the file content SHALL NOT contain any of the methodology phase names (`Problem Framing`, `Context and Stakeholders`, `Solution Proposal`, `Alternatives Analysis`, `Impact and Risks`) used as behavioral instructions, SHALL NOT contain XML-like section tags (`<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`), and SHALL NOT define Socratic questioning rules or domain adaptation logic.

**Validates: Requirements 2.5**

### Property 3: No prohibited formatting in output files

*For any* output file in the Proposal directory, the file content SHALL NOT contain HTML tags (e.g., `<div>`, `<span>`, `<table>`), LaTeX notation (e.g., `\begin{`, `$$`), Mermaid diagram blocks (e.g., ` ```mermaid`), embedded image references, or platform-specific formatting features.

**Validates: Requirements 10.3, 12.2**

### Property 4: Core prompt structural completeness

*For any* valid core prompt file, it SHALL contain all five required XML-like sections (`<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`), all seven required state markers (`[PROBLEM FRAMING]`, `[CONTEXT]`, `[SOLUTION]`, `[ALTERNATIVES]`, `[IMPACT]`, `[PROPOSAL DOCUMENT]`, `[SUMMARY]`), and the six methodology phases defined in the correct order.

**Validates: Requirements 1.1, 1.2, 1.3, 1.4, 1.5, 5.1, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7**

## Error Handling

Since this is a prompt engineering project producing static markdown files, traditional error handling (exceptions, retries, circuit breakers) does not apply. The relevant "error handling" is about what happens when the generated files are malformed or incomplete.

### File Generation Errors

- If the core prompt is missing any required XML section, the task implementation should fail validation (Property 4).
- If a platform wrapper accidentally includes agent logic, it should fail validation (Property 2).
- If any file contains prohibited formatting, it should fail validation (Property 3).

### Runtime Agent Behavior (Prompt-Level)

The core prompt itself includes error handling instructions for the LLM at runtime:
- **Vague input**: The prompt instructs the agent to ask targeted follow-up questions rather than guessing (Requirement 8).
- **Off-topic input**: The prompt instructs the agent to acknowledge warmly and redirect (Requirement 11).
- **Incomplete phases**: The prompt instructs the agent to produce a document with incompleteness notes rather than refusing (Requirement 5.9).
- **Context window exhaustion**: The prompt instructs the agent to proactively offer summaries before context is lost (Requirement 9.3).

These are not testable through automated tests — they are behavioral instructions embedded in the prompt that rely on the LLM's instruction-following capability.

## Testing Strategy

### Dual Testing Approach

Given the nature of this project (static markdown files, not executable code), the testing strategy focuses on structural validation of the output files.

**Unit Tests (Example-Based):**
- Verify the core prompt file exists at `Proposal/proposal-agent-prompt.md`
- Verify each platform wrapper file exists at its expected path
- Verify the proposal document template structure contains all required sections (executive summary, problem statement, proposed solution, alternatives considered, impact analysis, success criteria, next steps)
- Verify the six methodology phases appear in the correct order in the core prompt

**Property-Based Tests:**
- Use a property-based testing library (e.g., fast-check for TypeScript/JavaScript, or Hypothesis for Python) to validate structural properties across all output files
- Each property test should run a minimum of 100 iterations
- Each property test must reference its design document property with a comment tag

**Property Test Configuration:**
- Tag format: **Feature: proposal-agent, Property {number}: {property_text}**
- Minimum 100 iterations per property test
- Property tests validate structural invariants of the generated files

### Test Mapping

| Property | Test Type | What It Validates |
|---|---|---|
| Property 1: Platform wrappers reference core prompt | Property test | All 3 wrappers reference `proposal-agent-prompt.md` |
| Property 2: No agent logic in wrappers | Property test | Wrappers contain only deployment instructions |
| Property 3: No prohibited formatting | Property test | No HTML/LaTeX/Mermaid in any output file |
| Property 4: Core prompt structural completeness | Property test | All sections, markers, and phases present |

### Practical Testing Notes

Because the deliverables are static markdown files and most requirements describe LLM runtime behavior, the automated test surface is limited to file structure validation. The majority of acceptance criteria (Requirements 3, 4, 5.2–5.9, 7, 8, 9, 11) describe conversational behaviors that can only be validated through manual testing — actually loading the prompt into an LLM and running through proposal drafting sessions.

Manual testing should cover:
- Session initialization across all three platforms
- Complete proposal drafting flow through all six phases
- Phase navigation (skipping, returning to earlier phases)
- Domain adaptation (testing with software, business, policy, and budget proposal topics)
- Vague input handling and Socratic questioning behavior
- Session summary generation and context restoration
