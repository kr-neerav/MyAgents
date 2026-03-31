# Requirements Document

## Introduction

The SDD Agent brings Kiro's Spec Driven Development experience to other LLM platforms — Gemini, Kiro CLI, and Claude. It is a system prompt that turns any LLM into an SDD companion, guiding users through the full spec lifecycle: requirements gathering, design, and task generation. The agent replicates Kiro's structured, iterative, phase-based methodology as a conversational experience that works on any platform. The deliverable is a core prompt file (`sdd-agent-prompt.md`) and three platform setup guides (`platform-gemini.md`, `platform-kiro.md`, `platform-claude.md`), following the same folder structure and conventions as all other agents in the repository.

## Glossary

- **SDD_Agent**: The LLM persona defined by the core system prompt that guides users through Spec Driven Development workflows.
- **Spec**: A structured set of documents — requirements, design, and tasks — that define a feature or bugfix from problem statement through implementation plan.
- **Phase**: One of the three sequential stages of the SDD workflow: Requirements, Design, or Tasks.
- **EARS_Pattern**: Easy Approach to Requirements Syntax — a structured sentence pattern for writing unambiguous requirements (Ubiquitous, Event-driven, State-driven, Unwanted-event, Optional-feature, Complex).
- **Acceptance_Criteria**: Testable conditions attached to each requirement that define when the requirement is satisfied.
- **Correctness_Property**: A property-based testing pattern (invariant, round-trip, idempotence, metamorphic, model-based, confluence, error-condition) used to verify program correctness.
- **Design_Document**: A structured artifact covering high-level architecture, component decomposition, interfaces, data models, and design decisions with rationale.
- **Task_Document**: A structured breakdown of implementation work into ordered, actionable tasks with sub-tasks, dependencies, and acceptance criteria.
- **Session_Summary**: A portable, self-contained recap of session progress that can be pasted into a new session to restore context.
- **Conversational_State_Marker**: A text marker (e.g., `[REQUIREMENTS]`, `[DESIGN]`, `[TASKS]`) used to structure output and orient the user within the workflow.
- **Platform_Guide**: A setup document that provides deployment instructions for a specific LLM platform without modifying the core agent behavior.

## Requirements

### Requirement 1: Phase-Based Workflow Guidance

**User Story:** As a developer, I want the SDD Agent to guide me through a structured Requirements → Design → Tasks workflow, so that I produce a complete spec for my feature or bugfix.

#### Acceptance Criteria

1. WHEN a user starts a new session, THE SDD_Agent SHALL greet the user and ask what feature or bugfix they want to spec out.
2. WHEN the user describes a feature idea, THE SDD_Agent SHALL begin the Requirements phase by generating an initial requirements document using EARS_Pattern syntax and INCOSE quality rules.
3. WHEN the Requirements phase is complete and the user confirms, THE SDD_Agent SHALL transition to the Design phase and generate a Design_Document based on the confirmed requirements.
4. WHEN the Design phase is complete and the user confirms, THE SDD_Agent SHALL transition to the Tasks phase and generate a Task_Document based on the confirmed design.
5. THE SDD_Agent SHALL use Conversational_State_Markers to indicate the current phase at all times.
6. WHEN the user requests to return to a previous phase, THE SDD_Agent SHALL navigate back to that phase and allow modifications without losing work from later phases.

### Requirement 2: Requirements Generation

**User Story:** As a developer, I want the SDD Agent to generate structured requirements with acceptance criteria, so that I have a clear, testable definition of what I am building.

#### Acceptance Criteria

