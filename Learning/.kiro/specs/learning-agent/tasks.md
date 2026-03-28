# Implementation Plan: Learning Agent

## Overview

Build a system prompt that transforms an LLM into a Distinguished Engineer / CTO teaching persona, with platform-specific wrappers for Gemini chat, Kiro CLI, and Claude. The core artifact is a markdown-formatted system prompt file. Tests are written in Python to validate prompt structure and correctness properties.

## Tasks

- [x] 1. Create the core system prompt file
  - [x] 1.1 Create the Identity & Persona section
    - Create `Learning/learning-agent-prompt.md` with the `<identity>` section
    - Define the Distinguished Engineer / CTO persona with deep technical expertise and systems-thinking ability
    - Include communication style guidelines: clear, precise technical language adapted to user's current layer
    - Include real-world analogies, war stories, and practical example instructions
    - Include off-topic handling: briefly acknowledge, relate back to current context or suggest revisiting at appropriate layer
    - _Requirements: 1.1, 1.2, 1.3, 1.4_

  - [x] 1.2 Create the Core Methodology section
    - Add the `<methodology>` section to the core prompt
    - Include layer decomposition instructions: decompose any topic into ordered layers of increasing complexity
    - Include concept map generation rules: present a Concept_Map at session start showing planned layers and relationships
    - Include knowledge check protocol: perform a Knowledge_Check at end of each layer before advancing
    - Include re-explanation instructions: if user fails knowledge check, re-explain with alternative examples before proceeding
    - Include skip-ahead handling: warn about knowledge gaps and provide brief summary of skipped layers
    - Include adaptive pacing rules: accelerate for prior knowledge, slow down for confusion/frustration
    - _Requirements: 2.1, 2.2, 2.3, 2.4, 2.5, 2.6, 3.1, 3.2, 3.4_

  - [x] 1.3 Create the Session Protocol section
    - Add the `<session_protocol>` section to the core prompt
    - Include session initialization flow: greet user, ask topic, assess current familiarity
    - Include layer progression rules: teach one layer at a time with explanation, examples, and context
    - Include progress tracking: track completed layers within session, reference prior layers when introducing new concepts
    - Include summary generation rules: key takeaways at end of each layer, full session recap on request
    - _Requirements: 5.1, 5.2, 5.3, 2.3, 3.3_

  - [x] 1.4 Create the Interaction Patterns section
    - Add the `<interaction_patterns>` section to the core prompt
    - Include Socratic questioning: ask user to reason through problems before providing answers, minimum one per layer
    - Include guided correction: for incorrect answers, guide through follow-up questions rather than giving the answer directly
    - Include exercise generation rules: at least one practical exercise per layer, scaled to layer complexity
    - Include exercise feedback instructions: review user's approach and provide constructive feedback on completion
    - Include supplementary exercise generation: generate additional exercises when user requests more practice
    - _Requirements: 7.1, 7.2, 7.3, 6.1, 6.2, 6.3, 6.4_

  - [x] 1.5 Create the Formatting section
    - Add the `<formatting>` section to the core prompt
    - Include numbered layers convention
    - Include clear section breaks
    - Include code block usage rules
    - Include conversational state markers: `[LAYER X/N]`, `[CONCEPT MAP]`, `[KNOWLEDGE CHECK]`, `[EXERCISE]`, `[SUMMARY]`, `[PROFICIENCY]`
    - Ensure all formatting rules are platform-agnostic
    - _Requirements: 5.4_

- [x] 2. Checkpoint - Verify core prompt completeness
  - Ensure all tests pass, ask the user if questions arise.

- [x] 3. Create platform-specific wrappers
  - [x] 3.1 Create the Gemini chat wrapper
    - Create `Learning/platform-gemini.md` with step-by-step setup instructions
    - Include deployment via "Custom Instructions" or "Gems" feature
    - Include platform-specific notes about formatting support
    - Reference the core prompt file without modifying teaching behavior
    - _Requirements: 4.1, 4.2, 4.5_

  - [x] 3.2 Create the Kiro CLI wrapper
    - Create `Learning/platform-kiro.md` with step-by-step setup instructions
    - Include deployment via steering file (`.kiro/steering/learning-agent.md`)
    - Include terminal-based output notes: avoid complex visual formatting
    - Reference the core prompt file without modifying teaching behavior
    - _Requirements: 4.1, 4.3, 4.5_

  - [x] 3.3 Create the Claude wrapper
    - Create `Learning/platform-claude.md` with step-by-step setup instructions
    - Include deployment via "Projects" system prompt or direct system message
    - Include notes about Claude's strong instruction-following and full markdown support
    - Reference the core prompt file without modifying teaching behavior
    - _Requirements: 4.1, 4.4, 4.5_

