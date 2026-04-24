# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Added
- Added `deep-think-prompt.md` as the core reasoning system template.
- Added `DataEngineeringInterviewer` agent suite with specific prompts for Data Modeling, Coding, and System Design interviews.
- Added `DocumentAnalyzer` (The Principal Distiller) to ruthlessly extract expected value, blast radius, and N-order realities from verbose documents.
- Added `PodcastDistiller` to process audio transcripts into high-fidelity structured briefs.
- Added `InteractiveVideoCreator` prompt for multi-story workflow processing.
- Added `Mythology/narration-prompt.md` to define mythological narration constraints.

### Changed
- Integrated Deep Think methodology (thought deconstruction) into `BlogEvaluator`, `Learning`, `PaperBuddy`, and `AudioNarrator` prompts.
- Updated `VideoCreator` workflow to streamline single-story inputs and establish a robust 5-phase or 4-phase state machine architecture.
- Updated `VideoCreator` prompt to allow optional split screens, mandate 3 distinct conceptual image prompts per segment (Start, Mid, End), and generate 2 transitional Google Veo prompts.
- Updated `AudioNarrator` prompt to explicitly clarify it receives strictly text transcriptions, not reference audio.
- Updated `NotebookLMArchitect` prompt to instruct NotebookLM to map philosophical insights to real-world scenarios across life stages (Childhood, Adulthood, Old Age).
- Fixed constraint conflicts in `MythologyStories` prompt to allow conversational English fallback tags, preventing silence on empty states.
- Refactored `MythologyStories` agent prompt to mandate Hindi narration (Devanagari script), heavily optimized for 1-1.5 minute video generation.
- Hardened the `BlogEvaluator` agent using Deep Think's E5/E6 feedback. Added "Data Model Alignment" structured XML queries, "Forensic Traceability" rubrics, and the "Architectural Whitewashing" detection filter.

### Added
- Added `AudioNarrator` agent prompt for raw, music-free voice narration generation from transcriptions.
- Added `VideoCreator` agent prompt to orchestrate a 4-phase state machine for Hindu mythological video production.
- Added `ArchitectureExtractor` for deep dive codebase history and architecture extraction.
- Added the `RepoUnderstander` agent to ingest and synthesize enterprise codebases into a high-fidelity architectural mental model.
- Created the `CHANGELOG.md` tracking system.
- Added `Changelog Agent`, an outcome-focused Release Manager agent prompt.
- Added a `.git/hooks/pre-commit` script to enforce changelog updates on every code change.
- Added the `BlogEvaluator` agent to ruthlessly fact-check Technical Blog drafts against their raw architecture source.

