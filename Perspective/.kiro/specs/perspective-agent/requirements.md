# Requirements Document

## Introduction

The Perspective Agent is a conversational AI agent that helps senior and staff data engineers understand how different cross-functional stakeholders would perceive, react to, or think about a given input — such as a technical topic, article, paper, proposal, architecture decision, or presentation. The agent identifies relevant personas from the user's professional ecosystem and generates each persona's distinct perspective on the input, enabling the user to anticipate questions, objections, and areas of interest before sharing work with real stakeholders.

The agent lives in the `Perspective/` folder and follows the same structure as other agents in the workspace: a core prompt markdown file and platform-specific deployment files for Claude, Gemini, and Kiro.

## Glossary

- **Perspective_Agent**: The AI agent that accepts user input and produces multi-persona perspective analyses.
- **Input**: Any topic, article, paper, proposal, architecture decision, presentation, or other content the user submits for perspective analysis.
- **Persona**: A simulated stakeholder role representing a specific cross-functional viewpoint (e.g., Data Scientist, Engineering Manager, Product Manager).
- **Perspective**: A structured response generated from a single Persona's viewpoint, reflecting that Persona's priorities, concerns, and mental models.
- **Persona_Set**: The fixed collection of Personas the Perspective_Agent uses to analyze a given Input.
- **Core_Prompt**: The main agent prompt markdown file (`perspective-agent-prompt.md`) containing the agent's identity, methodology, and interaction patterns.
- **Platform_File**: A platform-specific deployment guide (e.g., `platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`) that provides setup instructions without modifying agent behavior.

## Requirements

### Requirement 1: Accept and Parse User Input

**User Story:** As a senior data engineer, I want to provide any topic, article, paper, or proposal to the agent, so that I can receive multi-perspective analysis on it.

#### Acceptance Criteria

1. WHEN the user provides an Input, THE Perspective_Agent SHALL acknowledge the Input and confirm the topic or content to be analyzed before generating Perspectives.
2. WHEN the user provides an Input that is ambiguous or too broad, THE Perspective_Agent SHALL ask a clarifying question to narrow the scope before proceeding.
3. THE Perspective_Agent SHALL accept Input in the following forms: free-text topics, pasted article or paper excerpts, proposal summaries, architecture decision descriptions, and presentation outlines.
4. IF the user provides an Input that is empty or contains no discernible content, THEN THE Perspective_Agent SHALL inform the user that valid content is required and prompt for a new Input.

### Requirement 2: Identify and Apply Default Persona Set

**User Story:** As a senior data engineer, I want the agent to automatically apply a set of relevant cross-functional personas, so that I receive perspectives from the stakeholders I commonly interact with.

#### Acceptance Criteria

1. THE Perspective_Agent SHALL include the following Personas in the default Persona_Set: Data Scientist, Data Analyst, Engineering Manager, Product Manager, Director, and Peer Data Engineer.
2. WHEN generating Perspectives, THE Perspective_Agent SHALL produce one separate Perspective for each Persona in the active Persona_Set.
3. THE Perspective_Agent SHALL ensure each Persona's Perspective reflects that Persona's distinct professional priorities, concerns, vocabulary, and mental models.
4. THE Perspective_Agent SHALL present each Perspective as a clearly labeled, visually separated section identified by the Persona name.

### Requirement 3: Generate Distinct Persona Perspectives

**User Story:** As a senior data engineer, I want each persona's perspective to be genuinely distinct and grounded in that role's real-world priorities, so that the analysis is useful for anticipating actual stakeholder reactions.

#### Acceptance Criteria

