---
type: skill-reference
skill: review-video
source: notes (1).md (video production:ai source corpus) + notes.md failure patterns
tags: [review, failure-patterns, seedance, kling, mikos]
---

# Tool-Specific Failure Patterns

Expand as renders accumulate failure data.

## Seedance 2.0 (on Higgsfield)

| Failure | Symptom at review | Fix |
|---|---|---|
| Subtle camera moves fail at short durations | Camera looks static or micro-tilts when prompt said "slow dolly" | Switch to Kling for literal camera instructions; or use longer duration |
| Lip-sync drift at clip boundaries | Mouth desync visible at seam | Keep dialogue in single clip; route to Kling for dialogue-heavy |
| Mid-clip physics re-inference | Skin tone / fabric / lighting temperature changes during a chain | Repeat lighting descriptors in every segment; fixed temperature language |
| 3+ characters | Face warp, extra limbs, identity collision | Max 2 characters per Seedance shot |
| Stacked camera moves | Broken-gimbal jitter | One primary camera move per shot |
| "Cinematic" alone | Gray flat output | Spell out lighting recipe (named source + direction) |
| On-screen text | Garbled 90% of the time | Skip entirely or use one large word |
| Fast hand gestures | Extra fingers, fused hands | Slow down; "hands at rest" |
| Niche equipment | GPNVGs → flashlight, breaching charge → padlock | Lock gear in NBP start frame, NOT in video prompt |
| Long durations (12s+) | Costume drift, face drift mid-shot | Stay under 8s for action; use chaining for length |
| "No cuts" / continuous take | Seedance breaks into multiple shots anyway | Accept it OR use Kling for literal instruction |
| Sparkle / shimmer materials | Sequins, mesh, herringbone cause texture crawl | Avoid these material descriptors |
| Micro-tilt bug on static | "Locked tripod" shot drifts | Route to Kling for truly static shots |

## Kling 3.0 (on Higgsfield)

| Failure | Symptom at review | Fix |
|---|---|---|
| Harder consonants in native audio | Consonants like "k", "t" drop out | Pair-test; rephrase to use softer phonemes |
| Secondary subject drift | Background character morphs during clip | Remove background subject, or move to B-roll |
| Compound camera moves still compute | Result is mechanically correct but uglier than sequential | Sequential camera moves ("X then Y") |

## Mikos / MIKOSLAB V2

| Failure | Symptom at review | Fix |
|---|---|---|
| UGC constraint breaks past 8s ceiling | Lip-sync desync, identity drift | Stay at ≤ 8s per clip; use [[2026-04-20-max-6-second-clips]] rule |
| Lost head-to-head vs Seedance | Worse per-frame quality | Use Seedance for Seedance-best shot types; Mikos 8-section structure still owns creative strategy via [[ugc-creator-setup]] |

## Nano Banana Pro (still-image subclass)

| Failure | Symptom at review | Fix |
|---|---|---|
| Secondary product copy at ~8 – 12 px garbles | Small-text bottom labels unreadable | Up to 3 re-rolls, then composite real label in Affinity / Photoshop |
| Real-face photo blocking | Degraded or blocked output | Use NBP-generated faces, not real reference |

## See also

- [[rubric-6-axis]]
- [[chain-drift-signals]]
- [[2026-04-18-seedance-chaining-protocol]]
