---
type: skill-reference
skill: cinematography-prompting
source: Camera Vocabulary.md (video production:ai source corpus)
tags: [cinematography, camera, kling, seedance]
---

# Camera Vocabulary

Named camera movements are anchor tokens Kling 3.0 recognizes from its training data on cinematography-captioned footage. Use these verbatim — don't paraphrase them.

## Substitution Rule

Never output these without a specific replacement:
- ❌ "camera moves"
- ❌ "camera goes closer"
- ❌ "camera follows"
- ❌ "camera rotates"
- ❌ "dynamic camera"
- ❌ "cinematic camera movement"

## Camera Movement Library

### Push / Pull (forward or backward along Z-axis)

| Term | What it produces |
|---|---|
| **Slow dolly push** | Camera moves closer through space with parallax |
| **Crash push** | Fast aggressive forward movement, snap at end |
| **Dolly pull back** | Camera retreats, revealing context |
| **Slow zoom** | Optical zoom, no parallax (different from dolly) |
| **Crash zoom** | Fast zoom in, dramatic |
| **Reverse dolly** | Moves back while subject moves forward |

### Lateral (side-to-side)

| Term | What it produces |
|---|---|
| **Tracking shot** | Camera follows subject laterally at consistent distance |
| **Parallel tracking** | Camera moves alongside subject at same speed |
| **Lateral slide** | Pure sideways movement across scene |
| **Whip pan** | Fast horizontal pan, motion blur transition |
| **Slow pan left/right** | Controlled horizontal rotation |
| **Fast lateral pass** | Quick side-to-side past subject |

### Vertical (up/down)

| Term | What it produces |
|---|---|
| **Crane up** | Camera rises revealing more of scene |
| **Crane down** | Camera descends into scene |
| **Tilt up** | Vertical rotation from ground to sky |
| **Tilt down** | Vertical rotation from above to below |
| **Elevator shot** | Pure vertical movement |

### Rotational

| Term | What it produces |
|---|---|
| **Orbit** | Circles subject at consistent distance |
| **180° arc** | Half-circle around subject |
| **360° rotation** | Full circle around subject |
| **Roll** | Camera rotates along its own axis |

### Handheld / Operator

| Term | What it produces |
|---|---|
| **Handheld** | Natural operator movement, documentary feel |
| **Handheld with subtle shake** | Slight imperfection, tension |
| **Shoulder-cam drift** | Over-shoulder with sway |
| **Steadicam** | Smooth but not rigid, flowing |
| **Gimbal stabilized** | Very smooth, no shake |

### Specialty

| Term | What it produces |
|---|---|
| **POV shot** | First-person perspective |
| **Over-the-shoulder** | Shot framed from behind a character |
| **Rack focus** | Focus shifts from foreground to background |
| **Snap focus** | Sudden focus change |
| **Speed ramp** | Variable speed within shot (e.g., 40% → 100%) |
| **Robotic arm** | Precise programmed movement, snappy |
| **Drone shot / aerial** | Overhead following |
| **Bird's eye** | Directly overhead, perpendicular to ground |
| **Worm's eye / low angle** | From below looking up |
| **Dutch angle** | Tilted frame, unease |

## Shot Sizes

| Term | What it shows |
|---|---|
| **Extreme close-up (ECU)** | Part of a face, an eye, fine detail |
| **Macro** | Very tight, object-level detail |
| **Close-up (CU)** | Face and neck |
| **Medium close-up (MCU)** | Head and shoulders |
| **Medium shot (MS)** | Waist up |
| **Medium wide** | Knees up |
| **Wide shot (WS)** | Full body in environment |
| **Extreme wide (EWS)** | Landscape with small figures |
| **Establishing shot** | Context-setting wide |
| **Two-shot** | Two characters in frame |
| **Over-the-shoulder** | Behind one character looking at another |

## Lens Character

| Lens | Character |
|---|---|
| **24mm wide angle** | Wide field, architectural, landscape |
| **35mm** | Natural human-eye perspective |
| **50mm** | Flattering for portraits, slight compression |
| **85mm** | Portrait compression, shallow depth |
| **100mm macro** | Extreme detail |
| **200mm telephoto** | Compressed background, isolated subject |
| **Anamorphic** | Horizontal lens flares, cinematic aspect |

## Assembly Examples

Camera layer with proper vocabulary:

> "**Slow dolly push** from medium shot to extreme close-up over 3 seconds. **85mm lens**, shallow depth of field. Background falls completely to bokeh."

> "**Handheld shoulder-cam drift** behind subject with subtle sway. **35mm lens**, natural perspective. Middle distance sharp, background soft."

> "**Static locked-off tripod** shot. **24mm wide**. Deep focus front-to-back."

> "**Smooth steadicam** tracking alongside subject at walking pace. **50mm lens**, shallow depth of field isolating subject."

> "**Crash push** into the face (0.8 seconds), then hold. **100mm lens**. Very snappy acceleration."

> "**Orbit** 180° around the product at constant distance. **35mm lens**. Studio lighting. Shallow depth of field keeping the orbit subject sharp."

## Anti-Patterns

Don't do:
- Listing 5+ camera moves in one shot ("dolly, pan, tilt, zoom, crane") — pick the dominant one
- Mixing contradictory moves ("static camera with fast pan")
- Using "cinematic camera" as a substitute for specifics
- Vague duration ("slowly" without a timeframe — use "over 3 seconds" or "across the clip")

## Per-Kling-3.0 Note

atlascloud.ai's most cited observation: **adding "tracking camera" alone** to an otherwise static prompt was consistently one of the biggest single quality improvements in their testing. Worth testing on your own setup. [Medium confidence]
