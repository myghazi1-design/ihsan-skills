---
name: voice-elevenlabs
description: Use when the user asks to generate a voiceover, design a voice, clone a voice, or prepare an `@Audio1` anchor for a Seedance/Kling video chain. Also use when the user mentions "ElevenLabs", "11Labs", "Eleven Labs v3", "Voice Design", "Instant Clone", "image-to-voice", or "voice ID reference". Stub-quality skill — the ElevenLabs-specific workflow details are referenced but not fully captured in the current source corpus.
version: 0.2.0-stub
tested_on: ElevenLabs v3 (April 2026, partial)
---

# Voice — ElevenLabs v3 (STUB)

Voice generation for AI video pipelines. Primary use: generate `@Audio1` — the shared voice anchor across multi-clip Seedance chains. This skill captures what's known from the source corpus; full Voice Design prompting, Instant Clone, and image-to-voice workflows need additional source material for a complete skill.

## When to Use

Activate when the user:
- Asks to "generate a voiceover", "design a voice", "clone a voice", "fix robotic audio"
- Mentions "ElevenLabs" / "11Labs" / "Eleven Labs" / "voice ID reference"
- Needs an `@Audio1` upload for a Seedance Mode 2 UGC prompt or multi-clip chain
- Has to decide between ElevenLabs vs Minimax vs Qwen3-TTS vs Resemble

## Core Rationale

Seedance's native voice is "decent", Kling's is "best" for lip-sync, but ElevenLabs v3 beats both for audio quality. Pre-generating the VO in ElevenLabs and uploading as `@Audio1` to Higgsfield gives:

1. Better voice quality
2. Enforcement mechanism for "keep the voice the same" across chained clips (which is otherwise an unenforceable soft instruction)
3. Audio that survives the 15s chunk boundary (the chain anchor)

## Tool Selection

| Tool | Best For | Cost |
|---|---|---|
| **ElevenLabs v3** | Best quality. Use Voice Design with "good quality" descriptor for "in the room" realism. | Paid, tiered |
| **Minimax** | Easy + fast. Natural cadence. Clone with 5s clip. | $9/mo |
| **Resemble AI / Chatterbox** | Fix robotic AI audio. Pipeline: Extract → Adobe Podcast → Resemble. | Paid |
| **Qwen3-TTS** | Free, local, offline. Short clips. | Free |

## How to Use (partial)

### Step 1 — Pick the tool

Use the Tool Selection table above. For video pipelines, default to ElevenLabs v3.

### Step 2 — Voice Design (ElevenLabs v3)

Use Voice Design with the "good quality" descriptor for "in the room" realism. Full prompt format for Voice Design is NOT captured in the current corpus — TODO section below.

### Step 3 — Voice ID Reference Method (Higgsfield native alternative)

If you don't want to generate the voice separately:

1. Find a video with the exact voice quality you want
2. Extract the audio
3. Upload to Higgsfield as `@Audio1`
4. Prompt: `Use @Audio1 as voice ID reference for her voice. Do not make her say the words in the audio, just use it as a voice ID.`

The model replicates that voice for your custom dialogue while keeping the natural tone.

### Step 4 — For multi-clip chains

The 15s audio cap means a 60s VO needs ≥4 independent audio segments. Rules:
- Generate the FULL VO in ElevenLabs first (the whole script)
- Chunk on natural breath pauses
- Align chunks to clip boundaries
- Label chunks: `@Audio1_clipA.mp3`, `@Audio1_clipB.mp3`
- In final post-production, use the single full-length VO over the edit (the chunked per-clip `@Audio1` was only for lip-sync reference)

## What's NOT Captured — TODO

The following subtopics are referenced in source but not enumerated:
- **Voice Design prompt templates** — exact wording for Voice Design inputs
- **Instant Clone workflow** — steps, sample duration required, best practices
- **Image-to-voice** — the referenced feature for pairing voice with a character image
- **Integration with Resemble AI / Chatterbox** — the "robotic audio fix" pipeline

**Action for user:** Provide or link to the ElevenLabs v3 prompt reference, and any hands-on notes about Voice Design / Instant Clone / image-to-voice so these sections can be filled out.

## Reference Files

Living in `Skills/voice-elevenlabs/`:
- `notes.md` — what's known from current source corpus
- `voice-id-reference-method.md` — the Higgsfield-native alternative, fully documented
- `tool-comparison.md` — ElevenLabs vs Minimax vs Resemble vs Qwen3-TTS quick pick
- `audio-chunking-for-chains.md` — 15s cap handling for Seedance chains

## See also

- `seedanceX` — downstream consumer of `@Audio1`
- `kling-video` — has native audio; ElevenLabs is still better quality for chain consistency
- `shotlist` (Overseer B) — orchestrator that triggers voice-elevenlabs for VO generation