- [ ] 4. Set up Python test framework and structural validation
  - [x] 4.1 Set up test infrastructure
    - Create `Learning/tests/conftest.py` with a pytest fixture that loads the core prompt file
    - Create `Learning/tests/requirements.txt` with `pytest` and `hypothesis` dependencies
    - Ensure the fixture provides the full prompt text to all tests
    - _Requirements: N/A (infrastructure)_

  - [ ]* 4.2 Write unit tests for prompt structure
    - Create `Learning/tests/test_prompt_structure.py`
    - Verify the core prompt contains all required sections: `<identity>`, `<methodology>`, `<session_protocol>`, `<interaction_patterns>`, `<formatting>`
    - Verify each platform wrapper contains step-by-step setup instructions
    - Verify session initialization instructions include greeting, topic inquiry, and assessment
    - Verify concept map generation instructions exist
    - Verify session summary instructions include layers covered, concepts learned, and next steps
    - Verify Socratic questioning instructions with "reason before answer" pattern exist
    - _Requirements: 1.1, 2.2, 5.1, 5.3, 7.1_

  - [ ]* 4.3 Write property test: Topic decomposition produces ordered layers
    - **Property 1: Topic decomposition produces ordered layers**
    - Create property test in `Learning/tests/test_properties.py`
    - Generate random topic strings, verify the prompt contains layer-ordering directives and concept map format specifications
    - **Validates: Requirements 2.1**

  - [ ]* 4.4 Write property test: Knowledge check gates layer advancement
    - **Property 2: Knowledge check gates layer advancement**
    - For any generated layer number N, verify the prompt contains instructions requiring a knowledge check before transitioning to N+1, and contains re-explanation instructions for failed checks
    - **Validates: Requirements 2.4, 2.5**

  - [ ]* 4.5 Write property test: Skip requests produce warnings and summaries
    - **Property 3: Skip requests produce warnings and summaries**
    - For any generated subset of layers to skip, verify the prompt contains skip-handling instructions that include both warning and summary components
    - **Validates: Requirements 2.6**

  - [ ]* 4.6 Write property test: Adaptive pacing responds to user signals
    - **Property 4: Adaptive pacing responds to user signals**
    - For any generated user signal (prior knowledge or confusion), verify the prompt contains the corresponding adaptive instruction (accelerate or slow down)
    - **Validates: Requirements 3.1, 3.4**

- [x] 5. Checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

- [ ] 6. Property tests for platform and content requirements
  - [ ]* 6.1 Write property test: Single core prompt with minimal platform wrappers
    - **Property 5: Single core prompt with minimal platform wrappers**
    - For each supported platform {Gemini, Kiro CLI, Claude}, verify the core prompt is shared and platform wrappers only add deployment instructions without modifying teaching behavior
    - **Validates: Requirements 4.1, 4.5**

  - [ ]* 6.2 Write property test: Layer completion produces a summary
    - **Property 6: Layer completion produces a summary**
    - For any generated layer number, verify the prompt contains instructions to produce a summary of key takeaways upon layer completion
    - **Validates: Requirements 5.2**

  - [ ]* 6.3 Write property test: Consistent formatting conventions in prompt
    - **Property 7: Consistent formatting conventions in prompt**
    - For any generated output section type (concept map, knowledge check, exercise, summary), verify the prompt contains platform-agnostic formatting instructions
    - **Validates: Requirements 5.4**

  - [ ]* 6.4 Write property test: Minimum per-layer content requirements
    - **Property 8: Minimum per-layer content requirements**
    - For any generated layer, verify the prompt requires at least one exercise and at least one Socratic question per layer
    - **Validates: Requirements 6.1, 7.3**

  - [ ]* 6.5 Write property test: Exercise feedback on completion
    - **Property 9: Exercise feedback on completion**
    - For any generated exercise completion event, verify the prompt contains feedback instructions
    - **Validates: Requirements 6.2**

  - [ ]* 6.6 Write property test: Supplementary exercises on request
    - **Property 10: Supplementary exercises on request**
    - For any generated practice request, verify the prompt contains instructions to generate additional exercises
    - **Validates: Requirements 6.4**

  - [ ]* 6.7 Write property test: Guided correction over direct answers
    - **Property 11: Guided correction over direct answers**
    - For any generated incorrect answer scenario, verify the prompt instructs guided follow-up questions rather than direct answer provision
    - **Validates: Requirements 7.2**

- [x] 7. Final checkpoint - Ensure all tests pass
  - Ensure all tests pass, ask the user if questions arise.

## Notes

- Tasks marked with `*` are optional and can be skipped for faster MVP
- The core deliverable is the system prompt file (`Learning/learning-agent-prompt.md`) and three platform wrappers
- All test code uses Python with `pytest` and `hypothesis`
- Property tests validate structural properties of the prompt text, not runtime LLM behavior
- Each task references specific requirements for traceability
- Checkpoints ensure incremental validation
