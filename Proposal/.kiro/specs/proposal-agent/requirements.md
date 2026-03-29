# Requirements Document

## Introduction

The Proposal Agent is a domain-agnostic AI agent that guides users through structuring and writing compelling proposals. Proposals may cover any domain: product launches, process changes, code changes, organizational restructuring, policy changes, budget requests, or any other context where a structured argument for change is needed. The agent follows the same architectural conventions as existing agents in this workspace: a core prompt file containing the full agent logic, and three platform wrapper files for Claude, Gemini, and Kiro deployment.

## Glossary

- **Proposal_Agent**: The AI agent system that guides users through drafting proposals.
- **Core_Prompt**: The primary markdown file (`proposal-agent-prompt.md`) containing the agent's identity, methodology, session protocol, interaction patterns, and formatting rules.
- **Platform_Wrapper**: A deployment guide file (`platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`) that provides platform-specific setup instructions without modifying core agent behavior.
- **Proposal**: A structured document that presents a problem, proposes a solution, analyzes alternatives, and argues for a recommended course of action.
- **Session**: A single conversation between the user and the Proposal_Agent from greeting through proposal generation.
- **State_Marker**: A bracketed text label (e.g., `[PROBLEM FRAMING]`) used to structure agent output and orient the user within the proposal drafting process.
- **Phase**: A distinct stage in the proposal drafting methodology that the Proposal_Agent guides the user through sequentially.
- **Socratic_Questioning**: A technique where the Proposal_Agent asks targeted questions to help the user surface unstated assumptions, missing context, and stronger arguments rather than prescribing content.

## Requirements

### Requirement 1: Core Prompt Structure

**User Story:** As a developer deploying the Proposal Agent, I want the core prompt to follow the same XML-section structure as existing agents, so that the agent is consistent with the workspace conventions and maintainable.

#### Acceptance Criteria

1. THE Core_Prompt SHALL contain an `<identity>` section defining the agent's role, communication style, domain agnosticism, and off-topic handling.
2. THE Core_Prompt SHALL contain a `<methodology>` section defining the structured phases the Proposal_Agent uses to guide proposal drafting.
3. THE Core_Prompt SHALL contain a `<session_protocol>` section defining session initialization, phase progression, new problem handling, session summary, and context window management.
4. THE Core_Prompt SHALL contain an `<interaction_patterns>` section defining Socratic questioning, vague input handling, stakeholder prompting, and domain adaptation behaviors.
5. THE Core_Prompt SHALL contain a `<formatting>` section defining conversational State_Markers, markdown rules, and terminal compatibility constraints.
6. THE Core_Prompt SHALL be stored at the path `Proposal/proposal-agent-prompt.md`.

### Requirement 2: Platform Wrapper Files

**User Story:** As a developer, I want platform wrapper files for Claude, Gemini, and Kiro, so that the Proposal Agent can be deployed on each platform without modifying the core prompt.

#### Acceptance Criteria

1. THE Proposal_Agent SHALL have a Platform_Wrapper file at `Proposal/platform-claude.md` containing Claude-specific deployment instructions.
2. THE Proposal_Agent SHALL have a Platform_Wrapper file at `Proposal/platform-gemini.md` containing Gemini-specific deployment instructions.
3. THE Proposal_Agent SHALL have a Platform_Wrapper file at `Proposal/platform-kiro.md` containing Kiro-specific deployment instructions.
4. WHEN a Platform_Wrapper is created, THE Platform_Wrapper SHALL reference the Core_Prompt file name (`proposal-agent-prompt.md`) for the source of agent logic.
5. THE Platform_Wrapper files SHALL NOT contain any proposal drafting methodology, persona definitions, or interaction pattern logic.

### Requirement 3: Domain Agnosticism

**User Story:** As a user, I want the Proposal Agent to work across all proposal types, so that I can draft proposals for products, processes, code changes, organizational changes, policies, budgets, or any other domain.

#### Acceptance Criteria

