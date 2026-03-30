# MyAgents
A repository of Agents I have developed for my development and work. I developed these using Spec Driven Development.

## [Learning Agent](Learning/learning-agent-prompt.md)

A system prompt that turns an LLM into a DE/CTO teaching persona. Teaches any technical topic layer by layer with concept maps, knowledge checks, Socratic questioning, and adaptive pacing. Platform setup guides for [Gemini](Learning/platform-gemini.md), [Kiro CLI](Learning/platform-kiro.md), and [Claude](Learning/platform-claude.md).

## [Mentor Agent](Mentor/mentor-agent-prompt.md)

A system prompt that turns an LLM into a principal engineer mentoring companion for senior data engineers growing toward staff/principal level. Guides users through real work situations using Socratic questioning, reframes, and growth dimension tracking across six areas: technical leadership, system thinking, influence without authority, driving alignment, owning ambiguity, and organizational impact. Platform setup guides for [Gemini](Mentor/platform-gemini.md), [Kiro CLI](Mentor/platform-kiro.md), and [Claude](Mentor/platform-claude.md).

## [Memory Email Agent](Memories/memory-email-agent-prompt.md)

A system prompt that turns an LLM into a warm writing companion for parents capturing memories as heartfelt emails for their daughter. Guides the parent through a structured flow — describe a memory, answer optional follow-up questions, pick a subject line, and review an HTML email draft — with multiple-choice interactions at every step to keep things quick and easy. Platform setup guides for [Gemini](Memories/platform-gemini.md), [Kiro CLI](Memories/platform-kiro.md), and [Claude](Memories/platform-claude.md).

## [Mythology Scholar Agent](Mythology/mythology-scholar-prompt.md)

A system prompt that turns an LLM into a mythology scholar and interpretive guide. Analyzes mythology texts with scholarly rigor — covering origin, symbolism, archetypes, and narrative structure — then connects themes to personal situations and current real-world events, with cross-tradition comparative analysis across world mythological traditions. Platform setup guides for [Gemini](Mythology/platform-gemini.md), [Kiro CLI](Mythology/platform-kiro.md), and [Claude](Mythology/platform-claude.md).

## [Mythology Story Narrator Agent](MythologyStories/mythology-story-narrator-prompt.md)

A system prompt that turns an LLM into a RAG-backed sequential mythology story narrator. Walks through a corpus of mythology stories one at a time, delivering vivid narrations with backstory context for returning characters, real-world lessons drawn from each story's themes, and a continuation marker system for pacing and session resumption. Platform setup guides for [Gemini](MythologyStories/platform-gemini.md), [Kiro CLI](MythologyStories/platform-kiro.md), and [Claude](MythologyStories/platform-claude.md).

## [Design Agent](Design/design-agent-prompt.md)

A system prompt that turns an LLM into a domain-agnostic design companion. Guides users through structured design phases — Understand, Decompose, Detail, Validate, Document — using Socratic questioning, tradeoff analysis, and domain-adaptive design patterns across software, business process, product, hardware, and organizational domains. Accepts Requirements Agent output as input for seamless requirements-to-design handoff. Platform setup guides for [Gemini](Design/platform-gemini.md), [Kiro CLI](Design/platform-kiro.md), and [Claude](Design/platform-claude.md).

## [Requirements Agent](Requirements/requirements-agent-prompt.md)

A system prompt that turns an LLM into a requirements engineering companion. Guides users through structured requirements elicitation, organization, refinement, and document generation for any problem domain — software, process, product, mechanism, or organizational. Uses Socratic questioning, conversational state markers, and domain-adaptive elicitation. Platform setup guides for [Gemini](Requirements/platform-gemini.md), [Kiro CLI](Requirements/platform-kiro.md), and [Claude](Requirements/platform-claude.md).

## [Proposal Agent](Proposal/proposal-agent-prompt.md)

A system prompt that turns an LLM into a domain-agnostic proposal drafting companion. Guides users through six structured phases — Problem Framing, Context and Stakeholders, Solution Proposal, Alternatives Analysis, Impact and Risks, Document Generation — using Socratic questioning, conversational state markers, and domain-adaptive elicitation to produce polished proposal documents for any domain: product launches, process changes, budget requests, policy reforms, or organizational restructuring. Platform setup guides for [Gemini](Proposal/platform-gemini.md), [Kiro CLI](Proposal/platform-kiro.md), and [Claude](Proposal/platform-claude.md).

## [Google Flow Video Agent](Google-Flow/google-flow-video-agent-prompt.md)

A system prompt that turns an LLM into a collaborative creative director for producing 1–2 minute videos using Google Flow. Guides users through a seven-phase workflow — story intake, sequence breakdown, character and scene identification, capability selection, prompt generation, stitching guidance, and production guide compilation — with built-in visual consistency management across separately generated segments. Platform setup guides for [Gemini](Google-Flow/platform-gemini.md) and [Kiro CLI](Google-Flow/platform-kiro.md).

## [Perspective Agent](Perspective/perspective-agent-prompt.md)

A system prompt that turns an LLM into a cross-functional perspective analysis companion for senior and staff data engineers. Generates structured stakeholder perspectives from six personas — Data Scientist, Data Analyst, Engineering Manager, Product Manager, Director, and Peer Data Engineer — surfacing key concerns, likely questions, potential objections, and areas of support for any input: proposals, architecture decisions, articles, or presentation outlines. Includes synthesis with common themes, key disagreements, and actionable preparation areas, plus iterative refinement through deeper dives, context updates, and in-character Q&A. Platform setup guides for [Gemini](Perspective/platform-gemini.md), [Kiro CLI](Perspective/platform-kiro.md), and [Claude](Perspective/platform-claude.md).

## [PaperBuddy Agent](PaperBuddy/paper-buddy-agent-prompt.md)

A system prompt that turns an LLM into a research paper comprehension companion for senior data engineers. Accepts papers or technical articles in any form — full text, excerpts, titles with abstracts, or copied sections — and produces a structured 4-pillar breakdown covering the core problem, why it matters, how it solves it, and actionable takeaways for data engineering work. Includes inline jargon translation to data engineering concepts, honest strengths and limitations assessment, familiarity-adaptive depth, and follow-up conversation support for deep dives, application questions, and critical evaluation. Platform setup guides for [Gemini](PaperBuddy/platform-gemini.md), [Kiro CLI](PaperBuddy/platform-kiro.md), and [Claude](PaperBuddy/platform-claude.md).

## [Pipeline Documentation Agent](PipelineDocumentation/pipeline-analyst-agent-prompt.md)

A two-agent workflow that turns an LLM into a pipeline documentation specialist for SQL and PySpark code. The [Analyst Agent](PipelineDocumentation/pipeline-analyst-agent-prompt.md) ingests pipeline code, performs multi-pass analysis, and produces structured transformation documentation covering source tables, transformation steps, business rules, data lineage, output schemas, and edge cases. The [Reviewer Agent](PipelineDocumentation/pipeline-reviewer-agent-prompt.md) independently validates the documentation against the original code through an automated review loop with confidence scoring and convergence detection. Platform setup guide for [Kiro CLI](PipelineDocumentation/platform-kiro.md).
