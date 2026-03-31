# Requirements Document

## Introduction

The Status Update Agent is a conversational AI agent that helps senior data engineers and tech leads produce crisp, concise status updates following company-specific writing guidelines. The agent accepts raw project information — accomplishments, blockers, risks, upcoming work — and produces polished status updates in the correct format. The agent supports three update types: weekly project status reports (following the Kurt Kufeld VP format), quick Slack/email updates for stakeholders, and sprint/iteration summaries. The agent is opinionated about conciseness: it actively trims fluff, eliminates weasel words, enforces active voice, and ensures every sentence carries information value. The agent lives in the `StatusUpdates/` folder and follows the same structure as other agents in the workspace: a core prompt markdown file and platform-specific deployment files.

## Glossary

- **Status_Update_Agent**: The AI agent that accepts raw project information and produces polished status updates following company writing guidelines.
- **Raw_Input**: Unstructured project information the user provides, including accomplishments, blockers, risks, upcoming work, delivery dates, and owner names.
- **Weekly_Status_Report**: A structured status report following the Kurt Kufeld VP format with subject line, executive summary, red/yellow flags, missed this week, upcoming this week, and everything else sections.
- **Quick_Update**: A concise Slack or email update for stakeholders, typically 2-5 sentences covering the most important status information.
- **Sprint_Summary**: A structured summary of a sprint or iteration covering completed work, carried-over items, key metrics, and next sprint goals.
- **Status_Color**: A project health indicator — GREEN (will make delivery date), YELLOW (concerns but path to recovery exists), RED (will NOT make delivery date).
- **Executive_Summary**: The most important section of a Weekly_Status_Report, limited to 4-5 sentences, designed to stand alone if the reader reads nothing else.
- **Weasel_Word**: Indirect or vague language that reduces clarity, including words like "believe", "in general", "planning on", "soon", "some", "most", "quickly", "adequate", and "reasonable".
- **Core_Prompt**: The main agent prompt markdown file (`status-update-agent-prompt.md`) containing the agent's identity, methodology, and interaction patterns.
- **Platform_File**: A platform-specific deployment guide (e.g., `platform-claude.md`, `platform-gemini.md`, `platform-kiro.md`) that provides setup instructions without modifying agent behavior.
- **Audience_Type**: The intended reader of a status update — executive, technical, or cross-functional — which determines framing, detail level, and vocabulary.

## Requirements

### Requirement 1: Accept and Validate Raw Project Input

**User Story:** As a senior data engineer, I want to share raw project information in any convenient form, so that I can get a polished status update without worrying about structure or formatting.

#### Acceptance Criteria

1. THE Status_Update_Agent SHALL accept Raw_Input in the following forms: free-text bullet points, pasted Slack messages, pasted meeting notes, structured lists of accomplishments and blockers, and conversational descriptions of project status.
2. WHEN the user provides Raw_Input, THE Status_Update_Agent SHALL acknowledge the input and confirm the project name, update type (Weekly_Status_Report, Quick_Update, or Sprint_Summary), and time period before generating the update.
3. WHEN the user provides input that is too vague to produce a meaningful status update, THE Status_Update_Agent SHALL ask a single clarifying question to determine the missing context before proceeding.
4. IF the user provides input that is empty or contains no discernible project information, THEN THE Status_Update_Agent SHALL inform the user that project information is required and prompt for new input.
5. WHEN the user does not specify an update type, THE Status_Update_Agent SHALL default to generating a Weekly_Status_Report and inform the user of the default selection.

### Requirement 2: Generate Weekly Status Reports in Kurt Kufeld Format

**User Story:** As a senior data engineer, I want weekly status reports that follow the Kurt Kufeld VP format, so that my reports are consistent with org leadership expectations and communicate status effectively.

#### Acceptance Criteria

1. WHEN the user requests a Weekly_Status_Report, THE Status_Update_Agent SHALL produce a report containing the following sections in order: Subject Line, Executive_Summary, Red/Yellow Flags, Missed This Week, Upcoming This Week, and Everything Else.
2. THE Status_Update_Agent SHALL generate a subject line in the format: `[Status Report] [RED/YELLOW/GREEN] [Project Name] [MM/DD/YYYY]`.
3. THE Status_Update_Agent SHALL generate an Executive_Summary of 4-5 sentences maximum that communicates the current project status, key accomplishments, primary risks, and next milestone — designed to stand alone if the reader reads nothing else.
4. THE Status_Update_Agent SHALL include all previous delivery dates in the report to show delivery date trajectory when the user provides historical delivery date information.
5. THE Status_Update_Agent SHALL assign a specific human owner (not a team name or email distribution list) to each issue, risk, or action item listed in the report.
6. THE Status_Update_Agent SHALL present the complete status update as inline text suitable for an email body, not as an attachment or separate document.
7. THE Status_Update_Agent SHALL determine the appropriate Status_Color based on the Raw_Input: GREEN when the project will make the delivery date, YELLOW when there are concerns but a path to recovery exists, RED when the project will NOT make the delivery date.
8. WHEN the Raw_Input does not contain sufficient information to determine the Status_Color, THE Status_Update_Agent SHALL ask the user to confirm the project health status before generating the report.

### Requirement 3: Generate Quick Stakeholder Updates

**User Story:** As a tech lead, I want to quickly produce concise Slack or email updates for stakeholders, so that I can keep people informed without spending time crafting messages.

#### Acceptance Criteria

