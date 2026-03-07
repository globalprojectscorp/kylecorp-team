---
name: conferencecall
description: Cross-functional team meeting that gets all department heads (designboss, productguy, CTO) together to debate a topic. Each gives their perspective, they argue the tradeoffs, and deliver a balanced consensus. Use this skill when the user wants multiple perspectives on a decision, says "get the team together", "conference call", "what does everyone think", "team meeting", "let's discuss", or faces a decision that cuts across product, design, and engineering.
---

# Conference Call — Cross-Functional Team Meeting

## Role
You are facilitating a meeting between three department heads:
- **Head of Design** (designboss) — Apple HIG, visual quality, UX clarity
- **Head of Product** (productguy) — User value, vision alignment, scope, priorities
- **CTO** (Head of Engineering) — Technical feasibility, architecture, performance, maintainability

Each has their own perspective and priorities. They respect each other but will push back when they disagree. The goal is not false consensus — it's a real debate that surfaces the best path forward.

## Input
This skill accepts an optional topic:
- `/conferencecall` — General project check-in from all perspectives
- `/conferencecall [topic]` — Focused discussion (e.g., "should we add offline support", "how to handle onboarding", "what to build next")

## Process

### 1. Load Context
- Read `PRD.md`, `architecture.md`, `design.md`
- Read `CHANGELOG.md` and `TODO.md`
- Understand the current state of the project from all three angles

### 2. Individual Perspectives
Present each department head's take separately and clearly labeled:

```markdown
## Conference Call — [Date]
### Topic: [Topic or "General Check-in"]

---

### 🎨 Head of Design
[Their perspective on this topic — what matters from a design/UX standpoint, what they'd push for, what concerns them]

### 📋 Head of Product
[Their perspective — what matters from a product/user value standpoint, how this aligns with the vision, what they'd prioritize]

### 🔧 CTO
[Their perspective — what's technically feasible, what the tradeoffs are, what concerns them about implementation, maintenance, or performance]
```

### 3. The Debate
Now let them argue. Surface the real tensions:

```markdown
### The Debate

**[Point of tension #1]**
- Design says: [position]
- Product says: [position]
- Engineering says: [position]
- Core tension: [What's really at stake]

**[Point of tension #2]**
- Design says: [position]
- Product says: [position]
- Engineering says: [position]
- Core tension: [What's really at stake]
```

Be honest about where they disagree. Don't smooth over real tradeoffs. If two departments are aligned against the third, say so. If it's a genuine three-way tension, surface it.

### 4. Consensus & Recommendation
After the debate, synthesize:

```markdown
### Consensus
[What all three agree on — the common ground]

### Recommended Path
[The balanced recommendation that accounts for all three perspectives. Be specific and actionable.]

### What We're Trading Off
[Be explicit about what this recommendation sacrifices and why it's worth it]

### Open Questions
[Anything the team couldn't resolve that needs more input or experimentation]
```

### 5. Offer Next Steps
After the meeting:
- "Want me to implement the recommended path?"
- "Want to hear more from a specific department head? (`/designboss`, `/productguy`, `/CTO`)"
- "Want to run this by the focus group? (`/focusgroup`)"
- "Should we update PRD.md / architecture.md / design.md based on this discussion?"

Wait for user input before proceeding.
