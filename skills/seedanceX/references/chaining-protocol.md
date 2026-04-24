---
type: skill-reference
skill: seedanceX
topic: chaining
---

# Seedance Multi-Clip Chaining Protocol

> [!warning] Master-Reference-Reuse is the CANONICAL protocol (2026-04-18)
> Exit-frame chaining (legacy) is SUPERSEDED because Seedance re-processes already-AI-generated frames, compounding drift.

## Master-Reference-Reuse Protocol (canonical)

### The 5-Step Protocol

1. **Anchor the master reference (NBP).** Generate the Nano Banana Pro 4K master reference image (character + setting + style). This is the identity anchor reused across every clip.

2. **Anchor the audio first.** Generate the full voiceover externally (ElevenLabs V3 or Higgsfield Audio). Export as MP3. Upload as `@Audio1` — shared constant across every clip. 15s audio input cap means pre-chunk VO on **natural breath pauses aligned to clip boundaries**.

3. **Anchor the global context.** Write the 1-3 sentence Global Context Block. Identical text across every prompt in the chain. Lock temperature language ("warm 3200K" not "warm-ish").

4. **For every clip, re-upload the NBP master as `@Image1`.** Do NOT use Clip A's exit-frame as Clip B's start-frame — that compounds AI drift. Each clip starts fresh from the original NBP master.

5. **Design seams around B-roll / product / environmental frames.** Dialogue-at-seams causes lip-sync failure; keep dialogue inside a single clip.

## Empirical Chain Ceiling

| Chain length | Status | Mitigation |
|---|---|---|
| 2 clips (~24s) | Reliable — basic workflow proven | Just follow the protocol |
| 3 clips (~36s) | Reliable with full protocol | Global context + `@Audio1` + master-reference-reuse |
| 4 clips (~48s) | Coin flip — requires validation render | Pre-validate with a short test render first |
| 5+ clips | Compounding drift visible to audience eye | Don't ship without per-clip human QA |

> **Test-first flag:** Skeptical review places ceiling at 2-3 clips; optimist says 3-4 with mitigations. Run a validation render before declaring 3-clip "locked."

## What Master-Reference-Reuse Does NOT Solve

| Failure mode | Mitigation |
|---|---|
| Lip sync at clip boundaries (dialogue UGC) | Use Mode 2 within single clip; keep mouth off-camera at seams |
| Mid-clip physics re-inference (skin tone, fabric, lighting temp) | Repeat lighting descriptors in every segment; fixed temperature language |
| Cross-clip world-state reset (crowd density, window light angle) | Design shots around B-roll / environmental / product frames |
| 15s audio input cap | Pre-chunk VO on natural breath pauses aligned to clip boundaries |
| "Keep voice the same" soft instruction has no enforcement | Needs external pre-gen audio anchor (`@Audio1`) |

## Anchor-Frame Technique (within single clip)

For the 2-3s exit moment of any clip that will be followed by another clip:
- Static or near-static motion (2-3s)
- Neutral background
- Subject face neutral / mouth closed
- No dialogue

This creates a safe handoff point even with master-reference-reuse (reduces perceptible discontinuity).

## Basic 2-Clip Extension (simple cases only)

For 2-clip chains where cross-clip identity anchoring isn't needed, the `@Video1` method still works:

1. Generate first clip (up to 15s)
2. Download the clip
3. Upload it back to Higgsfield as `@Video1`
4. Prompt: `Extend @Video1. Keep the voice the same as the original clip.`
5. Specify what happens in the next 15s using Mode 2 timestamp method

For 3+ clips or voice/palette-sensitive chains, use the Master-Reference-Reuse protocol above.

## Audio Chunking Rules (15s cap)

The 15s upload cap means a 60s VO needs ≥4 independent audio segments.

**Rules:**
- Chunk on natural breath pauses
- Align chunks to clip boundaries
- Pre-gen the full VO in ElevenLabs, then split
- Label each chunk with its target clip: `@Audio1_clipA.mp3`, `@Audio1_clipB.mp3`

## Legacy: Exit-Frame Chaining (SUPERSEDED, do not use)

Kept for historical context only. The earlier protocol used Higgsfield Start & End Frames:
1. Anchor audio first (`@Audio1` shared constant)
2. Anchor global context (identical per clip)
3. Generate Clip A with clean static exit frame (2-3s anchor-frame technique)
4. Export End frame of Clip A → Start frame of Clip B via Higgsfield Start & End Frames

**Why superseded:** Seedance re-processes the exit frame (which is already an AI-generated frame), compounding micro-drift. Master-Reference-Reuse keeps every clip anchored to the original NBP master.