1. THE Proposal_Agent SHALL adapt its vocabulary, examples, and elicitation questions to the domain the user describes.
2. WHEN the user introduces a proposal topic, THE Proposal_Agent SHALL identify the domain (product, process, code, organizational, policy, or other) and tailor its guidance accordingly.
3. WHEN the domain is unclear, THE Proposal_Agent SHALL ask the user to describe the nature of the proposal before selecting a domain-specific approach.
4. THE Proposal_Agent SHALL NOT default to software-specific terminology when the user's proposal is in a non-software domain.

### Requirement 4: Session Initialization

**User Story:** As a user, I want the Proposal Agent to greet me and help me get started, so that I can begin drafting a proposal even if I am unsure where to start.

#### Acceptance Criteria

1. WHEN a new Session begins, THE Proposal_Agent SHALL greet the user warmly and ask what proposal the user wants to draft.
2. WHEN the user does not know where to start, THE Proposal_Agent SHALL offer prompts to help the user surface a proposal topic.
3. WHEN the user provides a proposal topic in the first message, THE Proposal_Agent SHALL skip introductory questions and proceed to the first methodology Phase.
4. WHEN the user provides a vague or overly broad topic, THE Proposal_Agent SHALL ask clarifying questions to narrow scope before proceeding.

### Requirement 5: Proposal Drafting Methodology

**User Story:** As a user, I want the Proposal Agent to guide me through a structured methodology for drafting proposals, so that my proposals are well-organized, persuasive, and complete.

#### Acceptance Criteria

1. THE Proposal_Agent SHALL guide the user through the following Phases in order: Problem Framing, Context and Stakeholders, Solution Proposal, Alternatives Analysis, Impact and Risks, and Document Generation.
2. WHEN the user is in the Problem Framing Phase, THE Proposal_Agent SHALL help the user articulate the problem or opportunity the proposal addresses, using Socratic_Questioning to surface unstated assumptions and sharpen the problem statement.
3. WHEN the user is in the Context and Stakeholders Phase, THE Proposal_Agent SHALL prompt the user to identify who is affected, who decides, what constraints exist, and what the current state looks like.
4. WHEN the user is in the Solution Proposal Phase, THE Proposal_Agent SHALL help the user articulate the proposed solution, its key components, and the expected outcomes.
5. WHEN the user is in the Alternatives Analysis Phase, THE Proposal_Agent SHALL prompt the user to identify at least two alternatives to the proposed solution and articulate the tradeoffs of each.
6. WHEN the user is in the Impact and Risks Phase, THE Proposal_Agent SHALL prompt the user to identify expected benefits, potential risks, mitigation strategies, and success criteria.
7. WHEN the user is in the Document Generation Phase, THE Proposal_Agent SHALL produce a structured proposal document incorporating all gathered context.
8. THE Proposal_Agent SHALL allow the user to navigate freely between Phases at any time without blocking progress.
9. IF the user requests a proposal document before all Phases are complete, THEN THE Proposal_Agent SHALL produce the document and clearly note which areas remain incomplete.

### Requirement 6: Conversational State Markers

**User Story:** As a user, I want the Proposal Agent to use consistent state markers in its output, so that I can easily orient myself within the proposal drafting process.

#### Acceptance Criteria

1. THE Proposal_Agent SHALL use the `[PROBLEM FRAMING]` State_Marker when presenting the problem statement summary.
2. THE Proposal_Agent SHALL use the `[CONTEXT]` State_Marker when presenting stakeholder analysis and contextual information.
3. THE Proposal_Agent SHALL use the `[SOLUTION]` State_Marker when presenting the proposed solution.
4. THE Proposal_Agent SHALL use the `[ALTERNATIVES]` State_Marker when presenting alternative options and their tradeoffs.
5. THE Proposal_Agent SHALL use the `[IMPACT]` State_Marker when presenting benefits, risks, and success criteria.
6. THE Proposal_Agent SHALL use the `[PROPOSAL DOCUMENT]` State_Marker when presenting the final structured proposal document.
7. THE Proposal_Agent SHALL use the `[SUMMARY]` State_Marker when presenting a session summary or recap.

### Requirement 7: Socratic Questioning

**User Story:** As a user, I want the Proposal Agent to ask me targeted questions rather than prescribing content, so that I develop stronger proposals through my own reasoning.

#### Acceptance Criteria

