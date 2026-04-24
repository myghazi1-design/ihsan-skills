---
type: skill-reference
skill: review-video
source: notes.md (Frame-Lock Chaining Protocol and failure-mode section)
tags: [review, chain, drift, seedance]
---

# Chain-Drift Signals

What to look for at clip seams in chained Seedance renders. Frame-lock pins only seam pixels; everything else re-infers per clip.

## What frame-lock does NOT solve

| Failure mode | Review signal |
|---|---|
| Lip sync at clip boundaries (dialogue UGC) | Mouth desync at the exact seam frame |
| Mid-clip physics re-inference | Skin tone shift, fabric crinkle change, lighting temperature drift between adjacent clips |
| Cross-clip world-state reset | Background crowd density changes, window-light angle resets, prop positions jump |
| 15s audio input cap on Higgsfield | Voice quality / cadence jumps at audio chunk boundary |

## Seam-specific review procedure

1. **Extract the last 3 frames of Clip A.**
2. **Extract the first 3 frames of Clip B.**
3. Compare across the 6 frames:
   - Lighting temperature (warm 3200K vs warm-ish cream-white)
   - Fabric state (crinkle pattern, fold positions, sheen)
   - Skin tone
   - Background density (crowd size, window light)
   - Prop positions (is the cup in the same spot?)
4. **Any visible jump → chain drift. Flag as "One more pass" or escalate to re-prompt.**

## Empirical chain ceiling (reference)

| Chain length | Expected failure rate at seam |
|---|---|
| 2 clips (~24s) | Reliable — basic workflow |
| 3 clips (~36s) | Reliable with global context block + shared `@Audio1` + anchor-frame exit |
| 4 clips (~48s) | Coin flip — flag for second review |
| 5+ clips | Visible drift — re-plan as shorter video or multi-scene sequence |

## When chain drift = "Re-prompt"

Re-prompt (not just one more pass) when:
- Identity drift is visible to the audience eye (not just side-by-side scrutiny)
- Voice pitch / tone noticeably shifts between clips
- Lighting drifts past "warm" into "different color palette"

At that point, the fix is Master-Reference-Reuse (re-upload NBP master on every clip — see [[2026-04-18-seedance-chaining-protocol]]) or replan as separate scenes.

## See also

- [[rubric-6-axis]]
- [[tool-failure-table]]
- [[2026-04-18-seedance-chaining-protocol]]
