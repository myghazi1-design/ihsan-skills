---
type: skill-reference
skill: shotlist
source: notes.md (chaining protocol) + 2026-04-18 decision
tags: [shotlist, chain, seedance]
---

# Chain Planning (2 – 4 clip chains)

For videos longer than 15s, plan the chain up front. Decide method, seams, and the audience-eye risk BEFORE generation.

## Canonical method — Master-Reference-Reuse

See [[2026-04-18-seedance-chaining-protocol]] for the decision rationale.

1. Generate the clean 4K master in [[nbp]] (NBP).
2. Re-upload the same NBP master on every clip in the chain.
3. Identical global context block leading every clip.
4. Shared `@Audio1` pre-generated in ElevenLabs V3.
5. Anchor-frame exit on each clip (2 – 3s static segment at the end).

## Empirical chain ceiling

| Chain length | Status | Planning rule |
|---|---|---|
| 2 clips (~24s) | Reliable | Proceed |
| 3 clips (~36s) | Reliable with MRR + global context + `@Audio1` + anchor-frame | Proceed with audio pre-chunked on natural breath pauses |
| 4 clips (~48s) | Coin flip | Require a validation render at clip 3 before committing to clip 4 |
| 5+ clips | Visible drift to audience eye | Re-plan as shorter video or multi-scene sequence |

## Seam design rules

- **Avoid mouth-on-camera at seams** — lip-sync drifts at boundaries.
- **Place B-roll / product / environmental frames at seams** — no identity at the boundary = no drift to detect.
- **Pre-chunk voiceover on natural breath pauses aligned to clip boundaries** (Higgsfield 15s audio input cap).

## When to fall back to 2-clip

If during planning you find:
- The shot list keeps putting dialogue at every seam
- You can't find a natural B-roll seam
- The validation render for clip 3 fails any of the 6-axis checks

Fall back to 2 clips + different post-edit assembly (use CapCut transitions between renders). Don't force 3+ on a chain that wants 2.

## Chain review protocol

After chain clips are rendered, run [[review-video]] per [[chain-drift-signals]]:
1. Extract last 3 frames of Clip A + first 3 frames of Clip B.
2. Compare lighting temp / skin / fabric / background density / prop positions.
3. Any visible jump → "One more pass" with a specific fix (pre-chunk audio, fixed temperature language, relocate seam).

## Supersedes

The earlier exit-frame / Start-End-Frame pinning method. See [[2026-04-18-seedance-chaining-protocol]] — Master-Reference-Reuse is the canonical default.