1. WHEN the user provides a feature description, THE SDD_Agent SHALL generate requirements following exactly one EARS_Pattern per requirement (Ubiquitous, Event-driven, State-driven, Unwanted-event, Optional-feature, or Complex).
2. THE SDD_Agent SHALL attach a user story to each requirement in the format: "As a [role], I want [feature], so that [benefit]."
3. THE SDD_Agent SHALL attach Acceptance_Criteria to each requirement, with each criterion following an EARS_Pattern.
4. THE SDD_Agent SHALL include a Glossary section defining all system names and technical terms used in the requirements.
5. WHEN a requirement contains vague terms, passive voice, pronouns, escape clauses, or negative statements, THE SDD_Agent SHALL correct the violation and explain the correction to the user.
6. WHEN the feature involves a parser or serializer, THE SDD_Agent SHALL include explicit requirements for the parser, a pretty printer, and a round-trip property (parse → print → parse produces an equivalent object).
7. THE SDD_Agent SHALL generate the initial requirements document without asking additional clarifying questions first, then iterate based on user feedback.

### Requirement 3: Design Document Generation

**User Story:** As a developer, I want the SDD Agent to produce a structured design document from my requirements, so that I have a clear technical blueprint before implementation.

#### Acceptance Criteria

1. WHEN transitioning to the Design phase, THE SDD_Agent SHALL produce a Design_Document that includes: high-level architecture overview, component decomposition with responsibilities, interface definitions, data models, design decisions with rationale, and tradeoffs.
2. THE SDD_Agent SHALL trace each design component back to the requirements it addresses, using requirement identifiers.
3. WHEN the design involves competing quality attributes, THE SDD_Agent SHALL surface the tradeoff explicitly and present options with consequences.
4. THE SDD_Agent SHALL use Socratic questioning to help the user reason through design decisions rather than prescribing solutions.
5. WHEN the user explicitly asks for a direct recommendation, THE SDD_Agent SHALL provide one with clear reasoning and tradeoffs.

### Requirement 4: Task Document Generation

**User Story:** As a developer, I want the SDD Agent to break my design into actionable implementation tasks, so that I have a clear plan for building the feature.

#### Acceptance Criteria

1. WHEN transitioning to the Tasks phase, THE SDD_Agent SHALL produce a Task_Document that breaks the design into ordered, numbered tasks.
2. THE SDD_Agent SHALL decompose each task into sub-tasks where the task involves multiple distinct implementation steps.
3. THE SDD_Agent SHALL include acceptance criteria for each task that define when the task is complete.
4. THE SDD_Agent SHALL order tasks to respect dependencies, placing foundational work before dependent work.
5. THE SDD_Agent SHALL trace each task back to the design component and requirement it implements.

### Requirement 5: Iterative Refinement

**User Story:** As a developer, I want to review and refine each phase's output before moving on, so that the spec accurately reflects my intent.

#### Acceptance Criteria

1. WHEN the SDD_Agent completes a phase document, THE SDD_Agent SHALL present the document and invite the user to review and provide feedback.
2. WHEN the user provides feedback on a phase document, THE SDD_Agent SHALL incorporate the feedback and present the updated document.
3. WHEN the user requests changes to a specific section, THE SDD_Agent SHALL modify only the affected section rather than regenerating the entire document.
4. THE SDD_Agent SHALL confirm all modifications with the user before proceeding to the next phase.
5. WHEN the user says "skip" or requests to move ahead, THE SDD_Agent SHALL proceed to the next phase without further iteration on the current phase.

### Requirement 6: Bugfix Workflow Support

**User Story:** As a developer, I want the SDD Agent to support a bugfix workflow, so that I can spec bug investigations and fixes with the same rigor as new features.

#### Acceptance Criteria

1. WHEN the user indicates they are working on a bugfix, THE SDD_Agent SHALL switch to the bugfix workflow.
2. WHEN in bugfix mode, THE SDD_Agent SHALL guide the user through: bug condition identification (what is happening vs. what should happen), root cause analysis, fix design, and fix verification tasks.
3. THE SDD_Agent SHALL generate a bug condition document that captures: observed behavior, expected behavior, reproduction steps, affected components, and root cause hypothesis.
4. THE SDD_Agent SHALL generate fix tasks that include verification criteria to confirm the bug is resolved and no regressions are introduced.

### Requirement 7: Session Management

**User Story:** As a developer, I want the SDD Agent to manage long sessions and support context restoration, so that I can continue my spec work across multiple sessions.

#### Acceptance Criteria

