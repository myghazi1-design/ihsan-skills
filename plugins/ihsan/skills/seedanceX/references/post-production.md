---
type: skill-reference
skill: seedanceX
topic: post-production
---

# Seedance Post-Production Stack

The post-production stack applied AFTER Seedance renders complete. **Never bake music into the generation itself.**

## The Pipeline

1. **Export from Higgsfield** → original render
2. **Topaz Proteus** → upscale to 2× resolution AND 60fps (interpolated)
3. **CapCut** → import, export at 30fps (smooths out while keeping natural feel)
4. **Captions.ai** (or CapCut caption feature) → caption generation
5. **Suno AI** → generate music, layer on top

## Why 2× 60fps → 30fps

The Topaz 2× upscale runs frame interpolation to 60fps. CapCut re-encoding at 30fps (half) produces a cleaner natural-feeling motion than either:
- 30fps directly from Seedance (occasionally choppy)
- 60fps final export (overly smooth / AI-looking)

## Why Music is Post-Production Only

Seedance generates native sound effects and music when prompted. This should NOT be used for:
- Branded content (music rights)
- Chained clips (music doesn't continue across clips)
- UGC (native music sounds AI-generated)

Suno AI at the end gives you full music control, consistent across chained clips, and better quality than native generation.

## Captions Rules

- Use sentence case, not all caps
- Color: white with thin black outline
- Font: Montserrat or Proxima Nova (sans-serif)
- Position: bottom center, 10% margin
- Timing: match the VO from `@Audio1` (if used) exactly

## Audio Chain Integration

For multi-clip chains:
- Generate the FULL VO in ElevenLabs V3 first
- Feed it as `@Audio1` to every clip (chunked)
- In post-production, use the single full-length VO as the audio track
- The chunked per-clip `@Audio1` was only for lip-sync reference; the master VO plays over the final edit

This fixes the 15s audio cap issue at the editing stage, not the generation stage.
