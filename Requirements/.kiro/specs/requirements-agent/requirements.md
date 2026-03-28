# Requirements Document

## Introduction

The Requirements Agent is a new AI agent to be added to the MyAgents repository under the `Requirements/` folder. It helps users define, elicit, organize, and refine requirements for any type of problem — software systems, processes, mechanisms, products, organizational structures, or any domain where structured requirements are valuable. The agent follows the same structural patterns as existing agents in the repository: a core system prompt with XML-style sections, conversational state markers, Socratic questioning, and platform wrappers for Claude, Gemini, and Kiro CLI.

## Glossary

- **Requirements_Agent**: The AI agent system prompt that guides users through requirements elicitation, organization, and refinement for any problem domain.
- **Core_Prompt**: The main system prompt file (`requirements-agent-prompt.md`) containing the agent's identity, methodology, session protocol, interaction patterns, and formatting rules.
- **Platform_Wrapper**: A deployment guide file (`platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`) that provides setup instructions for a specific platform without modifying agent behavior.
- **Source_Problem**: The user's problem, idea, or domain for which requirements are being defined — may be software, process, mechanism, product, organizational, or any other type.
- **Requirements_Document**: The structured output produced by the agent containing elicited, categorized, and validated requirements.
- **Elicitation_Phase**: The conversational phase where the agent draws out requirements from the user through Socratic questioning and structured prompts.
- **Requirement_Category**: A classification for requirements — functional, non-functional, constraints, assumptions, dependencies, or out-of-scope items.
- **Conversational_State_Marker**: A bracketed text label (e.g., `[ELICIT]`, `[ORGANIZE]`) used to structure agent output and orient the user within the session.

## Requirements

### Requirement 1: Core Prompt Structure

**User Story:** As a repository maintainer, I want the Requirements Agent core prompt to follow the same XML-style section structure as existing agents, so that the repository remains consistent and maintainable.

#### Acceptance Criteria

1. THE Core_Prompt SHALL contain an `<identity>` section defining the agent's persona, role, communication style, and off-topic handling.
2. THE Core_Prompt SHALL contain a `<methodology>` section defining the structured approach for requirements elicitation, organization, and refinement.
3. THE Core_Prompt SHALL contain a `<session_protocol>` section defining session initialization, conversation flow, and summary generation.
4. THE Core_Prompt SHALL contain an `<interaction_patterns>` section defining how the agent handles specific conversational situations.
5. THE Core_Prompt SHALL contain a `<formatting>` section defining conversational state markers, markdown rules, and terminal compatibility guidelines.
6. THE Core_Prompt SHALL be saved as `Requirements/requirements-agent-prompt.md`.

### Requirement 2: Agent Identity and Persona

**User Story:** As a user, I want the Requirements Agent to have a clear, domain-agnostic identity that inspires confidence in its ability to help me define requirements for any type of problem.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL present itself as an experienced requirements engineer and systems analyst with expertise across multiple domains — software, hardware, process, product, organizational, and mechanism design.
2. THE Requirements_Agent SHALL use clear, precise language calibrated to the user's domain and familiarity level.
3. THE Requirements_Agent SHALL avoid jargon until it has been introduced and defined in the conversation.
4. THE Requirements_Agent SHALL be direct and supportive without being condescending or overly formal.
5. WHEN the user asks a question outside the domain of requirements definition, THE Requirements_Agent SHALL acknowledge the question and redirect warmly to the requirements domain.

### Requirement 3: Problem Scoping

**User Story:** As a user, I want the agent to help me scope and clarify my problem before jumping into requirements, so that the requirements we define are grounded in a clear understanding of the problem space.

#### Acceptance Criteria

1. WHEN a new Source_Problem is introduced, THE Requirements_Agent SHALL ask clarifying questions to understand the problem domain, stakeholders, goals, and constraints before generating requirements.
2. THE Requirements_Agent SHALL use Socratic questioning to help the user articulate aspects of the problem they may not have considered.
3. WHEN the user provides a vague or overly broad Source_Problem, THE Requirements_Agent SHALL help narrow the scope by asking the user to identify the most critical aspects or boundaries.
4. THE Requirements_Agent SHALL produce a problem statement summary using the `[PROBLEM SCOPE]` marker and confirm it with the user before proceeding to elicitation.
5. IF the user provides a well-defined Source_Problem in their first message, THEN THE Requirements_Agent SHALL skip extended scoping and proceed to elicitation after a brief confirmation.

### Requirement 4: Requirements Elicitation

**User Story:** As a user, I want the agent to systematically draw out requirements from me through structured conversation, so that I capture requirements I might otherwise miss.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL guide the user through elicitation using the `[ELICIT]` conversational state marker.
2. THE Requirements_Agent SHALL prompt the user to consider functional requirements, non-functional requirements, constraints, assumptions, and dependencies for the Source_Problem.
3. THE Requirements_Agent SHALL use Socratic questioning to surface implicit requirements the user has not explicitly stated.
4. WHEN the user describes a requirement in vague terms, THE Requirements_Agent SHALL ask targeted follow-up questions to make the requirement specific and testable.
5. THE Requirements_Agent SHALL prompt the user to consider stakeholder perspectives beyond their own — asking who else is affected by or has a stake in the Source_Problem.
6. THE Requirements_Agent SHALL prompt the user to consider edge cases, failure modes, and unwanted scenarios as sources of additional requirements.

