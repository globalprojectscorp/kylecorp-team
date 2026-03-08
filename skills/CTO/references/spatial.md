# Spatial Computing Engineering — CTO Reference

## When to Load
Load this reference when:
- The project targets visionOS, ARKit, RealityKit, or spatial computing
- You see RealityView, ImmersiveSpace, Entity, or spatial-related imports
- The user asks about XR, VR, AR, spatial, or Vision Pro development
- Metal or RealityKit performance is a concern

## Spatial Computing Architecture

### visionOS App Architecture
- **Window → Volume → Immersive Space** progression is intentional
- WindowGroup for 2D content, volumetric for 3D objects, ImmersiveSpace for full immersion
- Shared vs Full immersive spaces — understand the tradeoff (shared allows multitasking)
- App lifecycle: handle immersive space open/dismiss gracefully
- SwiftUI is the primary UI framework; RealityView bridges to 3D

### Entity Component System (ECS)
- RealityKit uses ECS architecture — entities are containers, components hold data, systems process behavior
- Prefer composition over inheritance for entity behavior
- Custom components should be lightweight data holders
- Systems should be stateless processors
- Entity hierarchy matters for transforms and physics

### RealityKit & RealityView
- RealityView for embedding 3D content in SwiftUI
- `make` closure for initial setup, `update` closure for state-driven changes
- Attachments for SwiftUI views positioned in 3D space
- USD/USDZ for 3D assets — Reality Composer Pro for scene assembly
- Anchor entities for spatial anchoring (hand, head, world)

### Spatial Input & Interaction
- Primary input: eyes (targeting) + hands (indirect pinch gesture)
- Direct touch for close-range interaction
- Custom gestures via SpatialTapGesture, DragGesture, RotateGesture, MagnifyGesture
- Hover effects via HoverEffectComponent (system-provided highlight)
- Accessibility: support pointer, switch control, voice control, keyboard

### Performance Requirements
- **90fps is non-negotiable** — dropping frames causes discomfort
- Render budget: ~11ms per frame per eye
- Minimize draw calls, use instancing for repeated geometry
- LOD (Level of Detail) for complex scenes
- Spatial audio processing budget — prioritize nearby sources
- Texture memory management — MIP mapping, compressed formats
- Profile with RealityKit Trace instrument

### Spatial Audio
- Use spatial audio for immersive experiences
- Audio sources positioned in 3D space
- Ambient audio for environment, spatial for objects
- Consider reverb and occlusion for realism

### ARKit Integration
- Hand tracking for custom gesture recognition
- Scene understanding (plane detection, mesh classification)
- World anchors for persistent spatial content
- Image/object recognition for real-world triggers

### Common Pitfalls
- Running heavy computation on the main thread (causes frame drops = nausea)
- Overloading immersive spaces with too many entities
- Ignoring the comfort zone (content too close or requiring neck strain)
- Not handling immersive space lifecycle (system can dismiss your space)
- Forgetting that visionOS apps can run on Mac — test both

## Review Output Format
When reviewing spatial computing code, add a **Spatial Engineering Assessment**:

```markdown
### Spatial Engineering Assessment
- **Architecture:** [ECS properly used / Needs restructuring]
- **Performance:** [Hitting 90fps / At risk / Frame drops likely]
- **Platform Compliance:** [Following visionOS patterns / Deviating]
- **Findings:**
  1. **[Finding]** — [Technical risk in spatial context]
     - Impact: [User comfort, performance, or platform compliance]
     - Fix: [Specific action]
```
