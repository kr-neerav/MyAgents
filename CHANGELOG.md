# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [Unreleased]

### Changed
- Hardened the `BlogEvaluator` agent using Deep Think's E5/E6 feedback. Added "Data Model Alignment" structured XML queries, "Forensic Traceability" rubrics, and the "Architectural Whitewashing" detection filter.

### Added
- Created the `CHANGELOG.md` tracking system.
- Added `Changelog Agent`, an outcome-focused Release Manager agent prompt.
- Added a `.git/hooks/pre-commit` script to enforce changelog updates on every code change.

### Added
- Added the `BlogEvaluator` agent to ruthlessly fact-check Technical Blog drafts against their raw architecture source.
