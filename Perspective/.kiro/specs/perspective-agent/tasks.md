# Tasks

## Task 1: Create Core Prompt File
- [x] 1.1 Create `Perspective/perspective-agent-prompt.md` with `<identity>` section defining agent role, communication style, domain awareness, and off-topic handling
- [x] 1.2 Add persona definitions for all six default personas (Data Scientist, Data Analyst, Engineering Manager, Product Manager, Director, Peer Data Engineer) with role-specific focus areas, mental models, and vocabulary
- [x] 1.3 Add `<methodology>` section defining input processing flow: validation, acknowledgment/confirmation, perspective generation, synthesis
- [x] 1.4 Add output formatting rules specifying markdown structure: persona-named headers, four subsections (Key Concerns, Likely Questions, Potential Objections/Risks, Areas of Support/Enthusiasm), and synthesis section (Common Themes, Key Disagreements, Suggested Areas to Address)
- [x] 1.5 Add `<interaction_patterns>` section defining iterative refinement: deeper dives, context updates, in-character Q&A responses
- [x] 1.6 Add `<session_protocol>` section defining session initialization, greeting, and conversation flow management
- [x] 1.7 Add input validation rules: empty input rejection, ambiguous input clarification, accepted input forms
- [x] 1.8 Add edge case handling: off-topic redirection, sensitive content warning, non-professional topic boundary

## Task 2: Create Platform Deployment Files
- [x] 2.1 Create `Perspective/platform-claude.md` with Claude-specific setup instructions (Projects and Direct System Message options), platform notes, and limitations
- [x] 2.2 Create `Perspective/platform-gemini.md` with Gemini-specific setup instructions (Custom Instructions and Gems options), platform notes, and limitations
- [x] 2.3 Create `Perspective/platform-kiro.md` with Kiro CLI setup instructions (steering file deployment), platform notes, and limitations
- [x] 2.4 Verify each platform file contains only deployment instructions and no behavioral definitions

## Task 3: Validate Agent Structure
- [x] 3.1 Verify all four files exist in `Perspective/` folder: perspective-agent-prompt.md, platform-claude.md, platform-gemini.md, platform-kiro.md
- [x] 3.2 Verify core prompt contains all required sections: identity, persona definitions, methodology, output formatting, interaction patterns, session protocol
- [x] 3.3 Verify each persona definition includes all focus areas specified in Requirements 3.1–3.6
- [x] 3.4 Verify platform files follow workspace naming convention and contain no behavioral markers

