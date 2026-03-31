# Tasks

## Task 1: Create the core system prompt file

Create `SDD/sdd-agent-prompt.md` with the complete SDD Agent prompt following repository conventions.

- [x] 1.1 Create the `<identity>` section defining the SDD Agent role, communication style, SDD domain focus, and off-topic handling
- [x] 1.2 Create the `<methodology>` section with the Requirements phase: EARS pattern definitions (Ubiquitous, Event-driven, State-driven, Unwanted-event, Optional-feature, Complex), INCOSE quality rules (no vague terms, passive voice, pronouns, escape clauses, negative statements), user story format, acceptance criteria format, glossary generation, and parser/serializer round-trip requirement trigger
- [x] 1.3 Create the `<methodology>` section with the Design phase: design document generation (architecture, components, interfaces, data models, decisions, tradeoffs), requirement traceability, Socratic questioning for design decisions, direct recommendation handling, and tradeoff surfacing
- [x] 1.4 Create the `<methodology>` section with the Tasks phase: task document generation (ordered numbered tasks, sub-tasks, acceptance criteria, dependency ordering, traceability to design components and requirements)
- [x] 1.5 Create the `<methodology>` section with the Bugfix workflow: bug condition identification, root cause analysis, fix design, fix verification tasks, bug condition document template (observed behavior, expected behavior, reproduction steps, affected components, root cause hypothesis)
- [x] 1.6 Create the `<methodology>` section with Correctness Properties: pattern definitions (invariant, round-trip, idempotence, metamorphic, model-based, confluence, error-condition), trigger conditions for each pattern, and instructions to suggest properties during acceptance criteria generation
- [x] 1.7 Create the `<methodology>` section with output templates for each phase document (requirements, design, tasks, bug condition, session summary)
- [x] 1.8 Create the `<session_protocol>` section: session initialization (greeting, feature/bugfix intake), phase progression rules, phase navigation (forward, backward, skip), iterative refinement (present → review → incorporate feedback → confirm), session summary generation (current phase, completed phases, latest documents, open questions, restoration instructions), context window management, and session summary restoration on paste
- [x] 1.9 Create the `<interaction_patterns>` section: Socratic questioning, vague input handling, iterative refinement patterns, skip-ahead handling, direct recommendation handling, scope boundary handling (warm acknowledgment + redirect)
- [x] 1.10 Create the `<formatting>` section: conversational state markers ([REQUIREMENTS], [DESIGN], [TASKS], [SESSION SUMMARY], [BUG CONDITION]), markdown rules, terminal compatibility

**Acceptance Criteria:**
- File exists at `SDD/sdd-agent-prompt.md`
- Contains all five section tags: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
- All EARS patterns defined inline
- All correctness property patterns defined inline
- Output templates for requirements, design, tasks, bug condition, and session summary included
- No external dependencies or file references required at runtime
- Traces to: Requirements 1, 2, 3, 4, 5, 6, 7, 8, 9, 12

## Task 2: Create the Gemini platform guide

Create `SDD/platform-gemini.md` following the standard platform guide template.

- [x] 2.1 Write setup instructions for Custom Instructions (quick start) and Gems (dedicated agent) options
- [x] 2.2 Write platform notes covering formatting support and Gemini-specific behavior
- [x] 2.3 Write limitations section (context window, custom instructions length limit, session persistence)
- [x] 2.4 Write disclaimer that the guide does not modify agent behavior and references `sdd-agent-prompt.md` as the source of all agent logic

**Acceptance Criteria:**
- File exists at `SDD/platform-gemini.md`
- Contains setup options, platform notes, limitations, and disclaimer sections
- References `sdd-agent-prompt.md` as the source of agent logic
- Follows the same structure as `StatusUpdates/platform-gemini.md` and other existing Gemini guides
- Traces to: Requirements 10.1, 10.4, 10.5

## Task 3: Create the Kiro CLI platform guide

Create `SDD/platform-kiro.md` following the standard platform guide template.

- [x] 3.1 Write setup instructions for steering file deployment
- [x] 3.2 Write platform notes covering terminal-based output considerations
- [x] 3.3 Write limitations section (context window, session persistence, session summaries)
- [x] 3.4 Write disclaimer that the guide does not modify agent behavior and references `sdd-agent-prompt.md` as the source of all agent logic

**Acceptance Criteria:**
- File exists at `SDD/platform-kiro.md`
- Contains setup instructions, platform notes, limitations, and disclaimer sections
- References `sdd-agent-prompt.md` as the source of agent logic
- Follows the same structure as `StatusUpdates/platform-kiro.md` and other existing Kiro CLI guides
- Traces to: Requirements 10.2, 10.4, 10.5

## Task 4: Create the Claude platform guide

Create `SDD/platform-claude.md` following the standard platform guide template.

- [x] 4.1 Write setup instructions for Projects (persistent agent) and Direct System Message (quick start) options
- [x] 4.2 Write platform notes covering instruction-following and formatting support
- [x] 4.3 Write limitations section (context window, session persistence, direct system message approach)
- [x] 4.4 Write disclaimer that the guide does not modify agent behavior and references `sdd-agent-prompt.md` as the source of all agent logic

**Acceptance Criteria:**
- File exists at `SDD/platform-claude.md`
- Contains setup options, platform notes, limitations, and disclaimer sections
- References `sdd-agent-prompt.md` as the source of agent logic
- Follows the same structure as `StatusUpdates/platform-claude.md` and other existing Claude guides
- Traces to: Requirements 10.3, 10.4, 10.5

## Task 5: Update the root README

Add an SDD Agent entry to `README.md` following the established format.

- [x] 5.1 Add an SDD Agent section to `README.md` with a description of the agent's purpose, a link to `SDD/sdd-agent-prompt.md`, and links to each platform guide (`SDD/platform-gemini.md`, `SDD/platform-kiro.md`, `SDD/platform-claude.md`)

**Acceptance Criteria:**
- README contains an SDD Agent entry
- Entry includes description, link to core prompt, and links to all three platform guides
- Entry follows the same format as other agent entries in the README
- Traces to: Requirements 11.1, 11.2

## Task 6: Manual validation

Manually test the core prompt on at least one platform to verify conversational behavior.

- [ ] 6.1 Deploy the prompt on one platform and test: session greeting, requirements generation from a sample feature description, phase transition to design, phase transition to tasks, session summary generation, and session summary restoration
- [ ] 6.2 Test the bugfix workflow: bugfix detection, bug condition document generation, fix task generation with verification criteria
- [ ] 6.3 Test edge cases: off-topic handling, skip-ahead, backward navigation, vague input handling

**Acceptance Criteria:**
- Agent greets user and asks about feature/bugfix
- Agent generates requirements with EARS patterns and user stories
- Agent transitions between phases on user confirmation
- Agent generates session summaries that restore context
- Agent handles bugfix workflow
- Agent redirects off-topic questions warmly
- Traces to: Requirements 1, 2, 3, 4, 5, 6, 7, 8, 12
