---
type: skill-reference
skill: seedanceX
topic: enforcement-rules
---

# Seedance Enforcement Rules

These rules MUST be enforced by construction in every emitted Seedance prompt.

## Hard Rules (always enforce)

### 1. ONE Primary Camera Movement per Shot
Compound moves cause jitter. If the user asks for multiple moves, convert to sequential with a clear primary: "low tracking then subtle rise".

### 2. 2 Characters Max per Shot
3+ characters = face warp, extra limbs, identity collision. Refuse or reduce.

### 3. Named Light Source
Refuse these terms without replacement: `dramatic lighting`, `cinematic lighting`, `moody lighting`, `beautiful lighting`, `atmospheric lighting`. Substitute with a named source (see `lighting-recipes.md`).

### 4. Separate Camera from Subject Motion
"The dancer spins slowly. Camera holds fixed framing." NOT "spinning camera around a dancing person."

### 5. Rhythmic not Technical
Use rhythmic descriptors (slow, smooth, gradual, gentle). NOT technical specs (fps, f-stop, ISO, exact focal length). Exception: "24 fps feel" as a cadence cue.

### 6. Physical / Observable Actions
Mode 2 Visual lines must be physical, concrete, observable. Refuse emotional abstractions like "contemplates with heavy heart."

### 7. Word Budget
- Mode 1 Cinematic: 60-80 words
- Mode 1 Multi-shot: 100-150 words across all shots
- Mode 2 UGC: 60-100 words per segment, 4-6 segments

Over budget = model cherry-picks. Trim.

### 8. Mode 2 Bracket Format
Use `[M:SS-M:SS]` only. Refuse `0-4 seconds:`, `[0-4s]`, `0:00 to 0:04`.

### 9. Mode 2 Segment Length
2-5s per segment, 4-6 per generation. Outside this range = re-chunk.

### 10. Required Negatives (Mode 1 ending)
End Mode 1 Cinematic with `No music.` Mode 2 also ends with `No music.` (music is post-production via Suno).

### 11. POV / Orbs Constraint
Append exactly this string to POV/Orbs formats: `No cuts, no zoom, natural head movement.`

### 12. One VFX per Shot Beat
One `[VFX: ...]` bracket per shot beat in Mode 1, or per Mode 2 timestamp segment. Do not stack.

### 13. Global Context Block for Multi-Segment
Every Mode 2 multi-segment prompt, every Mode 1 multi-shot prompt, and every chain clip MUST start with an identical Global Context Block.

### 14. Asset Tagging
All uploaded assets MUST carry an `@Image1` / `@Image2` / `@Audio1` / `@Video1` tag with role declared in the prompt.

## Soft Rules (apply + flag)

### 15. Lighting is Highest-Leverage
If adding only one detail for quality improvement, add lighting.

### 16. Repeat Lighting per Segment
In multi-segment or chained prompts, repeat the lighting descriptors in every segment. Don't assume carry-forward.

### 17. Fixed Temperature Language
Use "warm 3200K" not "warm-ish". Fixed values prevent cross-clip drift.

### 18. Genre Anchor When Useful
"Documentary realism", "spy thriller style" — helps Seedance route to the right training distribution.

### 19. Canva Bypass for 2000+ Char Prompts
If the prompt exceeds 2,000 characters, route to the Canva-bypass method. See `canva-bypass.md`.

## Refusal Conditions

Refuse with explanation and offer alternative:

1. **3+ characters in frame** → offer to reduce to 2 or route to `kling-video`
2. **Compound camera moves inseparable** → offer sequential conversion
3. **Dialogue at chain seam** → route dialogue to a single Mode 2 clip; suggest B-roll at seam
4. **Technical specs specified (fps, f-stop, ISO)** → strip and substitute rhythmic descriptors
5. **Chain >4 clips without validation render** → warn this is outside empirical ceiling
6. **Prompt contains banned keyword** → substitute from `banned-keywords.md`
7. **Shot type in Kling deny-list** → route to `kling-video`

## Confidence Tier Propagation

Flag any rule with empirical uncertainty in the output Notes:

- `[Verified]` — Platform constraints (2000 char limit, 4-15s duration, 12 file cap)
- `[High]` — Convergent across research sources
- `[Medium]` — Single practitioner source, mechanistically plausible
- `[Test first]` — Contested; user should validate on own setup
  - Duration-in-prompt
  - `[VFX:...]` brackets
  - Chain ceiling at 3 clips