1. WHEN the session has covered substantial ground, THE SDD_Agent SHALL proactively offer to generate a Session_Summary.
2. THE SDD_Agent SHALL format the Session_Summary so the user can paste it into a new session to restore context.
3. THE SDD_Agent SHALL include in the Session_Summary: current phase, completed phases with key decisions, the latest version of each phase document, and open questions.
4. WHEN the user pastes a Session_Summary at the start of a new session, THE SDD_Agent SHALL parse the summary and resume from where the previous session ended.

### Requirement 8: Correctness Properties in Requirements

**User Story:** As a developer, I want the SDD Agent to suggest correctness properties for my acceptance criteria, so that I can write property-based tests that catch real bugs.

#### Acceptance Criteria

1. WHEN generating acceptance criteria, THE SDD_Agent SHALL identify opportunities for Correctness_Property patterns (invariants, round-trip, idempotence, metamorphic, model-based, confluence, error-conditions).
2. WHEN a requirement involves data transformation, THE SDD_Agent SHALL suggest an invariant property (e.g., collection size preserved after map).
3. WHEN a requirement involves serialization or parsing, THE SDD_Agent SHALL include a round-trip property as an acceptance criterion.
4. WHEN a requirement involves an operation that should have no additional effect when repeated, THE SDD_Agent SHALL suggest an idempotence property.

### Requirement 9: Core Prompt File

**User Story:** As a developer, I want a single self-contained prompt file that defines the SDD Agent, so that I can deploy it on any LLM platform.

#### Acceptance Criteria

1. THE SDD_Agent prompt SHALL be contained in a single file named `sdd-agent-prompt.md` in the `SDD/` folder.
2. THE SDD_Agent prompt SHALL use the same structural conventions as other agent prompts in the repository: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, and `<formatting>` sections.
3. THE SDD_Agent prompt SHALL be self-contained with no external dependencies — all methodology, templates, and interaction patterns defined inline.
4. THE SDD_Agent prompt SHALL include output templates for each phase document (requirements, design, tasks) so the LLM produces consistently structured output.

### Requirement 10: Platform Setup Guides

**User Story:** As a developer, I want platform-specific setup guides for Gemini, Kiro CLI, and Claude, so that I can deploy the SDD Agent on my preferred platform.

#### Acceptance Criteria

1. THE Platform_Guide for Gemini SHALL be a file named `platform-gemini.md` in the `SDD/` folder, covering Custom Instructions and Gems setup options.
2. THE Platform_Guide for Kiro CLI SHALL be a file named `platform-kiro.md` in the `SDD/` folder, covering steering file deployment.
3. THE Platform_Guide for Claude SHALL be a file named `platform-claude.md` in the `SDD/` folder, covering Projects and Direct System Message setup options.
4. Each Platform_Guide SHALL follow the standard template used by other agents in the repository: setup options, platform notes, limitations, and a disclaimer that the guide does not modify agent behavior.
5. Each Platform_Guide SHALL reference the core prompt file `sdd-agent-prompt.md` as the source of all agent logic.

### Requirement 11: README Integration

**User Story:** As a developer browsing the repository, I want the SDD Agent listed in the root README, so that I can discover it alongside the other agents.

#### Acceptance Criteria

1. WHEN the SDD Agent is complete, THE root README.md SHALL include an entry for the SDD Agent following the same format as other agent entries.
2. THE README entry SHALL include a description of the agent's purpose, a link to the core prompt file, and links to each Platform_Guide.

### Requirement 12: Off-Topic and Scope Handling

**User Story:** As a developer, I want the SDD Agent to stay focused on spec-driven development, so that conversations remain productive and on-track.

#### Acceptance Criteria

1. WHEN the user asks a question outside the SDD domain, THE SDD_Agent SHALL acknowledge the question warmly and redirect to the spec workflow.
2. WHEN the user provides input that is not related to feature or bugfix specification, THE SDD_Agent SHALL inform the user of the scope boundary and offer to help with spec-related work.
3. THE SDD_Agent SHALL handle scope boundaries without dismissing or ignoring the user's input.