### Requirement 5: Requirements Organization and Categorization

**User Story:** As a user, I want the agent to organize elicited requirements into clear categories, so that the resulting requirements document is structured and navigable.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL categorize each requirement into one of the following Requirement_Categories: functional, non-functional, constraints, assumptions, dependencies, or out-of-scope.
2. THE Requirements_Agent SHALL present organized requirements using the `[ORGANIZE]` conversational state marker.
3. THE Requirements_Agent SHALL assign a unique identifier to each requirement for traceability.
4. THE Requirements_Agent SHALL group related requirements together within their categories.
5. WHEN a requirement does not clearly fit a single category, THE Requirements_Agent SHALL ask the user to clarify the intent or suggest the most appropriate category with reasoning.

### Requirement 6: Requirements Refinement and Validation

**User Story:** As a user, I want the agent to help me review and refine requirements for quality, so that the final requirements are clear, complete, testable, and free of common defects.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL review requirements for clarity, checking for vague terms, ambiguous language, and undefined terminology.
2. THE Requirements_Agent SHALL review requirements for testability, checking that each requirement has measurable or verifiable acceptance criteria.
3. THE Requirements_Agent SHALL review requirements for completeness, checking for gaps, missing edge cases, and unstated assumptions.
4. THE Requirements_Agent SHALL review requirements for consistency, checking for contradictions or conflicts between requirements.
5. THE Requirements_Agent SHALL present refinement suggestions using the `[REFINE]` conversational state marker.
6. WHEN the Requirements_Agent identifies a quality issue in a requirement, THE Requirements_Agent SHALL explain the issue and suggest a specific improvement.
7. THE Requirements_Agent SHALL confirm all refinements with the user before incorporating them into the Requirements_Document.

### Requirement 7: Requirements Document Generation

**User Story:** As a user, I want the agent to produce a structured requirements document as output, so that I have a usable artifact I can share, reference, and build upon.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL produce a Requirements_Document using the `[DOCUMENT]` conversational state marker when the user indicates readiness or when elicitation and refinement are complete.
2. THE Requirements_Document SHALL include: a problem statement, glossary of terms, categorized requirements with unique identifiers, assumptions, constraints, dependencies, and out-of-scope items.
3. THE Requirements_Document SHALL use standard markdown formatting that renders correctly in plain text terminals, web chat interfaces, and markdown renderers.
4. WHEN the user requests changes to the Requirements_Document, THE Requirements_Agent SHALL incorporate the feedback and regenerate the affected sections.
5. THE Requirements_Agent SHALL offer to produce the Requirements_Document at any point during the session if the user requests it, even if elicitation is not complete — noting which areas remain incomplete.

### Requirement 8: Domain Agnosticism

**User Story:** As a user, I want the agent to handle requirements for any type of problem — not just software — so that I can use it for process design, product development, organizational changes, mechanisms, and other domains.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL adapt its elicitation questions and requirement categories to the domain of the Source_Problem rather than defaulting to software-specific terminology.
2. WHEN the Source_Problem is a software system, THE Requirements_Agent SHALL include software-specific requirement types such as performance, security, scalability, and API contracts.
3. WHEN the Source_Problem is a process or mechanism, THE Requirements_Agent SHALL include process-specific requirement types such as inputs, outputs, triggers, roles, escalation paths, and success metrics.
4. WHEN the Source_Problem is a product, THE Requirements_Agent SHALL include product-specific requirement types such as user experience, market constraints, regulatory requirements, and acceptance criteria.
5. WHEN the Source_Problem domain is unclear, THE Requirements_Agent SHALL ask the user to describe the domain before selecting an elicitation approach.

### Requirement 9: Conversational State Markers

**User Story:** As a user, I want the agent to use consistent conversational state markers throughout the session, so that I can orient myself within the requirements process and scan the conversation easily.

#### Acceptance Criteria

1. THE Requirements_Agent SHALL use the `[PROBLEM SCOPE]` marker when presenting the problem statement summary.
2. THE Requirements_Agent SHALL use the `[ELICIT]` marker when actively drawing out requirements from the user.
3. THE Requirements_Agent SHALL use the `[ORGANIZE]` marker when presenting categorized requirements.
4. THE Requirements_Agent SHALL use the `[REFINE]` marker when reviewing and suggesting improvements to requirements.
5. THE Requirements_Agent SHALL use the `[DOCUMENT]` marker when presenting the final structured requirements document.
6. THE Requirements_Agent SHALL use the `[SUMMARY]` marker when producing a session summary.

### Requirement 10: Session Protocol

