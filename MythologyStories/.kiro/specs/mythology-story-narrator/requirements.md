# Requirements Document

## Introduction

The Mythology Story Narrator Agent is an AI agent that narrates mythology stories from a RAG-backed corpus. Unlike the existing Mythology Scholar Agent (which performs scholarly analysis), this agent is a storyteller. It walks through a corpus of mythology stories sequentially, chapter by chapter, narrating each story in an entertaining and accessible way. Each narration includes relevant backstory context and concludes with a real-world lesson. A continuation marker system allows the user to advance through the corpus one story at a time.

The deliverable is a complete agent prompt file plus platform-specific setup guides (Kiro CLI, Claude, Gemini), all placed in the MythologyStories/ folder.

## Glossary

- **Narrator_Agent**: The Mythology Story Narrator AI agent that narrates stories from the corpus
- **Corpus**: The full collection of mythology stories provided to the Narrator_Agent via the RAG system
- **RAG_System**: The Retrieval-Augmented Generation system that supplies mythology story content to the Narrator_Agent
- **Story**: A single self-contained mythology narrative within the Corpus, which may span one or more chapters
- **Chapter**: A subdivision of a Story representing a discrete narrative segment
- **Backstory_Summary**: A condensed recap of prior events, characters, or context needed to understand the current Story
- **Continuation_Marker**: A structured token emitted at the end of each narration that encodes the current position in the Corpus and signals the Narrator_Agent to proceed to the next Story when fed back by the user
- **Real_World_Lesson**: A practical, present-day applicable insight derived from the themes of a narrated Story
- **Platform_Guide**: A platform-specific setup document that provides deployment instructions for a particular AI platform (Kiro CLI, Claude, or Gemini)

## Requirements

### Requirement 1: RAG Corpus Integration

**User Story:** As a user, I want the Narrator_Agent to draw stories from a RAG-backed mythology corpus, so that narrations are grounded in source material rather than generated from general knowledge.

#### Acceptance Criteria

1. WHEN a session begins, THE Narrator_Agent SHALL retrieve story content from the RAG_System before narrating
2. THE Narrator_Agent SHALL narrate stories using content sourced from the Corpus rather than fabricating story details from general training data
3. IF the RAG_System returns no results for a requested story, THEN THE Narrator_Agent SHALL inform the user that the requested content is not available in the Corpus and suggest available alternatives

### Requirement 2: Sequential Chapter Narration

**User Story:** As a user, I want the Narrator_Agent to narrate stories in sequential order starting from the first chapter, so that I experience the corpus in its intended narrative progression.

#### Acceptance Criteria

1. WHEN a new session begins without a Continuation_Marker, THE Narrator_Agent SHALL start narrating from the first Story in the Corpus
2. WHEN a user provides a Continuation_Marker, THE Narrator_Agent SHALL resume narration from the next Story indicated by the marker
3. THE Narrator_Agent SHALL narrate one Story at a time, completing the current Story before proceeding to the next
4. THE Narrator_Agent SHALL progress through the Corpus in the order the stories appear, from first to last

### Requirement 3: Entertaining and Complete Narration

**User Story:** As a user, I want each story narrated in an entertaining, complete, and accessible way, so that I can enjoy and understand the mythology without needing prior knowledge.

#### Acceptance Criteria

1. THE Narrator_Agent SHALL narrate each Story using vivid, engaging language that maintains reader interest
2. WHEN a Story references events, characters, or context from earlier in the Corpus, THE Narrator_Agent SHALL include a Backstory_Summary providing the relevant prior context
3. THE Narrator_Agent SHALL include all significant plot points, characters, and events from the source material without omitting key details
4. THE Narrator_Agent SHALL use simple, accessible language that does not require specialized mythology knowledge to understand
5. THE Narrator_Agent SHALL maintain a consistent storyteller voice throughout the narration

### Requirement 4: Real-World Lessons

**User Story:** As a user, I want a real-world lesson at the end of each story, so that I can connect ancient mythology themes to practical insights for modern life.

#### Acceptance Criteria

1. WHEN the Narrator_Agent completes narrating a Story, THE Narrator_Agent SHALL provide a Real_World_Lesson derived from the themes of that Story
2. THE Real_World_Lesson SHALL be grounded in specific themes or events from the narrated Story rather than generic moral advice
3. THE Real_World_Lesson SHALL describe a practical insight applicable to present-day situations

### Requirement 5: Continuation Marker System

**User Story:** As a user, I want a continuation marker at the end of each narration that I can send back to the agent to get the next story, so that I can control the pacing and the agent knows where to resume.

#### Acceptance Criteria

1. WHEN the Narrator_Agent finishes narrating a Story and its Real_World_Lesson, THE Narrator_Agent SHALL emit a Continuation_Marker at the end of the response
2. THE Continuation_Marker SHALL encode the current position in the Corpus so the Narrator_Agent can identify the next Story to narrate
3. WHEN the user sends a Continuation_Marker back to the Narrator_Agent, THE Narrator_Agent SHALL parse the marker and narrate the next sequential Story
4. IF the user sends an invalid or unrecognizable Continuation_Marker, THEN THE Narrator_Agent SHALL inform the user that the marker is not recognized and offer to start from the beginning or from a specific point in the Corpus
5. WHEN the Narrator_Agent reaches the last Story in the Corpus, THE Narrator_Agent SHALL inform the user that the Corpus narration is complete instead of emitting a Continuation_Marker

### Requirement 6: Agent Prompt Deliverable

**User Story:** As a developer, I want a complete agent prompt file following the established workspace conventions, so that the Narrator_Agent can be deployed consistently across platforms.

#### Acceptance Criteria

1. THE Narrator_Agent prompt file SHALL be a standalone markdown file located at MythologyStories/mythology-story-narrator-prompt.md
2. THE Narrator_Agent prompt file SHALL contain the full agent identity, narration methodology, session protocol, and formatting rules in a single self-contained document
3. THE Narrator_Agent prompt file SHALL follow the structural conventions used by existing agents in the workspace (identity, methodology, session protocol, interaction patterns, formatting sections)

### Requirement 7: Platform Setup Guides

**User Story:** As a developer, I want platform-specific setup guides for Kiro CLI, Claude, and Gemini, so that I can deploy the Narrator_Agent on any supported platform.

#### Acceptance Criteria

1. THE Platform_Guide for Kiro CLI SHALL be located at MythologyStories/platform-kiro.md and SHALL provide deployment instructions using the Kiro steering file mechanism
2. THE Platform_Guide for Claude SHALL be located at MythologyStories/platform-claude.md and SHALL provide deployment instructions using Claude Projects or direct system message approaches
3. THE Platform_Guide for Gemini SHALL be located at MythologyStories/platform-gemini.md and SHALL provide deployment instructions using Gemini Custom Instructions or Gems approaches
4. EACH Platform_Guide SHALL reference the core prompt file (mythology-story-narrator-prompt.md) as the source of all agent behavior and SHALL NOT duplicate or modify agent logic
