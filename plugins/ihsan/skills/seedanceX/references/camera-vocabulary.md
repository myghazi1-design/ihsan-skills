---
type: skill-reference
skill: seedanceX
topic: camera-vocabulary
---

# Camera Movement Vocabulary (Seedance)

**ONE movement per shot.** Compound moves must be sequential with clear transitions.

## Seedance's Preferred Rig Words

Use rig words (dolly, track, crane, handheld, gimbal), NOT mood words (dynamic, flowing, smooth).

| Movement | Term | Notes |
|----------|------|-------|
| Push-in | `dolly in` | Most reliable movement |
| Pull-out | `dolly out` | Good for reveals |
| Pan | `pan left/right` | Horizontal sweep |
| Tracking | `follow shot` | Pursues moving subject |
| Orbit | `arc` | Rotation around subject |
| Aerial | `drone shot` | High-altitude perspective |
| Handheld | `handheld` | Natural shake; degrades past 8s |
| Static | `locked tripod` | UNRELIABLE on Seedance — adds micro-tilts. Route to Kling. |

## Pacing Words (use these for motion quality)

- `slow`
- `smooth`
- `gentle`
- `gradual`

Improve output when used with a rig word.

## DO NOT Specify

- fps (Seedance ignores or corrupts — train-distribution mismatch)
- ISO
- Exact focal lengths (use buckets: wide / normal / telephoto)

**Exception:** "24 fps feel" as a cadence cue.

## Compound Move Handling

If user asks for multiple moves, convert to sequential with a clear primary:

- Bad: "dolly-in while panning right and crane-up"
- Good: "low tracking then subtle rise"

## Per-Shot Constraint

Seedance reinterprets camera instructions through ByteDance/Douyin genre conventions. Write scene descriptions, not shot instructions. Examples:

- Bad (Kling-style literal): "Camera performs 360° orbit at 2m radius around subject at walking pace"
- Good (Seedance scene): "A woman dances at the center of a circle of onlookers, camera orbits her slowly"

## Anti-Patterns

- Listing 5+ camera moves in one shot → pick the dominant one
- Mixing contradictory moves ("static camera with fast pan")
- Using "cinematic camera" as a substitute for specifics
- Vague duration ("slowly" without a timeframe — use "over 3 seconds" or "across the clip")
