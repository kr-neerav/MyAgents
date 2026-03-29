# Design Document: Google Flow Video Agent

## Overview

The Google Flow Video Agent is a prompt-based AI assistant that guides users through transforming stories into 1–2 minute videos using Google Flow. Since Google Flow generates video only a few seconds at a time, the Agent acts as a production planner and creative director — breaking stories into filmable sequences, maintaining visual consistency through character sheets and scene descriptions, selecting the right Google Flow capabilities per segment, writing optimized prompts, and providing stitching guidance.

The deliverables are:
1. A core prompt file (`google-flow-video-agent-prompt.md`) containing the Agent's identity, methodology, and interaction patterns
2. Three platform deployment guides (`platform-kiro.md`, `platform-claude.md`, `platform-gemini.md`)

The Agent is conversational and stateful within a session. It walks users through a structured workflow from story intake to final production guide output, adapting to user feedback at each stage.

## Architecture

The Agent is a single-prompt AI assistant — no backend services, APIs, or databases. The architecture is entirely defined by the prompt structure and the conversation flow it orchestrates.

```mermaid
flowchart TD
    A[User provides Story] --> B[Story Analysis & Duration Estimation]
    B --> C[Sequence Breakdown]
    C --> D[Character Sheets & Scene Descriptions]
    D --> E[Capability Recommendations per Segment]
    E --> F[Prompt Generation per Segment]
    F --> G[Stitching Guidance]
    G --> H[Production Guide Compilation]
    
    B -->|Too long/short| B1[Duration Adjustment Recommendations]
    B1 --> B
    C -->|User revision| C
    F -->|User reports issues| F1[Prompt Revision]
    F1 --> F
    H -->|Component changes| H1[Production Guide Update]
    H1 --> H
```

The architecture consists of three layers:

1. **Prompt Layer**: The core prompt file defining identity, methodology, domain knowledge, and output formats
2. **Conversation Flow Layer**: The structured phases the Agent guides users through, with feedback loops at each stage
3. **Platform Layer**: Thin deployment wrappers for each target platform (Kiro CLI, Claude, Gemini)

### Design Decisions

- **Single prompt, no tool use**: The Agent relies entirely on conversational guidance rather than API integrations. This maximizes portability across platforms and keeps the Agent accessible to non-technical users.
- **Structured phases with flexible navigation**: The workflow follows a logical production order (story → sequences → characters/scenes → prompts → stitching → guide), but users can revisit any phase at any time.
- **Platform-agnostic core**: The core prompt contains zero platform-specific references. All platform concerns live in separate deployment guides.

## Components and Interfaces

### Component 1: Core Prompt (`google-flow-video-agent-prompt.md`)

The core prompt is the single deliverable that defines the Agent's behavior. It contains the following sections:

#### Identity Section
- Agent name, role, and communication style
- Domain expertise declaration (video production, Google Flow capabilities, storytelling)
- Tone: collaborative creative director — knowledgeable but approachable

#### Methodology Section
Defines the structured workflow phases:

1. **Story Intake & Analysis**: Accept story, summarize narrative arc, estimate duration, recommend adjustments if outside 1–2 minute target
2. **Sequence Breakdown**: Decompose story into ordered sequences with timing, narrative purpose, and transition recommendations
3. **Character & Scene Identification**: Extract characters and locations, produce Character Sheets and Scene Descriptions formatted for Google Flow prompts
4. **Capability Selection**: Map each segment to Google Flow features (text-to-video, image-to-video, camera controls, style references, video extension)
5. **Prompt Generation**: Write detailed, optimized prompts for each segment incorporating character/scene consistency references
6. **Stitching Guidance**: Provide assembly order, transition types, audio recommendations, and tool suggestions
7. **Production Guide Compilation**: Compile all outputs into a single structured document with checklist format

#### Google Flow Knowledge Section
- Capabilities reference: text-to-video, image-to-video, video extension, camera controls, style references
- Known limitations and workarounds (text rendering, complex multi-character interactions, action consistency)
- Prompt optimization patterns for Google Flow
- Reference image best practices

