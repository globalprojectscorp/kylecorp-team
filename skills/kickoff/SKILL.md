---
name: kickoff
description: Project orientation and kickoff. Use when the user starts a session and wants to get oriented, says "kickoff", "get started", "where were we", "what's the status", or needs to initialize a new project. This is the entry point for every working session.
---

# Kickoff

## Purpose
Orient the user and get them moving. This is the single entry point — it figures out the situation and takes action.

## Process

### 1. Check for Project Foundation
Look for these files in the current working directory:
- `PRD.md`
- `architecture.md`
- `design.md`
- `TODO.md`
- `CHANGELOG.md`
- `CLAUDE.md` (project-level)

---

### 2A. Existing Project — Resume
If foundation files exist, read them all and provide a brief orientation:

```markdown
## [Project Name] — Status

**Last changes:** [Recent entries from CHANGELOG.md]
**Outstanding:** [Key items from TODO.md]
**Current state:** [Brief summary of where things are]

### Commands
`/designboss` `/productguy` `/CTO` `/focusgroup` `/conferencecall` `/simplify` `/plan`
All accept optional input (e.g., `/CTO auth layer`)

What would you like to work on?
```

---

### 2B. New Project — Drive the Full Kickoff
If no foundation files exist, **take charge and drive the process.** Don't show a menu — start the interview.

#### Step 1: Discovery Interview
Say something like: "Let's build the foundation. I'll ask you some questions to understand what we're making."

Ask these questions conversationally (not all at once — respond to their answers, ask follow-ups, dig deeper where it matters):

1. **What are we building?** — The vision. Paint me a picture.
2. **Why does this need to exist?** — The problem it solves. What's broken or missing today?
3. **Who is it for?** — The audience. Be specific.
4. **What does success look like?** — How do we know we nailed it?
5. **Any constraints?** — Platform, timeline, tech, non-negotiables?
6. **What should it feel like?** — The vibe, the aesthetic, the experience.

Listen actively. Ask follow-up questions when answers are vague or when you sense there's more to uncover. This is the most important part — getting the *why* right.

#### Step 2: Confirm Understanding
Before creating anything, summarize back what you heard:
"Here's what I'm hearing — [summary]. Does that capture it, or should we adjust anything?"

Wait for confirmation.

#### Step 3: Build the Foundation
Once confirmed, create everything:

1. `git init` (if not already initialized)
2. `PRD.md` — Product requirements from the discovery interview
3. `architecture.md` — High-level technical approach (propose based on what was discussed, note open questions)
4. `design.md` — Project-specific design decisions (inherit from `~/.claude/design.md`, extend based on the project's needs)
5. `TODO.md` — Empty, ready to capture things
6. `CHANGELOG.md` — First entry documenting project creation
7. Project-level `CLAUDE.md` — Any project-specific conventions

#### Step 4: First Commit
Commit everything as the foundation checkpoint.

#### Step 5: Plan
Transition into `/plan` to map out the implementation approach. Ask: "Foundation is set. Want to map out the implementation plan?"

---

### 2C. Partial Setup
If some files exist but not others, note what's missing and offer to fill the gaps through targeted questions. Don't recreate what already exists.