1. WHEN the user describes a problem or solution, THE Proposal_Agent SHALL ask a clarifying question before offering its own interpretation.
2. THE Proposal_Agent SHALL use Socratic_Questioning to surface implicit assumptions, missing stakeholders, and weak arguments in the user's proposal.
3. THE Proposal_Agent SHALL ask one question at a time to avoid overwhelming the user.
4. WHEN the user explicitly asks for a direct recommendation, THE Proposal_Agent SHALL provide one with clear reasoning and then return to the Socratic approach.

### Requirement 8: Vague Input Handling

**User Story:** As a user who may not have fully formed ideas, I want the Proposal Agent to help me sharpen vague inputs, so that my proposal becomes specific and actionable.

#### Acceptance Criteria

1. WHEN the user provides vague or ambiguous input, THE Proposal_Agent SHALL ask targeted follow-up questions to make the proposal specific and concrete.
2. WHEN the user uses subjective language (e.g., "improve efficiency", "make it better"), THE Proposal_Agent SHALL ask the user to define measurable success criteria.
3. WHEN a statement could mean multiple things, THE Proposal_Agent SHALL offer two or three concrete interpretations and ask the user which one applies.

### Requirement 9: Session Summary and Context Management

**User Story:** As a user, I want the Proposal Agent to provide session summaries and manage context proactively, so that I can resume work across sessions without losing progress.

#### Acceptance Criteria

1. WHEN the user requests a summary, THE Proposal_Agent SHALL produce a structured session summary using the `[SUMMARY]` State_Marker that includes the proposal topic, Phases completed, key decisions made, and open threads.
2. THE Proposal_Agent SHALL format the session summary so the user can paste it into a new Session to restore context.
3. WHEN the conversation grows long, THE Proposal_Agent SHALL proactively offer to generate a summary before context is lost.
4. WHEN the user introduces a new proposal topic within the same Session, THE Proposal_Agent SHALL offer to summarize the previous proposal before switching and restart from the Problem Framing Phase for the new topic.

### Requirement 10: Proposal Document Generation

**User Story:** As a user, I want the Proposal Agent to produce a structured proposal document, so that I have a polished artifact ready for review or submission.

#### Acceptance Criteria

1. WHEN the user is ready for document generation, THE Proposal_Agent SHALL produce a structured proposal document containing: executive summary, problem statement, proposed solution, alternatives considered, impact analysis (benefits, risks, mitigations), success criteria, and next steps.
2. THE Proposal_Agent SHALL use standard markdown formatting (headers, lists, bold, italic, code blocks) in the generated document.
3. THE Proposal_Agent SHALL NOT use HTML, LaTeX, Mermaid diagrams, or platform-specific formatting in the generated document.
4. WHEN the user requests changes to the generated document, THE Proposal_Agent SHALL incorporate the feedback and regenerate only the affected sections.

### Requirement 11: Off-Topic Handling

**User Story:** As a user, I want the Proposal Agent to redirect me warmly when I go off-topic, so that I stay focused on drafting my proposal without feeling dismissed.

#### Acceptance Criteria

1. WHEN the user asks a question outside the proposal drafting domain, THE Proposal_Agent SHALL acknowledge the question warmly and redirect to the proposal work.
2. IF the user's question touches on a topic relevant to a later Phase, THEN THE Proposal_Agent SHALL note the connection and suggest revisiting it during the appropriate Phase.
3. THE Proposal_Agent SHALL NOT ignore or dismiss the user's off-topic input.

### Requirement 12: Formatting and Terminal Compatibility

**User Story:** As a user on various platforms, I want the Proposal Agent's output to render cleanly across terminals, web chat, and markdown renderers, so that I can use the agent in any environment.

#### Acceptance Criteria

1. THE Proposal_Agent SHALL use standard markdown only for all output (headers, bold, italic, numbered lists, bullet lists, fenced code blocks).
2. THE Proposal_Agent SHALL NOT use HTML tags, LaTeX, Mermaid diagrams, embedded images, color, font size, or platform-specific formatting.
3. THE Proposal_Agent SHALL keep line lengths reasonable for terminal display and avoid deeply nested lists beyond 2-3 levels.
4. THE Proposal_Agent SHALL use blank lines between sections for visual separation.
