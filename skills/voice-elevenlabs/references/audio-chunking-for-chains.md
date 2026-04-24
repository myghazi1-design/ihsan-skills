---
type: skill-reference
skill: voice-elevenlabs
topic: audio-chunking
---

# Audio Chunking for Seedance Chains

Higgsfield's 15s audio input cap means a 60s voiceover needs at least 4 independent audio segments. This file documents how to chunk cleanly.

## The Problem

Seedance's `@Audio1` input has a 15s cap. For videos >15s (every multi-clip chain), you can't upload the whole VO — you have to chunk it. If you chunk badly (mid-word, mid-syllable, mid-breath), each clip's lip-sync goes off and the seams are audible.

## The Rule

**Chunk on natural breath pauses, aligned to clip boundaries.**

## 4-Step Protocol

1. **Write the full VO script first** — all clips combined. This is the master.
2. **Generate the full VO in ElevenLabs v3 Voice Design** — single pass, no breaks.
3. **Identify natural breath pauses** — listen/scan the waveform. Pauses longer than ~300ms are candidates.
4. **Split at pause points that fall within 13-15s of each other** — gives margin under the 15s cap. Save each chunk as `@Audio1_clipA.mp3`, `@Audio1_clipB.mp3`, etc.

## Aligning Chunks to Clip Boundaries

The clip boundaries (Shot 1, Shot 2, Shot 3 of a chain) should fall on the same breath pauses. When writing the original script:

1. Draft the script in clip-sized blocks (8-14s each)
2. Write each block so the final sentence ends on a natural breath
3. This means the VO breath pause already aligns with the planned clip seam

## Final Post-Production

**Important:** the chunked per-clip `@Audio1` uploads are ONLY for lip-sync reference during Seedance generation. In the final edit:

- Use the SINGLE full-length ElevenLabs VO over the stitched video
- Do NOT stitch the chunks — there's a small gap / click at each chunk boundary
- The full master VO is continuous and pro-quality

The per-clip chunks are scaffolding; the master VO is the deliverable.

## Failure Modes

- **Chunking mid-word** → audible glitch at seam, lip-sync misaligned
- **Chunks longer than 15s** → Higgsfield rejects upload
- **Chunks that don't align to clip boundaries** → lip-sync drift mid-clip
- **Using chunked audio in the final edit** → tiny gaps/clicks at stitch points

## Example Chunking Plan for a 36s (3-clip) Chain

```
Clip A (0-12s):
  VO lines 1-2, ending on a natural breath at ~12s
  Export: @Audio1_clipA.mp3 (11.8s)

Clip B (12-24s):
  VO lines 3-4, ending on a natural breath at ~24s
  Export: @Audio1_clipB.mp3 (11.9s)

Clip C (24-36s):
  VO lines 5-6, ending at ~36s
  Export: @Audio1_clipC.mp3 (11.7s)

Final edit: the full 36s master ElevenLabs VO plays over the stitched Seedance output.
```

## See Also

- [[Skills/seedanceX/chaining-protocol]] — full Master-Reference-Reuse chain protocol
- [[Skills/voice-elevenlabs/voice-id-reference-method]] — alternative to ElevenLabs for simpler jobs
