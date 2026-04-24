---
type: skill-reference
skill: seedanceX
topic: failure-patterns
---

# Seedance Known Failure Patterns

Known Seedance 2.0 failures and their mitigations.

## Per-Clip Failures

| Problem | Why | Fix |
|---------|-----|-----|
| 3+ characters | Faces drift, bodies warp, extra limbs | Max 2 per shot |
| Stacked camera moves | "Jittery mess like a broken gimbal" | ONE movement per shot |
| Prompts over 100 words (action mode) | Model cherry-picks random details | Cut to 60-80 words |
| Vague style language | "Cinematic" alone = flat gray output | Spell out the lighting recipe |
| On-screen text | Garbled 90% of the time | Skip entirely or one large word |
| Fast hand gestures | Extra fingers, fused hands | Slow everything down, "hands at rest" |
| Niche equipment | GPNVGs → flashlight, breaching charge → padlock | Get gear correct in NBP start frame |
| Long durations (12s+) | Costume changes, face drift mid-shot | Stay under 8s for action, use extensions for length |
| "No cuts" / continuous take | Seedance breaks into multiple shots | Accept it or use Kling for literal instruction following |
| Sparkle/shimmer materials | Sequins, mesh, herringbone cause texture crawl | Avoid these material descriptors |
| Technical specs (fps, f-stop, ISO) | Ignored or corrupted; train-distribution mismatch | Use rhythmic descriptors instead |
| Static / locked tripod | Micro-tilt bug on Seedance | Route to Kling |

## Chaining Failures

| Problem | Why | Fix |
|---------|-----|-----|
| Copy-of-a-copy extension | By cycle 3, each clip re-infers from a previous AI-generated frame | Use Master-Reference-Reuse (re-upload NBP master per clip) |
| Lip-sync at clip boundaries | Seedance lip-sync is "decent" vs Kling "best"; seam is worst case | Keep dialogue inside single Mode 2 clip |
| Mid-clip physics re-inference | Frame-lock pins ONLY seam pixels; skin tone / fabric / lighting re-inferred | Repeat lighting descriptors in every segment, fixed temperature |
| Cross-clip world-state reset | Background crowd density, window-light angle, prop positions reset | Design shots around B-roll / environmental / product frames |
| 15s audio upload cap | A 60s VO requires ≥4 independent audio segments | Pre-chunk on natural breath pauses aligned to boundaries |
| "Keep voice the same" has no enforcement | Soft instruction, not a mechanism | Pre-gen external audio anchor |
| 4th clip is a coin flip | Compounding micro-drift becomes visible | Validation render required |
| 5+ clips | Drift visible; multiple shoots appearance | Break into separate deliverables |

## Platform-Level Failures

| Problem | Fix |
|---|---|
| Regional access — business email required outside US/Japan | Personal email = hard wall for collaborators |
| Real-face photo blocking | Identifiable real faces produce degraded or blocked output — use NBP-generated faces |
| Pro/Ultimate gating on Start & End Frames | Free/basic plans cannot access frame-lock primitive |
| 2000-character Canva bypass is fragile | Depends on emergent OCR; can break on platform update |

## Contested Areas (Test-First)

| Contested claim | Skeptic | Optimist | Resolution |
|---|---|---|---|
| Duration in prompt | `notes.md` says omit | Higgsfield says at top; ByteDance API uses `--duration` | A/B test required |
| `[VFX:...]` brackets | Marketing cargo cult | Triangulated across sources | One Orbs test |
| Chain ceiling | 2-3 clips | 3-4 with mitigations | Validation render |

## Workarounds Catalog

- **Dialogue chains** → keep dialogue within single clip (Mode 2 UGC timestamp), use B-roll / environmental / product frames at seams
- **Audio >15s** → chunk on natural breath pauses aligned to clip boundaries
- **Canva bypass failure** → split into 2 shorter prompts chained via Master-Reference-Reuse
- **Mid-clip physics drift** → repeat lighting descriptors in every segment, lock temperature language (fixed not ranged)
- **3+ character requests** → offer to reduce OR route to Kling
- **Real-face needs** → generate faces in NBP first, use those as reference

## Review Checklist (5-point negative audit)

Before shipping any Seedance render, audit the output for:
1. Jitter (any)
2. Bent limbs (people shots)
3. Temporal flicker (videos over 10s)
4. Identity drift (character content)
5. Chaotic composition (complex scenes)

For chains, additionally:
- Lighting temperature drift between clips
- Fabric state continuity (crease positions, wrinkle patterns)
- World density continuity (crowd, props, ambient elements)
