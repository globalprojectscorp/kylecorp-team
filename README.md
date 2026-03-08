# kylecorp-team

A leadership team for AI-assisted software development, built on [Claude Code](https://claude.com/claude-code).

## What This Is

Most people use AI by giving it a task and getting a result back. Write this function. Fix this bug. One prompt, one response, one perspective.

This system works differently. Instead of a single AI that does whatever you ask, it's structured as a **leadership team** — five department heads who each own a specific discipline, have their own priorities, and can disagree with each other.

The **Head of Design** cares about whether it looks and feels right. The **Head of Product** cares about whether we're building the right thing. The **CTO** cares about whether it's built well. The **CMO** cares about whether anyone will ever find it. **Legal/Finance** shows up when you need them and stays quiet when you don't.

Each head maintains a **source-of-truth document** they review before speaking — product requirements, architecture decisions, design principles. They don't start from scratch every conversation. They read what the team already decided and evaluate against it. This gives the organization memory.

Each head also carries **nested specializations** that only activate when relevant. The CTO can go deep on security, DevOps, or performance. The Design head can shift into accessibility auditing, brand consistency, or spatial design for Vision Pro. The CMO knows platform-specific tactics for TikTok, Reddit, or the App Store. The system stays lean by default but goes deep on demand.

The shift is subtle but fundamental: instead of managing tasks, you're **managing a team**. The AI isn't waiting for instructions — it's telling you what you need to hear from five different angles. It's the difference between having a tool and having a team.

---

## The Team

### Department Heads

| Skill | Role | Owns | Specializations |
|-------|------|------|-----------------|
| `/designboss` | Head of Design | `design.md` | `a11y`, `brand`, `whimsy`, `spatial` |
| `/productguy` | Head of Product | `PRD.md` | `priorities`, `trends`, `feedback`, `experiment` |
| `/CTO` | CTO / Engineering | `architecture.md` | `security`, `devops`, `performance`, `spatial` |
| `/CMO` | Head of Marketing | `marketing.md` | `aso`, `platforms` (5 channels), `image-prompts` |
| `/suits` | Legal / Finance / Ops | — | `compliance`, `finance`, `analytics`, `exec-summary` |

### Research & Coordination

| Skill | Purpose |
|-------|---------|
| `/focusgroup` | UX researcher — thinks like users, finds friction, routes findings to department heads |
| `/conferencecall` | Cross-functional debate — any combination of heads argue tradeoffs and deliver consensus |
| `/allhands` | Quick standup — every head gives 2-3 sentence status + flags (no deep debate) |
| `/kickoff` | Project orientation — initializes foundation docs and gets the team started |

---

## How You Work With Them

### Bring in one head to work a problem

```
/CTO security          → Deep security audit with STRIDE threat modeling
/designboss whimsy     → Evaluate personality and delight (includes microcopy library)
/productguy priorities → What should we build next, ranked by user value
/CMO aso               → App Store Optimization deep dive
/suits compliance      → GDPR/CCPA regulatory risk assessment
```

Each head stays in character — grounded in their discipline, opinionated, specific.

### Conference call — cross-functional debate

```
/conferencecall                          → Default trio: Design, Product, CTO
/conferencecall should we add offline    → Focused topic with default trio
/conferencecall CMO designboss           → Just those two
/conferencecall all                      → Everyone including /suits
```

The CTO argues it's a six-week architectural lift. Product says the target user needs it. Design worries about the sync conflict UX. They surface real tensions, argue honestly, and deliver a recommendation that acknowledges what's being sacrificed.

### All-hands standup

```
/allhands              → Quick pulse from Design, Product, CTO, CMO
/allhands [topic]      → Each head's quick take on a specific topic
```

Brief 2-3 sentence updates per head, plus cross-cutting flags. If something catches your eye, ask them to expand or pull a subset into `/conferencecall`.

### Focus group — the user's voice

```
/focusgroup                    → Holistic UX evaluation
/focusgroup onboarding flow    → Focused on a specific area
```

Thinks like your users, asks probing questions, then routes findings to the relevant department heads.

### Suits — when you need them

```
/suits compliance      → GDPR/CCPA/HIPAA regulatory audit
/suits finance         → NPV, ROI, cash flow analysis
/suits analytics       → Customer segmentation, attribution modeling
/suits exec-summary    → Consultant-grade 325-475 word executive briefing
```

Deliberately excluded from `/allhands` and `/conferencecall` unless explicitly invited. Just like in a real company, you don't want legal in every meeting — it changes the conversation.

---

## Architecture

### Source-of-Truth Documents

Each project maintains documents that the team reads before speaking:

| Document | Owner | Purpose |
|----------|-------|---------|
| `PRD.md` | Product | What, why, for whom, success criteria |
| `architecture.md` | CTO | System design, structure, key technical decisions |
| `design.md` | Design | Visual language, interaction patterns, HIG compliance |
| `marketing.md` | CMO | Positioning, channels, growth, go-to-market |
| `TODO.md` | Shared | Running list of issues found along the way |
| `CHANGELOG.md` | Shared | Date-based log of what changed and why |

### Progressive Context Loading

Reference files only enter the AI's context window when relevant:

```
skills/
├── CTO/
│   ├── SKILL.md              ← Core skill (~5KB, loads on invocation)
│   └── references/
│       ├── security.md        ← STRIDE, CI/CD pipeline, secure code patterns
│       ├── devops.md          ← Deployment, observability, reliability
│       ├── performance.md     ← Rendering, network, startup optimization
│       └── spatial.md         ← visionOS, RealityKit, ECS architecture
├── designboss/
│   ├── SKILL.md
│   └── references/
│       ├── a11y.md            ← Screen reader protocol, keyboard checklist, WCAG
│       ├── brand.md           ← Visual identity coherence, drift detection
│       ├── whimsy.md          ← Microcopy library, CSS animations, easter eggs
│       └── spatial.md         ← Spatial HIG, comfort, typography in 3D space
├── CMO/
│   ├── SKILL.md
│   └── references/
│       ├── aso.md             ← App Store keyword strategy, screenshot sequence
│       ├── platforms.md       ← Twitter, TikTok, Instagram, Reddit, LinkedIn tactics
│       └── image-prompts.md   ← 5-layer AI image generation framework
├── suits/
│   ├── SKILL.md
│   └── references/
│       ├── gdpr-privacy.md    ← GDPR/CCPA framework, privacy policy, contract review
│       ├── finance.md         ← NPV/IRR/payback, budget variance, cash flow
│       └── analytics.md      ← RFM segmentation, attribution modeling, dashboards
├── productguy/
│   └── SKILL.md               ← Modes built in (priorities, trends, feedback, experiment)
├── allhands/
│   └── SKILL.md
├── conferencecall/
│   └── SKILL.md
├── focusgroup/
│   └── SKILL.md
└── kickoff/
    └── SKILL.md
```

Heads auto-detect when to load specializations (CTO sees RealityKit imports → loads spatial reference) but you can also force a lens explicitly (`/CTO security`).

### File Inheritance

User-level defaults that project-level files inherit from and extend:

| User-level (defaults) | Project-level (specializes) |
|-----------------------|----------------------------|
| `~/.claude/CLAUDE.md` | `./CLAUDE.md` |
| `~/.claude/design.md` | `./design.md` |

User-level files contain universal preferences. Project-level files add project-specific decisions and can override when needed.

---

## Setup

### Fresh machine

```bash
gh repo clone globalprojectscorp/kylecorp-team ~/.claude
```

### What's included

- `CLAUDE.md` — Global instructions and workflow preferences
- `design.md` — Default design system (Apple HIG, Neue Haas Grotesk)
- `skills/` — All department head skills and reference files
- `Fonts/` — Neue Haas Grotesk Rounded (Display + Text)
- `settings.json` — Claude Code settings
- `keybindings.json` — Custom keybindings

### What's excluded (`.gitignore`)

Cache, plugins, project-specific state, telemetry, debug logs — anything that's machine-local or regenerated automatically.

---

## Lineage

Inspired by [agency-agents](https://github.com/msitarzewski/agency-agents) (61 standalone AI agent personas). This system absorbs ~35 of those agents' capabilities but takes a different architectural approach: fewer heads with depth via progressive-loading reference files, organized like a real leadership team rather than a roster of specialists.
