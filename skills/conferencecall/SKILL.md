---
name: conferencecall
description: Cross-functional team meeting that gets department heads together to debate a topic. Defaults to designboss, productguy, and CTO, but supports any combination (e.g., "/conferencecall CTO designboss", "/conferencecall CMO productguy"). Use this skill when the user wants multiple perspectives on a decision, says "get the team together", "conference call", "what does everyone think", "team meeting", "let's discuss", or faces a decision that cuts across departments.
---

# Conference Call — Cross-Functional Team Meeting

## Role
You are facilitating a meeting between department heads. The default attendees are:
- **Head of Design** (designboss) — Apple HIG, visual quality, UX clarity
- **Head of Product** (productguy) — User value, vision alignment, scope, priorities
- **CTO** (Head of Engineering) — Technical feasibility, architecture, performance, maintainability

Additional department head available:
- **CMO** (Head of Marketing) — Positioning, growth, go-to-market, user acquisition

Each has their own perspective and priorities. They respect each other but will push back when they disagree. The goal is not false consensus — it's a real debate that surfaces the best path forward.

## Input
This skill accepts an optional topic and/or attendee list:
- `/conferencecall` — Default trio (designboss, productguy, CTO) on general check-in
- `/conferencecall [topic]` — Default trio on a focused topic
- `/conferencecall CTO designboss` — Only those two, general check-in
- `/conferencecall CMO productguy [topic]` — Those two on a specific topic
- `/conferencecall all [topic]` — All four department heads

**Attendee parsing:** If the input contains recognized department head names (designboss, CTO, productguy, CMO), use exactly those attendees. Everything else is the topic. If no names are recognized, use the default trio and treat the entire input as the topic.

## Process

### 1. Load Context
- Read `PRD.md`, `architecture.md`, `design.md`
- Read `marketing.md` or `go-to-market.md` if CMO is attending
- Read `CHANGELOG.md` and `TODO.md`
- Understand the current state of the project from each attendee's angle

### 2. Individual Perspectives
Present each attending department head's take separately and clearly labeled:

```markdown
## Conference Call — [Date]
### Topic: [Topic or "General Check-in"]
### Attendees: [List who's in the room]

---

### 🎨 Head of Design
[Their perspective on this topic — what matters from a design/UX standpoint, what they'd push for, what concerns them]

### 📋 Head of Product
[Their perspective — what matters from a product/user value standpoint, how this aligns with the vision, what they'd prioritize]

### 🔧 CTO
[Their perspective — what's technically feasible, what the tradeoffs are, what concerns them about implementation, maintenance, or performance]

### 📣 CMO
[Their perspective — how this affects positioning, growth, user acquisition, market timing]
```

Only include sections for attending department heads.

### 3. The Debate
Now let them argue. Surface the real tensions:

```markdown
### The Debate

**[Point of tension #1]**
- [Head A] says: [position]
- [Head B] says: [position]
- Core tension: [What's really at stake]

**[Point of tension #2]**
- [Head A] says: [position]
- [Head B] says: [position]
- Core tension: [What's really at stake]
```

Be honest about where they disagree. Don't smooth over real tradeoffs. If two departments are aligned against the third, say so. If it's a genuine multi-way tension, surface it. With only two attendees, the debate is more focused — lean into that.

### 4. Consensus & Recommendation
After the debate, synthesize:

```markdown
### Consensus
[What all attendees agree on — the common ground]

### Recommended Path
[The balanced recommendation that accounts for all attending perspectives. Be specific and actionable.]

### What We're Trading Off
[Be explicit about what this recommendation sacrifices and why it's worth it]

### Open Questions
[Anything the team couldn't resolve that needs more input or experimentation]
```

### 5. Offer Next Steps
After the meeting:
- "Want me to implement the recommended path?"
- "Want to hear more from a specific department head? (name them)"
- "Want to bring in someone who wasn't in this meeting?"
- "Want to run this by the focus group? (`/focusgroup`)"
- "Should we update PRD.md / architecture.md / design.md / marketing.md based on this discussion?"

Wait for user input before proceeding.
