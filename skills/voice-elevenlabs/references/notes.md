---
type: skill-reference
skill: voice-elevenlabs
status: partial-stub
---

# ElevenLabs v3 Voice — Reference Notes (partial stub)

Source material for this skill is limited in the current corpus. The notes below capture what IS known.

## What's Known

### Tool Quick Pick

| Tool | Best For |
|------|----------|
| **Minimax** | Easy + fast. $9/mo. Natural cadence. Clone with 5s clip. |
| **ElevenLabs v3** | Best quality. Use Voice Design with "good quality" descriptor for "in the room" realism. |
| **Resemble AI** | Fix robotic AI audio. Chatterbox models. Extract → Adobe Podcast → Resemble. |
| **Qwen3-TTS** | Free, local, offline. Great for short clips. |

### "Good Quality" Descriptor

In ElevenLabs Voice Design, adding the phrase "good quality" to the voice descriptor produces an "in the room" realism effect. This is a documented convention.

### Voice ID Reference Method (Higgsfield native)

Instead of generating voice separately in ElevenLabs:
1. Extract audio from a reference video
2. Upload to Higgsfield as `@Audio1`
3. Prompt: `Use @Audio1 as voice ID reference for her voice. Do not make her say the words in the audio, just use it as a voice ID.`

### Pipeline Position

1. Generate VO in ElevenLabs v3 BEFORE rendering video
2. Export as MP3
3. Upload to Higgsfield as `@Audio1`
4. For chains >15s: chunk on natural breath pauses aligned to clip boundaries
5. In final post-production: use the full-length VO over the edit; the chunked `@Audio1` was only for per-clip lip-sync reference

### Robotic Audio Fix Pipeline

For voices that come out robotic / too-AI:
Extract → Adobe Podcast (denoise / enhance) → Resemble AI (Chatterbox models) → polished output

## What's NOT Known (TODO)

- Full Voice Design prompt format / input fields
- Instant Clone workflow (sample length, tuning)
- Image-to-voice feature details
- Specific Voice Design "descriptor" vocabulary beyond "good quality"
- Pricing tiers

## See Also

- [[Skills/seedanceX/notes|Seedance 2.0 Prompting]] — primary downstream consumer of VO
- [[Skills/kling-video/SKILL|Kling 3.0 Prompting]] — has native audio, but ElevenLabs wins on quality
