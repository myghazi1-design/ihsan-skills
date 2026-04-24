# Kling 3.0 Platform Specs

**Source: kling.ai official developer documentation (first-party, [Verified])**

## Model Variants

| Model ID | Description |
|---|---|
| `kling-v3` | Kling Video 3.0 — text-to-video and image-to-video |
| `kling-v3-omni` | Kling Video 3.0 Omni — adds native audio-visual sync + reference video |
| `kling-image-3-omni` | Kling Image 3.0 Omni — 2K/4K UHD stills |
| `kling-v3-motion-control` | Motion Control — facial consistency across angles |

## Hard Capabilities

| Capability | Value |
|---|---|
| **Max video duration** | 15 seconds (up from 10s in v2.x) |
| **Min video duration** | 3 seconds |
| **Max shots per generation** | 6 scene cuts |
| **Native audio** | Yes (lip sync, dialogue, SFX, BGM differentiation) |
| **Supported languages** | Chinese, English, Japanese, Korean, Spanish + dialects |
| **Image output (Omni)** | 2K / 4K native UHD |
| **Reference video (Omni)** | 3–10 seconds |
| **Modes** | STD and PRO |
| **Start + end frame control** | Yes |
| **Element control** | Video character + multi-image elements |
| **Motion control** | Yes |

## API Pricing (2026)

| Mode | No Audio ($/sec) | With Audio ($/sec) | 15s video cost |
|---|---|---|---|
| STD | $0.084 | $0.126 | $1.26–1.89 |
| PRO | $0.112 | $0.168 | $1.68–2.52 |

Additional services:
- Video-to-audio (3–20s): $0.035 per call
- Text-to-audio (3–10s): $0.035 per call
- Motion Control STD/sec: $0.126
- Motion Control PRO/sec: $0.168

**Billing model:** Prepaid resource packs only. No pay-as-you-go. Failed generations are not charged.

## Feature Differences — v3 vs v2.6

| Feature | v2.6 | v3.0 |
|---|---|---|
| Max duration | 10s | 15s |
| Multi-shot | No | Up to 6 shots |
| Native audio | Voice control only | Full lip sync + multilingual |
| Languages | Chinese, English | + Japanese, Korean, Spanish + dialects |
| Character consistency | Basic | Enhanced, three-person support |
| Text retention (I2V) | Basic | Improved |
| Scene transitions | No | Yes, customizable storyboarding |

## What v3.0 Adds Over v2.x — Official List

From kling.ai docs:
- "Supports subject upload in I2V scenarios for enhanced consistency"
- "Significant improvement in multi-character referencing, especially for three-person scenarios"
- "Supports Japanese, Korean, and Spanish in addition to Chinese and English"
- "Capable of generating certain dialects and accents"
- "Better distinction and control over different types of audio (speech, sound effects, BGM)"
- "Improved text retention in I2V scenarios"
- "Supports scene transitions, with up to 6 shots and customizable storyboarding"
- "Native audio-visual synchronization" (Omni variant)
- "Supports video subject creation" (Omni variant)
- "Extended generation duration up to 15 seconds"

## API Access

- REST API at `kling.ai`
- JWT authentication
- Prepaid credit packs (video, image, virtual try-on categories)
- Rate limits documented in developer portal
- Callback webhooks for async generation

## Aspect Ratios

| Ratio | Use |
|---|---|
| 16:9 | Standard widescreen (YouTube, TV) |
| 9:16 | Vertical (TikTok, Instagram Reels, Stories) |
| 1:1 | Square (Instagram feed) |
| 4:5 | Portrait (Instagram) |

## Version History

- **v1.0 / v1.5 / v1.6** — 720p/1080p, 5s or 10s, 30fps
- **v2.0 Master** — 720p 24fps, 5s/10s, premium quality tier
- **v2.1** — 1080p, improved quality
- **v2.5 Turbo** — Fast inference, lower cost
- **v2.6** — Voice control, motion control, 10s max
- **v3.0** — Current. Multi-shot, native audio, 15s max, multilingual

## What's NOT Supported

Clarifications to avoid false promises:

- **No infinite video** — Max is 15 seconds
- **No more than 6 shots** — Hard cap
- **No real-time generation** — Async with callback
- **No training custom models** — Use element/character references instead
- **No 8K output** — 4K is the ceiling for images; video tops out at 1080p
- **No pay-as-you-go** — Must prepay into resource packs