#### Output Format Templates
- Sequence Breakdown table format
- Character Sheet template
- Scene Description template
- Segment Prompt template
- Production Guide structure with checklist format

#### Interaction Patterns
- How to handle user feedback and revision requests at each phase
- How to handle ambiguous or incomplete story input
- How to handle visual inconsistency reports
- Session summary and context restoration patterns

### Component 2: Platform Guide — Kiro CLI (`platform-kiro.md`)

- Deployment via steering file at `.kiro/steering/google-flow-video-agent.md`
- Terminal output considerations (markdown rendering, long document handling)
- Limitations: context window management for long production guides, session persistence

### Component 3: Platform Guide — Claude (`platform-claude.md`)

- Deployment via Claude Projects (persistent) or direct system message (quick start)
- Formatting support notes
- Limitations: context window for long sessions, session persistence

### Component 4: Platform Guide — Gemini (`platform-gemini.md`)

- Deployment via Custom Instructions or Gems
- Formatting support notes
- Limitations: custom instructions length limits, context window, session persistence

### Interfaces Between Components

The core prompt is the only component with behavioral logic. Platform guides are documentation-only and reference the core prompt file by name. There are no runtime interfaces — the "interface" is the user's conversation with the Agent.

## Data Models

Since this is a prompt-based agent with no persistent storage, "data models" here refer to the structured output formats the Agent produces during conversation.

### Sequence Breakdown
```
| # | Sequence Description | Duration (s) | Segments | Narrative Purpose | Transition |
|---|---------------------|--------------|----------|-------------------|------------|
| 1 | [description]       | [3-5s]       | [1-2]    | [purpose]         | [type]     |
```

### Character Sheet
```
**Character: [Name]**
- Appearance: [physical description]
- Clothing: [outfit details]
- Distinguishing Features: [unique visual markers]
- Props/Accessories: [relevant items]
- Appears in Sequences: [list]
- Appearance Changes: [if any, by sequence]
- Google Flow Prompt Reference: [condensed visual descriptor for prompt inclusion]
```

### Scene Description
```
**Scene: [Location Name]**
- Setting: [environment description]
- Lighting: [conditions]
- Color Palette: [dominant colors]
- Time of Day: [time]
- Weather: [conditions]
- Key Elements: [important visual objects]
- Used in Sequences: [list]
- Environmental Changes: [if any, by sequence]
- Google Flow Prompt Reference: [condensed visual descriptor for prompt inclusion]
```

### Segment Prompt
```
**Segment [#] — Sequence [#]**
- Google Flow Capability: [text-to-video | image-to-video | etc.]
- Camera: [angle, movement, controls]
- Reference Image: [yes/no, description if yes]
- Prompt: "[full optimized prompt text incorporating character/scene references]"
- Visual Consistency Notes: [specific references to Character Sheets and Scene Descriptions]
```

### Production Guide Structure
```
# Production Guide: [Story Title]

## Pre-Production
- [ ] Character Sheets (review and approve)
- [ ] Scene Descriptions (review and approve)
- [ ] Reference Images (generate or source)

## Production (Segment-by-Segment)
- [ ] Segment 1: [brief description] — Est. [X] min
  - Prompt: [...]
  - Capability: [...]
- [ ] Segment 2: ...

## Post-Production
- [ ] Stitch segments in order
- [ ] Apply transitions
- [ ] Add audio (narration/music/SFX)
- [ ] Final review and export

## Estimated Total Effort: [X] hours
```

## Correctness Properties

*A property is a characteristic or behavior that should hold true across all valid executions of a system — essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees.*

Since this is a prompt-based AI agent (not traditional software with deterministic functions), correctness properties focus on structural validation of the Agent's output formats and the deliverable files themselves. The Agent's conversational quality and subjective judgment (e.g., "is this a good summary?") are not amenable to property-based testing, but the structural completeness and consistency of its outputs are.

### Property 1: Sequence Breakdown Structural Completeness

*For any* Sequence Breakdown output produced by the Agent, every sequence entry SHALL contain a description, an estimated duration in seconds, a narrative purpose, and a transition type to the next sequence (except the last).

**Validates: Requirements 2.1, 2.4**

