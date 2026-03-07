# User Preferences

## Communication Style
- Explain reasoning and tradeoffs concisely — the "high order bits" only
- Plain English, purposeful, no over-explaining
- Calibrate to intermediate experience level: explain non-obvious things, skip fundamentals
- Brief progress updates during long tasks

## Git (Non-negotiable)
- Every project must have git initialized
- Commit constantly — after every meaningful change
- Always be able to revert; never lose work
- Git is the undo button for everything

## Workflow
- Explain briefly what I'll do, then do it
- Stay narrowly focused on what's asked — don't fix adjacent issues
- When I notice issues outside scope, add them to `TODO.md` for later
- Create new files freely when architecture calls for it
- Tests only when explicitly asked

## Handling Uncertainty
- Ask clarifying questions for high-leverage/high-stakes decisions
- Make intelligent inferences for low-stakes decisions and mention assumptions
- Questions are a tool for unlocking better solutions, not a sign of being stuck

## Mistakes
- Own it briefly, explain what went wrong, then fix

## Key Files (Check on Project Load)
- `TODO.md` — running list of issues and improvements found along the way
- `CHANGELOG.md` — date-based log of meaningful changes (what changed, why); update constantly

### The Three Perspectives
These files represent three disciplines working together, like a product team:

| File | Perspective | Owns |
|------|-------------|------|
| `PRD.md` | Product Manager | What, why, for whom, success criteria — the human/product level |
| `architecture.md` | CTO / Engineering | Technical what, why, how — system design, structure, key technical decisions |
| `design.md` | Design | Aesthetic and UX — visual language, interaction patterns, informed by PRD |

### File Inheritance Pattern
Some files have **user-level defaults** that project-level files inherit from and extend:

| User-level (defaults) | Project-level (specializes) |
|-----------------------|----------------------------|
| `~/.claude/CLAUDE.md` | `./CLAUDE.md` |
| `~/.claude/design.md` | `./design.md` |

**How it works:**
- User-level files contain your universal preferences (Apple HIG, Neue Haas Grotesk, etc.)
- Project-level files inherit those defaults and add project-specific decisions
- If no project-level file exists, user-level defaults apply
- Project-level can override user-level when needed

**PRD.md and architecture.md** are purely project artifacts — no user-level defaults.

Keep all three updated as the project evolves, especially after `/plan` sessions and key decisions.

### CHANGELOG.md Format
Organize by date, grouped by type of change:
```markdown
## YYYY-MM-DD

### Added
- New features

### Changed
- Modifications to existing functionality

### Fixed
- Bug fixes
```
Reference commits inline when useful: `(abc123)`. Add version tags when shipping releases.

## Code Style
- No rigid style rules — prioritize readability and functional clarity
- Highly functional, easy to parse, clear
- Right tool for the job over what's easy or economical
- Prefer native, purpose-built, high-performance solutions
- Native apps over "write once run anywhere"

## Commits
- Short, clear messages that capture what and why
- Easy to parse for rebuilding context later
- No excess verbosity

## Philosophy
- **The code is cheap, the attention is expensive**
- Decisions are carefully considered — don't undo thoughtful work
- Never lose context on *why* we made decisions
- Progress at both forest and trees level
- Git is the safety net — commit constantly so we can always revert
- **Insanely great, but ship it** — aim for quality and beauty, but don't let perfectionism paralyze progress. Real artists ship.

## Product Management (Project Kickoff)
When starting a new project, follow this protocol:

**1. Discovery** — Ask questions to understand:
- What are we building? (the vision)
- Why does this need to exist? (the problem it solves)
- Who is it for? (the audience)
- What does success look like?
- Any constraints or non-negotiables?

**2. Plan** — Use `/plan` to map out the approach

**3. Initialize the foundation:**
- `git init` (non-negotiable)
- Create `PRD.md` — capture the product vision from discovery
- Create `architecture.md` — high-level technical approach
- Create `design.md` — project-specific design decisions (starts with user defaults from `~/.claude/design.md`, extends as needed)
- Create `TODO.md` — empty, ready to capture things
- Create project-level `CLAUDE.md` — project-specific conventions (inherits from `~/.claude/CLAUDE.md`)

**4. First commit** — Checkpoint the foundation before any code

The three perspectives (Product, Engineering, Design) should inform each other throughout the project.

## Skills & Plugins (Enable on New Projects)

**LSP Plugins (code intelligence):**
- `swift-lsp` — SwiftUI/Apple development
- `typescript-lsp` — Front-end TypeScript/React

**Workflow Plugins:**
- `commit-commands` — Automated git commit/push/PR (supports constant-commits workflow)
- `pr-review-toolkit` — Quality-focused PR review agents
- `github` — GitHub integration for issues, PRs
- `figma` — Design file integration

**Built-in Skills:**
- `/kickoff` — Start here. Orients you on an existing project or starts a new one
- `/simplify` — Polish code for quality/efficiency after features
- `/batch` — Orchestrate large-scale changes in parallel

**Review Skills (Department Heads):**
- `/designboss` — Head of Design evaluates implementation against design.md
- `/productguy` — Head of Product evaluates progress against PRD.md
- `/CTO` — CTO evaluates architecture against architecture.md
- `/focusgroup` — UX researcher that probes for blind spots, then feeds findings into the department heads
- `/conferencecall` — Gets the whole team together to debate a topic, argue tradeoffs, and deliver balanced consensus

All skills accept optional input (e.g., `/designboss login screen`, `/CTO should we use SQLite?`, `/conferencecall how to handle onboarding`).

Each review provides a written assessment, recommendations, and offers to implement all or specific fixes.

## Also Load
- `~/.claude/design.md` — design principles and defaults
