---
type: skill-reference
skill: voice-elevenlabs
topic: voice-id-reference
---

# Voice ID Reference Method (Higgsfield native)

An alternative to ElevenLabs voice generation when you want a specific voice quality from a reference video rather than designing one from scratch.

## The Method

1. **Find a reference video** with the exact voice quality you want (YouTube, Instagram, etc.)
2. **Extract the audio** — any audio extraction tool (yt-dlp with audio-only option, or just rip the video locally)
3. **Upload to Higgsfield as `@Audio1`** — trimmed to ≤15s
4. **Prompt with this exact incantation:**

   ```
   Use @Audio1 as voice ID reference for her voice.
   Do not make her say the words in the audio, just use it as a voice ID.
   ```

5. **Provide the actual dialogue in your Mode 2 timestamp segments** — the model replicates the reference voice's timbre/tone while saying YOUR script

## When to Use This vs Full ElevenLabs

| Situation | Use |
|---|---|
| You have a target voice in mind (e.g., "like this TikTok creator") | Voice ID Reference |
| You want to design a new voice | ElevenLabs Voice Design |
| You're chaining 30+ seconds | ElevenLabs (for chain consistency) |
| Quick 8-15s demo | Voice ID Reference (faster) |
| Multi-language | ElevenLabs (supports explicit language tagging) |

## Caveats

- This is a Higgsfield-internal workaround; voice consistency across clips may drift
- For chains >15s, still pre-gen the VO in ElevenLabs as `@Audio1` so consistency is locked
- The reference audio has to be ≤15s (Higgsfield input cap)

## Incantation Variations

If the default prompt doesn't work, try:

```
@Audio1 — use only as a voice reference. She should say the words
provided in the dialogue segments, but with @Audio1's vocal quality.
```

Or explicitly block verbatim playback:

```
Use @Audio1 as voice timbre reference ONLY. Do not speak the lines
from @Audio1. Speak only the lines in the Dialogue: sections below.
```