### Property 2: Sequence Duration Constraints

*For any* sequence in a Sequence Breakdown, the estimated duration SHALL fall within Google Flow's achievable clip length range, and any sequence whose narrative requires more time than a single segment allows SHALL be subdivided into multiple segments.

**Validates: Requirements 2.2, 2.3**

### Property 3: Character Sheet Completeness

*For any* character identified in a Sequence Breakdown, a Character Sheet SHALL exist containing physical appearance, clothing, distinguishing features, props/accessories, and a list of sequences in which the character appears. For any character appearing in multiple sequences, appearance changes between sequences SHALL be noted.

**Validates: Requirements 3.1, 3.2, 3.3**

### Property 4: Scene Description Completeness

*For any* distinct location identified in a Sequence Breakdown, a Scene Description SHALL exist containing setting details, lighting conditions, color palette, time of day, weather, and key environmental elements. For any location used in multiple sequences, those sequences SHALL be listed and environmental changes between them SHALL be noted.

**Validates: Requirements 4.1, 4.2, 4.3**

### Property 5: Capability Recommendation Completeness

*For any* segment in a Sequence Breakdown, a Google Flow capability recommendation SHALL exist drawn from the valid set (text-to-video, image-to-video, video extension, camera controls, style references), accompanied by a rationale. For any segment requiring camera movement, specific camera control parameters SHALL be included.

**Validates: Requirements 5.1, 5.2, 5.3**

### Property 6: Prompt Structural Completeness

*For any* segment in a Sequence Breakdown, a corresponding Prompt SHALL exist that includes scene setting, character actions, camera angle, movement direction, lighting, and style descriptors. Prompts SHALL be numbered sequentially and cross-referenced to their source sequence.

**Validates: Requirements 6.1, 6.2, 6.5**

### Property 7: Visual Consistency Across Prompts

*For any* two prompts that reference the same character or location, the visual descriptors for that shared element SHALL be identical. Each prompt SHALL incorporate the relevant Character Sheet and Scene Description details to maintain visual consistency.

**Validates: Requirements 6.3, 9.1, 9.2**

### Property 8: Production Guide Completeness

*For any* Production Guide output, it SHALL contain all required sections (Sequence Breakdown, Character Sheets, Scene Descriptions, Prompts, capability recommendations, stitching guidance) organized in chronological production order (pre-production, production, post-production), with estimated time-of-effort for each step and stitching guidance including segment order, transition types, and timing adjustments.

**Validates: Requirements 7.1, 8.1, 8.2, 8.5**

### Property 9: Core Prompt Platform Agnosticism

*For any* text in the core prompt file (`google-flow-video-agent-prompt.md`), it SHALL contain no references to specific deployment platforms (Kiro, Claude, Gemini, or any other platform names).

**Validates: Requirements 10.5**

## Error Handling

Since this is a prompt-based agent, "error handling" refers to how the Agent responds to problematic user inputs and edge cases during conversation.

### Story Input Issues
- **No clear narrative**: Agent asks clarifying questions about intended story arc, characters, and key moments (Req 1.4)
- **Story too long (>2 min)**: Agent recommends specific cuts or condensation with rationale (Req 1.2)
- **Story too short (<1 min)**: Agent suggests narrative expansions, additional visual beats, or pacing adjustments (Req 1.3)
- **Ambiguous characters/locations**: Agent asks the user to clarify before generating Character Sheets or Scene Descriptions

### Generation Issues
- **Segment doesn't match expectations**: Agent suggests specific prompt revisions addressing the user's identified issues (Req 6.4)
- **Visual inconsistencies between segments**: Agent diagnoses likely cause (prompt wording, missing reference image, style drift) and suggests corrections (Req 9.4)
- **Pacing/continuity issues in stitched video**: Agent suggests segment re-generations, trim adjustments, or transition changes (Req 7.3)

### Revision Handling
- **User requests changes to any component**: Agent revises the specific component and cascades updates to all dependent outputs (Req 2.5, 8.4)
- **User provides reference images or visual preferences mid-session**: Agent incorporates into relevant Character Sheets or Scene Descriptions (Req 3.5, 4.5)

