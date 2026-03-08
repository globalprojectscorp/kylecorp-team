# Brand Consistency — Design Boss Reference

## When to Load
Load this reference when:
- The user asks about brand consistency (`/designboss brand`)
- The project has an established visual identity that needs guarding
- Reviewing new screens or components for consistency with existing UI
- The project has grown large enough that visual drift is a risk

## Brand Consistency Evaluation

### Visual Identity Coherence
- **Color palette:** All colors used are from the defined palette (no rogue hex values)
- **Typography:** Consistent font usage (weights, sizes, line heights match the system)
- **Iconography:** Consistent icon style (outline vs filled, stroke width, visual weight)
- **Spacing system:** Consistent use of spacing scale (not arbitrary pixel values)
- **Corner radii:** Consistent across similar elements
- **Shadows & elevation:** Following a defined depth system

### Component Consistency
- Same interaction pattern = same component (no "almost-the-same" variants)
- Button hierarchy is clear and consistent (primary, secondary, tertiary, destructive)
- Form elements share visual language throughout
- Cards, lists, and containers follow the same structural pattern
- Loading states, empty states, and error states are visually unified

### Tone & Voice
- Copy style is consistent (formal/informal, technical/accessible)
- Error messages follow the same pattern and tone
- Labels and headings use consistent capitalization
- Microcopy (tooltips, placeholders, helper text) has a unified voice

### Cross-Screen Coherence
- Navigation patterns are consistent across all screens
- Transitions and animations follow the same physics/timing
- New screens feel like they belong to the same app
- No screen feels like it was designed by a different team

### Design Drift Detection
Watch for:
- One-off colors that don't match the palette
- Components that are "close but not quite" the standard version
- Inconsistent spacing that creates visual tension
- Typography that deviates from the scale (17.5px instead of 17px)
- Interaction patterns that break user expectations set elsewhere in the app

## Review Output Format
When reviewing brand consistency, add a **Brand Assessment**:

```markdown
### Brand Assessment
- **Consistency:** [Unified / Minor drift / Significant inconsistency]
- **Findings:**
  1. **[Finding]** — [Where the brand drifts and the impact]
     - Expected: [What it should be per the design system]
     - Actual: [What it is]
     - Fix: [Specific correction]
```
