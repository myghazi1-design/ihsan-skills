---
type: skill-reference
skill: seedanceX
topic: banned-keywords
---

# Banned Keywords (Seedance) — Words to Never Use

These words either produce flat output or confuse the model. Every emission must be scanned and substituted.

## Never-Use List

- `cinematic` alone
- `epic`
- `amazing`
- `beautiful`
- `fast` (unqualified)
- `lots of movement`
- `dynamic`
- `flowing`

## Substitution Table

| Banned | Replacement rule |
|---|---|
| `cinematic` (alone) | Spell out the lighting recipe: "cinematic film tone, 35mm, warm" |
| `fast` (unqualified) | Make ONE element fast, name it: "fast dolly push" or "fast hand gesture" |
| `epic` | Describe the visual effect explicitly: "wide vista, low angle, golden backlight" |
| `amazing` / `beautiful` | Specific lighting + composition |
| `lots of movement` | Name the single specific motion: "a single car passes through frame" |
| `dynamic` | Rig word: "handheld with subtle shake" |
| `flowing` | Rig word: "gimbal stabilized" |

## Why These Fail

- Adjective stacking causes Seedance to cherry-pick random details
- "Cinematic" alone produces flat gray output
- "Lots of movement" + "fast" guarantees jitter
- Abstractions have no pixel mapping in the training distribution

## Scan Checklist

Before emission, grep the prompt for all eight banned tokens. Any hit = rewrite.