**User Story:** As a user, I want the agent to follow a clear session flow — from greeting through problem scoping, elicitation, organization, refinement, and document generation — so that the conversation feels structured and productive.

#### Acceptance Criteria

1. WHEN a new session begins, THE Requirements_Agent SHALL greet the user warmly and ask what problem they want to define requirements for.
2. WHEN the user is unsure where to start, THE Requirements_Agent SHALL offer prompts to help them surface a problem — such as asking about current pain points, upcoming projects, or decisions they need to make.
3. THE Requirements_Agent SHALL guide the session through the phases in order: problem scoping, elicitation, organization, refinement, and document generation.
4. THE Requirements_Agent SHALL allow the user to move between phases freely — returning to elicitation after refinement, or requesting a document before refinement is complete.
5. WHEN the user introduces a new Source_Problem within the same session, THE Requirements_Agent SHALL acknowledge the transition and begin the flow from problem scoping for the new problem.

### Requirement 11: Session Summary

**User Story:** As a user, I want the agent to produce a session summary on request, so that I can save my progress and resume in a future session.

#### Acceptance Criteria

1. WHEN the user requests a summary, THE Requirements_Agent SHALL produce a structured session summary using the `[SUMMARY]` marker.
2. THE session summary SHALL include: problems scoped, requirements elicited (count and categories), refinements made, and open threads remaining.
3. THE session summary SHALL be formatted so the user can paste it into a new session to restore context.
4. WHEN the conversation grows long, THE Requirements_Agent SHALL proactively offer to generate a summary for context preservation.

### Requirement 12: Platform Wrapper for Claude

**User Story:** As a user deploying on Claude, I want a platform wrapper that provides setup instructions without modifying agent behavior, so that I can deploy the Requirements Agent on Claude following the same pattern as other agents.

#### Acceptance Criteria

1. THE Platform_Wrapper for Claude SHALL be saved as `Requirements/platform-claude.md`.
2. THE Platform_Wrapper for Claude SHALL include setup instructions for both Projects (persistent agent) and Direct System Message (quick start) deployment options.
3. THE Platform_Wrapper for Claude SHALL include a Platform Notes section covering instruction-following characteristics, formatting support, and limitations.
4. THE Platform_Wrapper for Claude SHALL include a "What This Wrapper Does NOT Do" section stating that the wrapper does not modify agent behavior.
5. THE Platform_Wrapper for Claude SHALL follow the same structure and tone as `Learning/platform-claude.md`.

### Requirement 13: Platform Wrapper for Gemini

**User Story:** As a user deploying on Gemini, I want a platform wrapper that provides setup instructions without modifying agent behavior, so that I can deploy the Requirements Agent on Gemini following the same pattern as other agents.

#### Acceptance Criteria

1. THE Platform_Wrapper for Gemini SHALL be saved as `Requirements/platform-gemini.md`.
2. THE Platform_Wrapper for Gemini SHALL include setup instructions for both Custom Instructions (quick start) and Gems (dedicated agent) deployment options.
3. THE Platform_Wrapper for Gemini SHALL include a Platform Notes section covering formatting support and limitations.
4. THE Platform_Wrapper for Gemini SHALL include a "What This Wrapper Does NOT Do" section stating that the wrapper does not modify agent behavior.
5. THE Platform_Wrapper for Gemini SHALL follow the same structure and tone as `Learning/platform-gemini.md`.

### Requirement 14: Platform Wrapper for Kiro CLI

**User Story:** As a user deploying on Kiro CLI, I want a platform wrapper that provides setup instructions without modifying agent behavior, so that I can deploy the Requirements Agent on Kiro CLI following the same pattern as other agents.

#### Acceptance Criteria

1. THE Platform_Wrapper for Kiro CLI SHALL be saved as `Requirements/platform-kiro.md`.
2. THE Platform_Wrapper for Kiro CLI SHALL include setup instructions for deploying via a steering file at `.kiro/steering/requirements-agent.md`.
3. THE Platform_Wrapper for Kiro CLI SHALL include a Platform Notes section covering terminal-based output considerations and limitations.
4. THE Platform_Wrapper for Kiro CLI SHALL include a "What This Wrapper Does NOT Do" section stating that the wrapper does not modify agent behavior.
5. THE Platform_Wrapper for Kiro CLI SHALL follow the same structure and tone as `Learning/platform-kiro.md`.

### Requirement 15: Repository Structure Compliance

**User Story:** As a repository maintainer, I want the Requirements Agent to follow the same folder and file structure as existing agents, so that the repository remains consistent.

#### Acceptance Criteria

1. THE Requirements_Agent files SHALL be placed in the `Requirements/` folder at the repository root.
2. THE Requirements_Agent folder SHALL contain: `requirements-agent-prompt.md`, `platform-claude.md`, `platform-gemini.md`, and `platform-kiro.md`.
3. THE Requirements_Agent folder SHALL contain a `.kiro/` subfolder for Kiro specs.
4. THE file naming pattern SHALL follow the convention `{agent-name}-prompt.md` used by existing agents in the repository.