### Google Flow Limitations
- **Known capability limitations**: Agent proactively notes limitations (text rendering, complex multi-character interactions) and suggests workarounds (Req 5.5)
- **Segment exceeds clip length**: Agent automatically subdivides into multiple segments with visual continuity guidance (Req 2.3)

## Testing Strategy

### Dual Testing Approach

Testing for a prompt-based agent differs from traditional software testing. The "code" is the prompt itself, and the "outputs" are conversational responses. Testing focuses on:

1. **Structural validation tests (unit/example tests)**: Verify that output templates and deliverable files meet structural requirements
2. **Property-based tests**: Verify universal properties across generated outputs using randomized inputs

### Unit / Example Tests

- **Deliverable existence**: Verify that all four deliverable files exist (`google-flow-video-agent-prompt.md`, `platform-kiro.md`, `platform-claude.md`, `platform-gemini.md`) — validates Req 10.1–10.4
- **Platform guide structure**: Each platform guide contains setup instructions and a limitations section — validates Req 10.6
- **Checklist format**: Production Guide output contains checklist markers (`- [ ]`) — validates Req 8.3
- **Tool recommendations**: Stitching guidance includes video editing tool recommendations — validates Req 7.2
- **Audio guidance**: Stitching guidance includes audio element recommendations — validates Req 7.4
- **Technical specs**: Output includes recommended format, resolution, and aspect ratio — validates Req 7.5
- **Style recommendations**: Output includes art style, color grading, and aspect ratio recommendations — validates Req 9.3
- **Reference image recommendations**: Output recommends generating reference images for major characters and scenes — validates Req 9.5

### Property-Based Tests

Property-based testing library: **fast-check** (JavaScript/TypeScript) or **Hypothesis** (Python), depending on the test harness chosen for validating prompt outputs.

Each property test should run a minimum of 100 iterations with randomized inputs.

- **Property 1 test**: Generate random sequence breakdowns and validate each entry contains description, duration, narrative purpose, and transition type.
  Tag: `Feature: google-flow-video-agent, Property 1: Sequence Breakdown Structural Completeness`

- **Property 2 test**: Generate random sequences with varying durations and validate all fall within Google Flow limits, with subdivision for longer sequences.
  Tag: `Feature: google-flow-video-agent, Property 2: Sequence Duration Constraints`

- **Property 3 test**: Generate random character sets across random sequences and validate each has a complete Character Sheet with all required fields.
  Tag: `Feature: google-flow-video-agent, Property 3: Character Sheet Completeness`

- **Property 4 test**: Generate random location sets across random sequences and validate each has a complete Scene Description with all required fields.
  Tag: `Feature: google-flow-video-agent, Property 4: Scene Description Completeness`

- **Property 5 test**: Generate random segments and validate each has a capability recommendation from the valid set with rationale, plus camera parameters when movement is specified.
  Tag: `Feature: google-flow-video-agent, Property 5: Capability Recommendation Completeness`

- **Property 6 test**: Generate random segment sets and validate each has a corresponding prompt with all required fields, sequential numbering, and sequence cross-references.
  Tag: `Feature: google-flow-video-agent, Property 6: Prompt Structural Completeness`

- **Property 7 test**: Generate random prompt sets with shared characters/locations and validate that visual descriptors for shared elements are identical across prompts.
  Tag: `Feature: google-flow-video-agent, Property 7: Visual Consistency Across Prompts`

- **Property 8 test**: Generate random production guides and validate they contain all required sections in chronological order with time estimates and stitching guidance.
  Tag: `Feature: google-flow-video-agent, Property 8: Production Guide Completeness`

- **Property 9 test**: Scan the core prompt file text for any platform-specific references and validate none exist.
  Tag: `Feature: google-flow-video-agent, Property 9: Core Prompt Platform Agnosticism`

### Testing Notes

Given that this is a prompt-based agent, many acceptance criteria (especially around conversational quality, subjective judgment, and interactive behavior) are best validated through manual testing and user feedback rather than automated tests. The automated tests above focus on the structural and consistency properties that can be verified programmatically.
