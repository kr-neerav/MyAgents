# Requirements Document

## Introduction

The Memory Email Agent is a conversational agent that helps a parent compose beautifully formatted, heartfelt emails capturing memories and incidents for their daughter to read in the future. The parent describes a memory or incident, optionally attaches photos and videos, and the agent generates a polished email draft. The agent uses multiple-choice follow-up questions to gather additional context while keeping the interaction quick and easy. The agent's scope ends when the parent has a finalized email draft they are happy with.

## Glossary

- **Memory_Email_Agent**: The conversational agent that guides the parent through composing a memory email
- **Parent**: The user who describes memories and triggers email generation
- **Memory**: A described incident, moment, or story the parent wants to preserve for their daughter
- **Memory_Email**: The final formatted email output containing the memory narrative, media references, and heartfelt tone
- **Follow_Up_Question**: A multiple-choice question the agent asks to enrich the memory description
- **Subject_Line_Options**: A set of 4–5 proposed subject lines presented to the Parent for selection before email generation
- **Media_Attachment**: A photo or video the parent shares alongside the memory description
- **Core_Prompt**: The main prompt file containing all Memory_Email_Agent logic, persona, and behavior
- **Platform_Wrapper**: A platform-specific setup guide that provides deployment instructions without modifying agent behavior

## Requirements

### Requirement 1: Memory Description Intake

**User Story:** As a parent, I want to describe a memory or incident in my own words, so that the agent can use it as the basis for a heartfelt email.

#### Acceptance Criteria

1. WHEN the Parent provides a text description of a Memory, THE Memory_Email_Agent SHALL acknowledge receipt and begin processing the description.
2. THE Memory_Email_Agent SHALL accept free-form text descriptions of any length from the Parent.
3. IF the Parent provides an empty or blank description, THEN THE Memory_Email_Agent SHALL prompt the Parent to provide a description before proceeding.

### Requirement 2: Media Attachment Support

**User Story:** As a parent, I want to share photos and videos alongside my memory description, so that the email includes references to those moments.

#### Acceptance Criteria

1. WHEN the Parent shares one or more Media_Attachments alongside a Memory description, THE Memory_Email_Agent SHALL associate the attachments with the Memory.
2. THE Memory_Email_Agent SHALL support image file formats (JPEG, PNG, GIF) and video file formats (MP4, MOV) as Media_Attachments.
3. WHEN Media_Attachments are provided, THE Memory_Email_Agent SHALL include references or inline placements for the media in the generated Memory_Email.
4. THE Memory_Email_Agent SHALL proceed with email generation when no Media_Attachments are provided by the Parent.

### Requirement 3: Multiple-Choice Follow-Up Questions

**User Story:** As a parent, I want the agent to ask me follow-up questions as multiple-choice options, so that I can quickly add more detail without typing long responses.

#### Acceptance Criteria

1. WHEN the Parent submits a Memory description, THE Memory_Email_Agent SHALL generate one to three Follow_Up_Questions to enrich the Memory.
2. THE Memory_Email_Agent SHALL present each Follow_Up_Question with three to five multiple-choice options.
3. WHEN the Parent selects an option from a Follow_Up_Question, THE Memory_Email_Agent SHALL incorporate the selected detail into the Memory_Email.
4. THE Memory_Email_Agent SHALL provide a "Skip" option for each Follow_Up_Question so the Parent can proceed without answering.
5. WHILE the Parent has unanswered Follow_Up_Questions, THE Memory_Email_Agent SHALL allow the Parent to skip all remaining questions and proceed to email generation.

### Requirement 4: Email Generation

**User Story:** As a parent, I want the agent to generate a beautifully formatted, heartfelt email from my memory, so that my daughter can read it in the future and feel the love behind it.

#### Acceptance Criteria

1. WHEN the Parent has completed the memory intake (description, optional media, and follow-up answers), THE Memory_Email_Agent SHALL present Subject_Line_Options containing four to five subject line choices to the Parent before generating the full Memory_Email.
2. THE Memory_Email_Agent SHALL include a "Suggest different options" choice in the Subject_Line_Options so the Parent can request a new set of subject lines.
3. WHEN the Parent selects a subject line from the Subject_Line_Options, THE Memory_Email_Agent SHALL use the selected subject line in the generated Memory_Email.
4. WHEN the Parent selects "Suggest different options", THE Memory_Email_Agent SHALL generate a new set of four to five Subject_Line_Options and present them to the Parent.
5. THE Memory_Email_Agent SHALL write the Memory_Email in a warm, heartfelt, and personal tone addressed to the daughter.
6. THE Memory_Email_Agent SHALL format the Memory_Email with the selected subject line, greeting, narrative body, media references, and a closing.
7. THE Memory_Email_Agent SHALL use HTML formatting in the Memory_Email to support rich text styling (bold, italic, paragraph spacing, inline images).
8. WHEN the Memory_Email is generated, THE Memory_Email_Agent SHALL present a preview of the email to the Parent for review.

### Requirement 5: Email Review and Edit

**User Story:** As a parent, I want to review and request changes to the generated email before it is finalized, so that I can make sure it captures the memory the way I want.

#### Acceptance Criteria

1. WHEN the Parent reviews the Memory_Email preview, THE Memory_Email_Agent SHALL offer options to approve, edit, or regenerate the email.
2. WHEN the Parent requests an edit, THE Memory_Email_Agent SHALL present multiple-choice edit suggestions (e.g., "Make it more emotional", "Shorten it", "Add more detail", "Change the tone") along with a free-text option.
3. WHEN the Parent selects an edit option, THE Memory_Email_Agent SHALL regenerate the Memory_Email incorporating the requested change.
4. WHEN the Parent approves the Memory_Email, THE Memory_Email_Agent SHALL mark the email draft as finalized and present the final version to the Parent.

### Requirement 6: Platform Wrappers for Gemini, Kiro CLI, and Claude

**User Story:** As a developer, I want the Memory Email Agent to have a core prompt file and platform-specific wrapper files, so that the agent can be deployed consistently across Claude, Kiro CLI, and Gemini without duplicating or diverging agent logic.

#### Acceptance Criteria

1. THE Memory_Email_Agent SHALL have a Core_Prompt file containing all agent logic, persona, and behavior.
2. THE Memory_Email_Agent SHALL have a Platform_Wrapper file for each supported platform: Claude, Kiro CLI, and Gemini.
3. THE Memory_Email_Agent SHALL store the Core_Prompt and Platform_Wrapper files in the agent's directory following the established repository pattern (core prompt as `memory-email-agent-prompt.md`, wrappers as `platform-claude.md`, `platform-kiro.md`, `platform-gemini.md`).
4. WHEN a Platform_Wrapper is created, THE Platform_Wrapper SHALL contain only deployment and setup instructions for the target platform.
5. THE Platform_Wrapper SHALL reference the Core_Prompt as the single source of agent logic and instruct the user to copy the Core_Prompt contents into the platform's instruction mechanism.
6. THE Platform_Wrapper SHALL NOT modify, override, or extend the Memory_Email_Agent behavior defined in the Core_Prompt.
7. WHEN the Core_Prompt is updated, THE Platform_Wrapper files SHALL remain valid without requiring changes, because Platform_Wrapper files contain only platform-specific setup steps.
