---
name: kickoff
description: Project orientation and kickoff. Use when the user starts a session and wants to get oriented, says "kickoff", "get started", "where were we", "what's the status", or needs to initialize a new project. This is the entry point for every working session.
---

# Kickoff

## Purpose
Orient the user at the start of a session. Determine whether this is an existing project or a new one, and take the right action.

## Process

### 1. Check for Project Foundation
Look for these files in the current working directory:
- `PRD.md`
- `architecture.md`
- `design.md`
- `TODO.md`
- `CHANGELOG.md`
- `CLAUDE.md` (project-level)

### 2A. Existing Project — Resume
If foundation files exist, read them and provide a brief orientation:

```markdown
## Project Status

**Project:** [Name/description from PRD.md]
**Last changes:** [Recent entries from CHANGELOG.md]
**Outstanding items:** [Key items from TODO.md]
**Current state:** [Brief summary of where things are]

### Available Commands
- `/designboss` — Design review (or ideation)
- `/productguy` — Product review (or ideation)
- `/CTO` — Technical review (or ideation)
- `/focusgroup` — UX research
- `/conferencecall` — Cross-functional team discussion
- `/simplify` — Code quality polish
- `/plan` — Plan next steps

All accept optional input (e.g., `/CTO auth layer`, `/conferencecall offline support`).

What would you like to work on?
```

### 2B. New Project — Discovery
If no foundation files exist, start the project kickoff protocol:

1. Acknowledge this is a new project
2. Show available commands briefly
3. Begin the discovery process — ask questions:
   - What are we building? (the vision)
   - Why does this need to exist? (the problem it solves)
   - Who is it for? (the audience)
   - What does success look like?
   - Any constraints or non-negotiables?

After discovery, proceed to:
- `git init`
- Create `PRD.md`, `architecture.md`, `design.md`, `TODO.md`, `CHANGELOG.md`, project `CLAUDE.md`
- First commit to checkpoint the foundation
- Use `/plan` to map the approach

### 2C. Partial Setup
If some files exist but not others, note what's missing and offer to create them.