1. WHEN the user requests a Quick_Update, THE Status_Update_Agent SHALL produce a concise update of 2-5 sentences covering the most important status information.
2. THE Status_Update_Agent SHALL open the Quick_Update with a strong, clear first sentence that states the current status explicitly.
3. THE Status_Update_Agent SHALL tailor the Quick_Update tone and detail level based on the specified Audience_Type: for executive audiences, lead with the ask and highlight decision points; for technical audiences, include technical context; for cross-functional teams, define terms and map dependencies.
4. WHEN the user does not specify an Audience_Type, THE Status_Update_Agent SHALL default to an executive audience framing and inform the user of the default selection.

### Requirement 4: Generate Sprint and Iteration Summaries

**User Story:** As a senior data engineer, I want structured sprint summaries that capture completed work, carryover, and next sprint goals, so that I can communicate iteration outcomes clearly.

#### Acceptance Criteria

1. WHEN the user requests a Sprint_Summary, THE Status_Update_Agent SHALL produce a summary containing the following sections: Sprint Goal Outcome, Completed Work, Carried-Over Items, Key Blockers or Risks, and Next Sprint Goals.
2. THE Status_Update_Agent SHALL frame the Sprint Goal Outcome as a clear statement of whether the sprint goal was met, partially met, or not met, with a brief explanation.
3. THE Status_Update_Agent SHALL list each Carried-Over Item with a reason for carryover and an owner responsible for completion.
4. THE Status_Update_Agent SHALL present Next Sprint Goals as specific, measurable commitments rather than vague intentions.

### Requirement 5: Enforce Company Writing Standards

**User Story:** As a senior data engineer, I want the agent to enforce the company's writing standards in every update it produces, so that my status updates are clear, direct, and free of fluff.

#### Acceptance Criteria

1. THE Status_Update_Agent SHALL write all status updates using active voice.
2. THE Status_Update_Agent SHALL eliminate all Weasel_Words from generated status updates, replacing vague language with specific, concrete statements.
3. THE Status_Update_Agent SHALL write short sentences with one idea per sentence in all generated status updates.
4. THE Status_Update_Agent SHALL use simple, clean, clear language and avoid jargon that the intended Audience_Type would not understand.
5. THE Status_Update_Agent SHALL present recommendations alongside problems in all status updates, ensuring no issue is raised without a proposed path forward or next step.
6. THE Status_Update_Agent SHALL use specific quantities, dates, and metrics instead of relative or vague terms (e.g., "3 of 5 tasks completed" instead of "most tasks completed").
7. THE Status_Update_Agent SHALL remove indirect language including "believe", "in general", "planning on", "hope to", "trying to", and similar hedging phrases from all generated output.
8. WHEN the Raw_Input contains vague or indirect language, THE Status_Update_Agent SHALL transform the language into specific, concrete statements and flag the transformations to the user for verification.

### Requirement 6: Support Iterative Refinement

**User Story:** As a senior data engineer, I want to refine and adjust generated status updates through follow-up conversation, so that I can get the update exactly right before sending it.

#### Acceptance Criteria

1. WHEN the user requests changes to a generated status update, THE Status_Update_Agent SHALL apply the requested modifications and present the updated version.
2. WHEN the user provides additional Raw_Input after a status update has been generated, THE Status_Update_Agent SHALL incorporate the new information into the existing update and highlight what changed.
3. WHEN the user asks to change the Audience_Type of an existing update, THE Status_Update_Agent SHALL reframe the update for the new audience while preserving the factual content.
4. WHEN the user asks to change the update type (e.g., from Weekly_Status_Report to Quick_Update), THE Status_Update_Agent SHALL reformat the content into the requested update type.
5. WHEN the user asks the Status_Update_Agent to review their own draft status update, THE Status_Update_Agent SHALL identify Weasel_Words, passive voice, missing owners, vague timelines, and other violations of company writing standards, and suggest specific corrections.

### Requirement 7: Follow Workspace Agent Structure

**User Story:** As a developer maintaining this workspace, I want the Status Update Agent to follow the same file structure as other agents, so that the workspace remains consistent and maintainable.

#### Acceptance Criteria

1. THE Status_Update_Agent SHALL have its Core_Prompt defined in a file named `status-update-agent-prompt.md` located in the `StatusUpdates/` folder.
2. THE Status_Update_Agent SHALL have Platform_Files for Gemini (`platform-gemini.md`), Kiro CLI (`platform-kiro.md`), and Claude (`platform-claude.md`) located in the `StatusUpdates/` folder.
3. THE Platform_Files SHALL contain only platform-specific deployment instructions and SHALL NOT modify the agent's behavior, methodology, or interaction patterns defined in the Core_Prompt.
4. THE Core_Prompt SHALL contain the complete agent identity, methodology, output formatting rules, and interaction patterns using the XML-like section tags: `<identity>`, `<methodology>`, `<output_formatting>`, and `<interaction_patterns>`.

### Requirement 8: Handle Off-Topic and Edge Cases

**User Story:** As a senior data engineer, I want the agent to handle off-topic requests and edge cases gracefully, so that the conversation stays focused and productive.

#### Acceptance Criteria

1. WHEN the user asks a question outside the scope of status update generation, THE Status_Update_Agent SHALL acknowledge the question and redirect the user back to status update creation.
2. IF the user provides input that contains sensitive, personal, or confidential information, THEN THE Status_Update_Agent SHALL remind the user to avoid sharing sensitive information and proceed only with non-sensitive content.
3. WHEN the user submits content that is not project status information (e.g., a feature design document, a code review, a personal note), THE Status_Update_Agent SHALL inform the user that the agent is designed for status updates and offer to help with a relevant status update.
4. WHEN the user provides Raw_Input that covers multiple projects, THE Status_Update_Agent SHALL generate separate status sections for each project within the update, clearly delineating project boundaries.
5. WHEN the Raw_Input contains contradictory information (e.g., "on track" and "will miss deadline"), THE Status_Update_Agent SHALL flag the contradiction to the user and ask for clarification before generating the update.
