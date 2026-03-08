# Spatial Design — Design Boss Reference

## When to Load
Load this reference when:
- The project targets visionOS or spatial computing
- You see spatial UI patterns, 3D layouts, or immersive experiences
- The user asks about spatial design, Vision Pro UX, or XR interaction
- Evaluating comfort, ergonomics, or spatial hierarchy

## Spatial Design Principles

### The Spatial HIG Foundation
Apple's spatial design extends HIG into three dimensions. The same principles apply — clarity, deference, depth — but depth is now literal, not metaphorical.

### Window & Volume Design
- **Windows** float in the user's space — respect their environment
- Default window placement: comfortable viewing distance (~1.5m), slightly below eye level
- Windows should have glass material (system-provided) for environmental blending
- Don't fight for attention — the user controls placement
- Volumetric content should have a clear "front" and be interesting from multiple angles
- Ornaments (floating toolbars) for contextual controls — don't clutter the window

### Spatial Layout & Hierarchy
- **Z-axis is meaningful** — closer = more important/interactive, further = ambient/contextual
- Don't scatter content randomly in 3D space; use depth with purpose
- Group related elements at the same depth plane
- Progressive disclosure works spatially: overview → lean in → detail
- Respect the "comfort zone" — content between 0.5m and 5m from the user

### Interaction Design
- **Eyes + hands** is the primary input — design for gaze targeting
- Interactive elements need generous hit targets (minimum 60pt in spatial)
- Hover states are critical — the system highlights what the user looks at
- Direct manipulation for close-range; indirect pinch for at-a-distance
- Don't require sustained arm-raising — hands rest at sides naturally
- Feedback must be immediate — spatial latency is more disorienting than 2D

### Immersive Experience Design
- Shared spaces: your content coexists with other apps and the room
- Full immersion: you own the entire visual field — use responsibly
- Transition between immersion levels should be gradual, not jarring
- Passthrough blending: consider how your content looks against real environments
- Dark environments need care — bright UI elements feel harsh in dim rooms

### Typography & Readability in Space
- Text needs to be larger than on 2D screens — it's further from the eyes
- Minimum text size is larger in spatial (consider viewing distance)
- High contrast is essential — glass materials can reduce readability
- Vibrancy materials help text pop against varied backgrounds
- Avoid placing text on moving or complex 3D surfaces

### Motion & Animation in Space
- Motion in spatial is more visceral — subtlety matters even more
- Objects should move with physically plausible dynamics (gravity, inertia)
- Avoid rapid movement toward the user (startling/uncomfortable)
- Parallax and subtle depth shifts reinforce spatial presence
- 90fps is the floor — choppy animation breaks immersion AND causes discomfort

### Comfort & Ergonomics
- **Comfort is non-negotiable** — nausea-inducing design is broken design
- Content in the peripheral vision should be ambient, not demanding attention
- Don't require the user to turn their head frequently
- Seated use is the primary posture — design for it
- Duration awareness: spatial experiences can be tiring; provide natural break points

### Spatial Audio as Design
- Sound reinforces spatial relationships (a button behind you sounds behind you)
- Audio feedback for interactions the user can't see directly
- Ambient soundscapes for immersive environments
- Volume should respect the user's real environment

## Red Flags (Spatial-Specific)
- Content placed too close (closer than arm's reach without reason)
- UI requiring sustained arm-raising or head-turning
- Flat 2D interfaces ported to 3D with no spatial consideration
- Ignoring the glass material system (opaque windows feel alien in visionOS)
- Rapid or unpredictable motion toward the user
- Text that's too small to read at spatial viewing distances
- Immersive experiences with no graceful exit

## Review Output Format
When reviewing spatial design, add a **Spatial Design Assessment**:

```markdown
### Spatial Design Assessment
- **Spatial Maturity:** [2D port / Spatially aware / Truly spatial]
- **Comfort:** [Comfortable / Minor issues / Discomfort risk]
- **Findings:**
  1. **[Finding]** — [What the user would experience]
     - Spatial principle violated: [Which principle]
     - Fix: [Specific design action]
```
