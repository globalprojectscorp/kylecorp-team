# AI Image Prompt Engineering — CMO Reference

## When to Load
Load this reference when:
- The user needs AI-generated imagery for marketing, product, or brand
- Creating prompts for Midjourney, DALL-E, Stable Diffusion, or Flux
- Developing visual content for app stores, landing pages, social media, or presentations
- The user asks about image generation or visual asset creation

## The 5-Layer Prompt Framework

Build prompts by stacking these five layers. Each layer adds specificity and control.

### Layer 1 — Subject Description
- **Primary Subject:** Detailed description of main focus
- **Subject Details:** Attributes, expressions, poses, textures, materials
- **Subject Interaction:** Relationship with environment
- **Scale & Proportion:** Size relationships and spatial positioning

### Layer 2 — Environment & Setting
- **Location Type:** Studio, outdoor, urban, natural, interior, abstract
- **Environmental Details:** Elements, textures, weather, time of day
- **Background Treatment:** Sharp, blurred, gradient, contextual, minimalist
- **Atmospheric Conditions:** Fog, rain, dust, haze, clarity

### Layer 3 — Lighting Specification
- **Light Source:** Natural (golden hour, overcast, direct sun) or Artificial (softbox, rim light, neon)
- **Light Direction:** Front, side, back, top, Rembrandt, butterfly, split
- **Light Quality:** Hard/soft, diffused, specular, volumetric, dramatic
- **Color Temperature:** Warm, cool, neutral, mixed

### Layer 4 — Technical Photography
- **Camera Perspective:** Eye level, low angle, high angle, bird's eye, worm's eye
- **Focal Length Effect:** Wide angle distortion, telephoto compression, standard
- **Depth of Field:** Shallow (portrait), deep (landscape), selective focus
- **Exposure Style:** High key, low key, balanced, HDR, silhouette

### Layer 5 — Style & Aesthetic
- **Photography Genre:** Portrait, fashion, editorial, commercial, documentary, fine art
- **Era/Period Style:** Vintage, contemporary, retro, futuristic, timeless
- **Post-Processing:** Film emulation, color grading, contrast treatment, grain
- **Reference Photographers:** Style influences (Annie Leibovitz, Peter Lindbergh, etc.)

## Ready-to-Use Templates

### Cinematic Portrait
```
Dramatic portrait of [subject], [age/appearance], wearing [attire],
[expression/emotion], photographed with cinematic lighting setup:
strong key light from 45 degrees camera left creating Rembrandt
triangle, subtle fill, rim light separating from [background type],
shot on 85mm f/1.4 lens at eye level, shallow depth of field with
creamy bokeh, [color palette] color grade, inspired by [photographer],
[film stock] aesthetic, 8k resolution, editorial quality
```

### Luxury Product
```
[Product name] hero shot, [material/finish description], positioned
on [surface description], studio lighting with large softbox overhead
creating gradient, two strip lights for edge definition, [background
treatment], shot at [angle] with [lens] lens, focus stacked for
complete sharpness, [brand aesthetic] style, clean post-processing
with [color treatment], commercial advertising quality
```

### Environmental Portrait
```
[Subject description] in [location], [activity/context], natural
[time of day] lighting with [quality description], environmental
context showing [background elements], shot on [focal length] lens
at f/[aperture] for [depth of field description], [composition
technique], candid/posed feel, [color palette], documentary style
inspired by [photographer], authentic and unretouched aesthetic
```

### App Store / Marketing Hero
```
[App interface/feature] displayed on [device] in [environment],
[time of day] ambient lighting, device screen glowing with
[app content], shallow depth of field keeping device sharp,
[lifestyle context around device], clean and aspirational feel,
[brand color palette], commercial product photography quality
```

## Platform-Specific Notes

- **Midjourney:** Use --ar for aspect ratio, --v for version, --style for aesthetic, --chaos for variation. Multi-prompt weighting with :: syntax.
- **DALL-E:** Natural language works best. Be descriptive, avoid technical jargon. Style mixing through description.
- **Stable Diffusion:** Token weighting with (parentheses:1.2), embedding references, LoRA for consistent style.
- **Flux:** Detailed natural language, photorealistic emphasis, strong with text rendering.

## Film Stock References
Use these for specific aesthetic vibes:
- **Kodak Portra 400** — Warm skin tones, soft pastels, wedding/portrait feel
- **Fuji Velvia 50** — Saturated colors, punchy contrast, landscape/nature
- **Ilford HP5** — Classic B&W, medium grain, documentary feel
- **Cinestill 800T** — Tungsten-balanced, halation glow, cinematic night scenes