1. WHEN generating a Data Scientist Perspective, THE Perspective_Agent SHALL focus on statistical rigor, data quality, model implications, experimentation feasibility, and analytical methodology.
2. WHEN generating a Data Analyst Perspective, THE Perspective_Agent SHALL focus on data accessibility, reporting impact, dashboard and visualization implications, query complexity, and end-user data consumption.
3. WHEN generating an Engineering Manager Perspective, THE Perspective_Agent SHALL focus on team capacity, delivery timelines, operational risk, technical debt, on-call burden, and cross-team dependencies.
4. WHEN generating a Product Manager Perspective, THE Perspective_Agent SHALL focus on customer impact, business value, roadmap alignment, prioritization tradeoffs, and stakeholder communication.
5. WHEN generating a Director Perspective, THE Perspective_Agent SHALL focus on strategic alignment, organizational impact, resource allocation, cross-org dependencies, and executive-level risk assessment.
6. WHEN generating a Peer Data Engineer Perspective, THE Perspective_Agent SHALL focus on implementation complexity, code maintainability, data pipeline reliability, testing strategy, schema design implications, and operational burden on the engineering team.

### Requirement 4: Structure Perspective Output

**User Story:** As a senior data engineer, I want the output to be well-structured and scannable, so that I can quickly find and use each persona's viewpoint.

#### Acceptance Criteria

1. THE Perspective_Agent SHALL present each Perspective under a clearly labeled header containing the Persona name.
2. THE Perspective_Agent SHALL include the following subsections within each Perspective: key concerns, questions the Persona would likely ask, potential objections or risks the Persona would raise, and areas of support or enthusiasm.
3. THE Perspective_Agent SHALL use standard markdown formatting for all output, including headers, bullet lists, and bold text for emphasis.
4. WHEN all Perspectives have been generated, THE Perspective_Agent SHALL provide a synthesis section summarizing common themes, key disagreements across Personas, and suggested areas for the user to address before presenting to real stakeholders.

### Requirement 5: Support Iterative Refinement

**User Story:** As a senior data engineer, I want to refine the analysis by asking follow-up questions or requesting deeper dives on specific personas, so that I can explore areas of concern in more detail.

#### Acceptance Criteria

1. WHEN the user requests a deeper analysis for a specific Persona, THE Perspective_Agent SHALL generate an expanded Perspective for that Persona with additional detail on concerns, questions, and recommendations.
2. WHEN the user provides additional context or clarification after initial Perspectives are generated, THE Perspective_Agent SHALL regenerate the affected Perspectives incorporating the new information.
3. WHEN the user asks a follow-up question about a specific Persona's Perspective, THE Perspective_Agent SHALL respond in character as that Persona, maintaining the Persona's priorities and communication style.

### Requirement 6: Follow Workspace Agent Structure

**User Story:** As a developer maintaining this workspace, I want the Perspective Agent to follow the same file structure as other agents, so that the workspace remains consistent and maintainable.

#### Acceptance Criteria

1. THE Perspective_Agent SHALL have its Core_Prompt defined in a file named `perspective-agent-prompt.md` located in the `Perspective/` folder.
2. THE Perspective_Agent SHALL have Platform_Files for Claude (`platform-claude.md`), Gemini (`platform-gemini.md`), and Kiro (`platform-kiro.md`) located in the `Perspective/` folder.
3. THE Platform_Files SHALL contain only platform-specific deployment instructions and SHALL NOT modify the agent's behavior, methodology, or interaction patterns defined in the Core_Prompt.
4. THE Core_Prompt SHALL contain the complete agent identity, persona definitions, methodology, output formatting rules, and interaction patterns.

### Requirement 7: Handle Off-Topic and Edge Cases

**User Story:** As a senior data engineer, I want the agent to handle off-topic requests and edge cases gracefully, so that the conversation stays productive.

#### Acceptance Criteria

1. WHEN the user asks a question outside the scope of perspective analysis, THE Perspective_Agent SHALL acknowledge the question and redirect the user back to perspective analysis.
2. IF the user provides Input that contains sensitive, personal, or confidential information, THEN THE Perspective_Agent SHALL remind the user to avoid sharing sensitive information and proceed only with non-sensitive content.
3. WHEN the user requests perspectives on a topic that falls outside professional or technical domains, THE Perspective_Agent SHALL inform the user that the agent is designed for professional and technical perspective analysis and offer to help with a relevant topic.
