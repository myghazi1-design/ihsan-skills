---
type: skill-reference
skill: shotlist
source: notes (4).md (video production:ai source corpus)
tags: [shotlist, template, orchestration]
---

# Shot-List Output Template

Canonical output format for a shotlist delegation run. Fill each section in order.

## Full template

```
# [Concept name] — Shot List

## Concept
[What the video is, who it's for, what the viewer should feel — 2–3 sentences]

## Target platform & aspect
[YouTube 16:9 | TikTok / Shorts / Reels 9:16 | Square 1:1]

## Total duration
[N seconds across M clips]

---

## Hook (Clip 1, first 1–2s)
**Hook category:** [from hook-library — visual / pain-point / curiosity / etc.]
**Visual hook:** [physical action in first frame]
**Spoken hook:** [opening line, if any]

---

## Shot Table

| # | Duration | Tool | Shot type | Dependency | NBP | Notes |
|---|---|---|---|---|---|---|
| 1 | 6s | Seedance | Hook / cinematic | — | NBP-1 | Opens on subject gesture |
| 2 | 5s | Kling | Talking head | Clip 1 subject | NBP-1 | Lip-sync precision, static tripod |
| 3 | 4s | Seedance | Product close-up | Independent | NBP-2 | Product hero, orbit camera |
| ... | | | | | | |

---

## NBP Prompts (4K start / reset frames)

### NBP-1 — Subject start frame
[Full Nano Banana Pro prompt using /nbp]

### NBP-2 — Product hero
[Full NBP prompt]

---

## Video Prompts

### Clip 1 — [Shot name]
**Tool:** Seedance 2.0
**Duration:** 6s
**Aspect:** 16:9
**Prompt:**
```
[Full Seedance prompt assembled via /seedanceX]
```

### Clip 2 — [Shot name]
**Tool:** Kling 3.0
**Duration:** 5s
**Aspect:** 16:9
**Prompt:**
```
[Full Kling prompt assembled via /kling]
```

...

---

## Parallel Generation Plan

### Islands (kick off first)
- Clip 3 (independent product close-up)
- Clip 5 (independent B-roll)

### Chains (plan seams)
- Clip 1 → Clip 2: shared NBP-1, Master-Reference-Reuse
- Clip 4 → Clip 5: standalone clip (no chain)

---

## Post-Production Stack

1. Export from Higgsfield
2. Topaz Proteus — 2× upscale + 60 fps
3. CapCut — export at 30 fps
4. Captions via Captions.ai or CapCut
5. Music layer via Suno AI (added in post, never baked into generation)
```

## Field notes

- **Total duration** should add up to the sum of clip durations in the table. If it doesn't, something's wrong.
- **Dependency** column maps chain relationships. Empty dependency = island.
- **NBP** column: every clip should cite its NBP reference unless it's pure B-roll with no character.
