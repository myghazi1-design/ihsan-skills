---
type: skill-reference
skill: cinematography-prompting
source: Consolidated from Camera Vocabulary.md + Light Sources.md + Physics Translations.md
tags: [cinematography, anti-patterns, prompting]
---

# Cinematography Anti-Patterns

Consolidated don't-do list across camera / light / physics sources.

## Camera anti-patterns

- Listing 5+ camera moves in one shot ("dolly, pan, tilt, zoom, crane") — pick the dominant one
- Mixing contradictory moves ("static camera with fast pan")
- Using "cinematic camera" or "dynamic camera" as a substitute for specifics
- Vague duration ("slowly" without a timeframe — use "over 3 seconds" or "across the clip")
- Specifying technical specs the models ignore: fps, ISO, exact focal length. Use lens buckets (wide / normal / telephoto) instead.

## Lighting anti-patterns

- "Dramatic lighting" / "cinematic lighting" / "moody lighting" / "beautiful lighting" / "atmospheric lighting" — all produce flat gray default output
- "Warm lighting" / "cool lighting" without naming a physical source
- Abstract color ("blue lighting") instead of specific ("cool blue haze filling the corridor")
- Assuming lighting carries forward across multi-segment prompts — repeat the lighting descriptor in every segment

## Motion / physics anti-patterns

- Emotional abstractions as motion directives ("contemplates with heavy heart") — the model cannot translate to pixels
- Free-floating hands — always anchor to an object (Anchor Hands rule)
- Camera and subject motion in the same clause ("spinning camera around a dancing person") → jitter
- Compound camera moves without sequential markers ("dolly AND tilt" — use "dolly, then tilt")

## Universal anti-patterns

- Adjective stacking ("cool, fast, cinematic, amazing, epic, dynamic") — Seedance cherry-picks, Kling ignores
- Banned keywords (unqualified): `fast`, `cinematic` alone, `epic`, `amazing`, `beautiful`, `lots of movement`, `dynamic`, `flowing`
- 3+ characters in frame (Seedance) — face warp + extra limbs
- On-screen text (any model) — garbles at small sizes

## See also

- [[camera-vocabulary]]
- [[light-sources]]
- [[physics-translations]]
- [[2026-04-20-positive-framing-over-negative]]
