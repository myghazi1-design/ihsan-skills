---
type: skill-reference
skill: seedanceX
domain: video-production
created: 2026-04-10
updated: 2026-04-20
status: locked
tags: [seedance, prompting, video-production, higgsfield]
---

> [!important] Status — LOCKED (2026-04-14) · Extensions Applied (2026-04-18) · Ported (2026-04-20)
> `/seedanceX` won the head-to-head against `/mikos` on Seedance 2.0. 8s + 15s renders passed audience test. The 30-day structural-change lock is preserved as optional discipline.
>
> **Extensions applied 2026-04-18 (additive, lock honored):**
> 1. Shot-structure-first header for Mode 1 multi-shot variants
> 2. Global Context Block — required prefix for multi-segment / chained prompts
> 3. Inline `[VFX: …]` bracket syntax for effects content
> 4. POV / Orbs negative-camera constraint: "No cuts, no zoom, natural head movement"
> 5. Mode 2 timestamps migrated to `[0:00-0:04]` bracket format (Seedance-2.0-trained convention)
> 6. Frame-Lock Chaining Protocol SUPERSEDED 2026-04-18 — use Master-Reference-Reuse (re-upload NBP master asset every clip)
> 7. Five Optimized Formats reference table (Higgsfield)
>
> **Pipeline position:** [[Skills/nbp/notes|/nbp]] (4K reference image) → this skill (Seedance render) → CapCut → Topaz Proteus 2x → Suno → Captions.

# Seedance 2.0 Prompting

**Platform:** Higgsfield
**Model:** Seedance 2.0 (ByteDance)
**Character limit:** 2,000 characters per prompt (bypass method below)

---

## Pre-Production: Starting Images (Nano Banana Pro)

Your starting image is the foundation. If it looks like AI slop, your video will look like AI slop.

### The JSON Method (10x better than text-only prompts)

1. **Find a reference image on Pinterest** — search for the aesthetic you want, save the image that matches your style
2. **Convert to JSON** — upload the image to Gemini 3.0 Pro and ask: "Create a detailed JSON to recreate this image." Copy the JSON output.
3. **Use JSON as base for your image** — paste the JSON + add your prompt. Example: `"Use this JSON as reference: [JSON] Now generate: woman holding my product on beach."` Upload your product image if needed.
4. **Generate in AI.Studio** (best platform for Nano Banana Pro) — paste your JSON + prompt and generate.

**Why this works:** The JSON handles realism + color grading. Your prompt handles the actual content. This prevents the standard grey AI look that comes from text-only prompts.

**Alternative method:** Find reference content on TikTok in your niche. Screenshot a good frame, put it into Claude or Gemini, ask it to create a JSON prompt to recreate that image. Then adjust the prompt for your own unique character — different clothes, different background. Your starting frames will already look like content that performs in that niche.

See also: [[Skills/nbp/notes|Nano Banana Pro Prompting]]

---

## Pre-Production: Realistic AI Voices

Generate your voiceover BEFORE you generate your video. You need the audio file to upload as a reference in Higgsfield.

### Voice Tool Quick Pick

| Tool | Best For |
|------|----------|
| **Minimax** | Easy + fast. $9/mo. Natural cadence. Clone with 5s clip. |
| **ElevenLabs v3** | Best quality. Use Voice Design with "good quality" descriptor for "in the room" realism. |
| **Resemble AI** | Fix robotic AI audio. Chatterbox models. Extract → Adobe Podcast → Resemble. |
| **Qwen3-TTS** | Free, local, offline. Great for short clips. |

For the full ElevenLabs workflow (Voice Design prompting, image-to-voice, Instant Clone): see [[Skills/voice-elevenlabs/notes|ElevenLabs Voice Design]].

### Voice ID Reference Method (Higgsfield Native)

Instead of generating a voice separately, find a video with the exact voice quality you want:
1. Extract that audio
2. Upload to Higgsfield as `@Audio1`
3. Prompt: "Use @Audio1 as voice ID reference for her voice. Do not make her say the words in the audio, just use it as a voice ID."

The model replicates that voice for your custom dialogue while keeping the natural tone.

---

## Platform Specs

- Up to 9 images as input
- Up to 3 videos (15s max total)
- Up to 3 audio files (15s max MP3)
- 4-15 second output duration per generation
- Native 2K resolution
- Generates native sound effects and music
- 12 files max per generation
- 2,000 character prompt limit

ByteDance API inline flags (from official docs):
- `--duration` — explicit duration specifier
- `--resolution` — resolution control
- `--camerafixed` — locks camera to fixed framing
- `Camera switch.` — multi-shot connector phrase

---

## When to Use Seedance vs Kling

| Use Case | Seedance 2.0 | Kling 3.0 |
|----------|-------------|-----------|
| Action / explosions / debris | **Best choice** | Decent |
| Dynamic camera movement | **Best choice** | Good |
| Animals / creatures / fantasy | **Best choice** | Decent |
| Environmental storytelling | **Best choice** | Good |
| UGC with timestamp method | **Best choice** | Good |
| Wide shots (no face detail) | **Best choice** | Good |
| Talking head / dialogue | Decent with timestamps | **Best choice** |
| Static / locked camera | Avoid (micro-tilt bug) | **Best choice** |
| Literal camera instructions | Avoid (reinterprets) | **Best choice** |
| 3+ characters | Avoid (face warp) | Better but still risky |
| Small object detail | Avoid | Better |
| Lip sync precision | Decent | **Best choice** |

**The routing rule:** Seedance *reinterprets* camera instructions through genre conventions from its ByteDance/Douyin training data. Kling follows instructions literally. Write Seedance prompts as scene descriptions. Write Kling prompts as shot instructions.

---

## Global Context Block — required for multi-segment or chained prompts

A 1–3 sentence **prefix** describing environment + mood + lighting + quality. Placed before the 6-step formula begins. **Identical text across every clip in a chain** — this is what anchors palette/atmosphere against drift.

**Example:**
```
Scene: A rain-soaked city street at night, neon reflections on wet
pavement, cinematic style, high contrast lighting, 4K quality.
```

Applies to: Mode 2 multi-segment timestamp prompts, multi-clip chaining (all clips identical), and any Mode 1 multi-shot montage.

Not a new layer inside the 6-step formula — it precedes it. MindStudio research identifies "skipping global context" as the primary cause of color/style drift between segments.

---

## Two Prompting Modes

### Mode 1: Cinematic / Action

For action scenes, B-roll, environmental shots, creatures, wide shots.

**Target:** 60-80 words. Shorter and cleaner beats longer and over-specified.

**The 6-Step Formula:**
```
[Subject] + [Action] + [Environment] + [Camera Movement] + [Style/Lighting] + [Constraints]
```

Subject first pins the center of gravity. Camera second locks framing. This order reduces contradictions.

**Rules:**
- ONE camera movement per shot. Multiple = jitter.
- ONE clear action per beat. Describe the SCENE, not puppet instructions.
- Named light source. Not "cinematic lighting."
- 2 characters max. 3+ = face warp, extra limbs.
- Genre-anchor when useful ("documentary realism", "spy thriller style").
- End with "No music."
- 4-8 second duration.

#### Mode 1 Multi-Shot — Shot-Structure-First Header

When the prompt contains ≥2 shots in a single 15s generation, **open** with this header and **close** with the duration summary. Only for multi-shot Mode 1 — NOT single-shot Mode 1, NOT any Mode 2 variant.

**Opening (required):**
```
Montage, multi-shot action Hollywood movie, don't use one camera angle
or single cut, cinematic lighting, photorealistic, 35mm film quality,
ARRI ALEXA aesthetic.
```

**Closing (required):**
```
Total: 15s / N shots / 16:9
```

> **Contradiction flagged:** The closing duration line contradicts the "Do not write duration in the prompt" rule documented in the Duration Guide section. Treat as Mode-1-multi-shot-only exception. 2-render A/B test pending — same prompt with and without the header block — will resolve empirically.

Source: Higgsfield's April 2026 format library opens every multi-shot cinematic prompt with this exact header.

### Mode 2: UGC / Dialogue (Timestamp Method)

For talking heads, product demos, branded content, AI UGC.

Break the video into **4-5 second chunks** using `[0:00-0:04]` bracket format. Specify exact dialogue and exact visuals for each chunk. Seedance 2.0 was trained on this specific bracket+colon convention — use it, not `0-4 seconds:` or `[0-4s]`.

**Example structure:**
```
Scene: A bright morning kitchen with natural light from a large window,
warm white tones, crisp 4K quality.

@Image1 is the starting frame of the video. @Image2 is how the coffee bag looks.
For dialogue and audio reference, use @Audio1.

[0:00-0:04] Introduction.
Dialogue: "I just made a peanut butter cup protein coffee with 25 grams of protein."
Visuals: Subject holds a clear round glass jar filled with light brown coffee.
Left hand holds the glass while right hand stirs the drink.

[0:04-0:12] Benefits.
Dialogue: "Getting 20 to 30 grams of protein in the morning really helps so much."
Visuals: Subject sets down the glass, picks up the protein bag from @Image2,
holds it toward camera while speaking.

No music.
```

**Why this works:** Seedance follows detailed per-second instructions better than any other model for UGC content. ByteDance trained it on TikTok data — it understands short-form pacing, cuts, and transitions natively.

**4-Layer UGC structure:**
1. Global Context (scene, visual style, lighting, quality descriptors)
2. Timestamp Segments (2-5s each, 4-6 per generation, `[M:SS-M:SS]` format)
3. Camera Direction (shot type + movement + speed, ONE instruction per segment)
4. Subject Action (physical/concrete/observable, no emotional abstractions)

---

## Inline [VFX: …] Bracket Syntax

For powers, effects, magic, elemental phenomena — especially Orbs, Transformations, and hero-VFX moments. Placed inside the Action layer (or inside a Mode 2 timestamp segment body).

**Syntax:**
```
[VFX: branching electric circuits pulsing with white-blue current]
[VFX: fire ignites along her forearm, orange core fading to amber edge]
[VFX: glass fractures in slow motion, shards suspended in mid-air]
```

**Usage rule:** One primary `[VFX: …]` bracket per shot beat. Single-shot = one bracket. Multi-beat VFX = one bracket per Mode 2 timestamp segment — NOT stacked inside one Action layer.

Keeps the shot description clean (subject / action / camera) while routing VFX instruction to a dedicated syntactic slot. Higgsfield's format library uses this convention explicitly for the Orbs and Transformations formats.

> **Contradiction flagged:** Test-first. Skeptical review suggests possible cargo-cult convention; triangulated evidence supports usage. Run one Orbs-format test before adopting as default.

---

## POV / Orbs — Required Negative-Camera Constraint

For first-person / camera-is-eyes formats (Orbs, POV formats from Higgsfield's format library), append this exact string to the constraints:

```
No cuts, no zoom, natural head movement.
```

Without this, Seedance inserts cuts (ByteDance training bias toward short-form edits) and zooms (general cinematic bias). The three-phrase constraint suppresses both and locks natural head-sway.

---

## The @ System

Every uploaded asset MUST be tagged with its role. Untagged files get misused — headshots become backgrounds, style refs become subjects.

| Tag | Role | Example |
|-----|------|---------|
| `@Image1` | Starting frame | `@Image1 is the starting frame of the video` |
| `@Image2` | Product / prop | `@Image2 is how the coffee bag looks` |
| `@Audio1` | Voiceover or voice ID | `@Audio1 is the voiceover for the dialogue` |
| `@Video1` | Previous clip (for extensions) | `Extend @Video1. Keep the voice the same.` |

---

## Multi-Clip Chaining Protocol (corrected 2026-04-18)

> [!warning] Frame-Lock exit-frame chaining is SUPERSEDED
> The earlier protocol used Higgsfield Start & End Frames to pin Clip A's final frame as Clip B's starting frame. Three practitioner sources + render-validated observation confirm this causes quality degradation because Seedance re-processes an already-AI-generated frame. The correct canonical protocol is **Master-Reference-Reuse**: re-upload the Nano Banana Pro master asset on every clip. The legacy frame-lock section is preserved as an appendix.

Seedance generates up to 15 seconds per clip. For videos longer than 15s, use this protocol:

### The Master-Reference-Reuse Protocol (canonical, 2026-04-18)

1. **Anchor the master reference.** Generate the NBP 4K master reference image (character + setting + style). This is the identity anchor.
2. **Anchor the audio first.** Generate the full voiceover in ElevenLabs V3 (or Higgsfield Audio). Export as MP3. This becomes `@Audio1` — shared constant across every clip. 15s audio input cap means pre-chunk VO on natural breath pauses aligned to clip boundaries.
3. **Anchor the global context.** Write the 1–3 sentence global context block (see Global Context Block above). Identical text across every prompt in the chain.
4. **For every clip, re-upload the NBP master reference as `@Image1`.** Do NOT chain exit-frame as start-frame — that compounds AI drift. Instead, each new clip starts fresh from the original NBP master.
5. **Lock lighting + temperature language.** Use fixed values ("warm 3200K") not ranges ("warm-ish"). Repeat lighting descriptors in every clip.
6. **Design seams around B-roll / product / environmental frames.** Dialogue-at-seams causes lip-sync failure; keep dialogue inside a single clip.

### Empirical ceiling

| Chain length | Status |
|---|---|
| 2 clips (~24s) | Reliable — basic workflow proven |
| 3 clips (~36s) | Reliable with global context + `@Audio1` + master-reference-reuse |
| 4 clips (~48s) | Coin flip — requires validation render |
| 5+ clips | Compounding drift visible to audience eye |

> **Contradiction flagged:** Test-first. Skeptical review places ceiling at 2-3 clips; optimist places it at 3-4 with mitigations. Validation render required before declaring 3-clip "locked."

### What master-reference-reuse does NOT solve

| Failure mode | Mitigation |
|---|---|
| Lip sync at clip boundaries (dialogue UGC) | Use Mode 2 within single clip; keep mouth off-camera at seams |
| Mid-clip physics re-inference (skin tone, fabric, lighting temp) | Repeat lighting descriptors in every segment; fixed temperature language |
| Cross-clip world-state reset (crowd density, window light angle) | Design shots around B-roll / environmental / product frames |
| 15s audio input cap | Pre-chunk VO on natural breath pauses aligned to clip boundaries |
| "Keep voice the same" soft instruction | No enforcement — needs external pre-gen audio anchor |

### Basic extension (2-clip simple cases only)

For 2-clip chains where you don't need cross-clip identity anchoring, the `@Video1` method still works:

1. Generate first clip (up to 15s)
2. Download the clip
3. Upload it back to Higgsfield as `@Video1`
4. Prompt: `Extend @Video1. Keep the voice the same as the original clip.`
5. Add new product images or assets as needed
6. Specify what happens in the next 15 seconds using the timestamp method

For anything longer than 2 clips, or for voice/palette-sensitive chains, use the Master-Reference-Reuse protocol.

### Appendix: Legacy Frame-Lock Protocol (SUPERSEDED — kept for historical context only)

The earlier protocol used Higgsfield Start & End Frames:
1. Anchor audio first (`@Audio1` shared constant)
2. Anchor global context (identical per clip)
3. Generate Clip A with clean static exit frame (2-3s anchor-frame technique)
4. Export End frame of Clip A → Start frame of Clip B via Higgsfield Start & End Frames

This is the superseded method. Do not use for new work.

---

## The Five Optimized Formats (Higgsfield)

Higgsfield's format library documents five recurring format shapes on Seedance 2.0. Pick a format before writing — each has a specific structure, input spec, and signature "move."

| Format | Structure | Inputs | Signature |
|---|---|---|---|
| **Transformations** (highest-performing) | 6 shots / 15s / 16:9 | Image × 3–4 | Escalation arc: calm → threat → transformation → aftermath |
| **Orbs (POV)** | 1 shot / 15s / 16:9 | Image × 1 | "The camera IS her eyes" + inline `[VFX: …]` + "No cuts, no zoom, natural head movement" |
| **POV** | 1 shot / 15s / 16:9 | Image × 1 | Must include: "No cuts, no zoom, natural head movement" |
| **Fights** | 1 shot / 15s / 16:9 | Image × 2 | Location + power mismatch + escalation arc + beat-by-beat action |
| **Animation** | 1 shot / 15s / 16:9 | Image × 1 (keyframe) | Break into 0-3s, 3-6s, 6-9s, 9-12s, 12-15s; describe particle physics |

**Routing tip:** Transformations is the highest-performing format on Higgsfield's leaderboard. When the concept allows it (subject undergoes visible change), bias toward Transformations over single-shot.

---

## Canva Character Limit Bypass

Seedance has a 2,000 character prompt limit. To bypass:

1. Create an image in Canva with your full detailed prompt as text
2. Include all movements, timestamps, dialogue — everything
3. Upload that image to Higgsfield as a reference
4. In your actual prompt: "Transcribe this image and use the movements described here."

Seedance reads the text in the reference image and follows those instructions. Works for any long prompt situation.

**Caveat:** Depends on emergent OCR, not a documented feature. Can break silently on platform update.

---

## Camera Movement Vocabulary

**ONE movement per shot.** Compound moves must be sequential with clear transitions.

| Movement | Term | Notes |
|----------|------|-------|
| Push-in | `dolly in` | Most reliable movement |
| Pull-out | `dolly out` | Good for reveals |
| Pan | `pan left/right` | Horizontal sweep |
| Tracking | `follow shot` | Pursues moving subject |
| Orbit | `arc` | Rotation around subject |
| Aerial | `drone shot` | High-altitude perspective |
| Handheld | `handheld` | Natural shake; degrades past 8s |
| Static | `locked tripod` | Unreliable — adds micro-tilts. Use Kling instead. |

Use **rig words** (dolly, track, crane, handheld, gimbal) not **mood words** (dynamic, flowing, smooth).

Use **pacing words** for motion quality: slow, smooth, gentle, gradual. These improve output.

Do NOT specify: fps, ISO, or exact focal lengths. Use lens buckets (wide / normal / telephoto). Exception: "24 fps feel" as a temporal cadence cue does help.

---

## Lighting

**Highest leverage single element.** If you can only add one detail, add lighting.

| Type | Phrase |
|------|--------|
| Golden hour | "warm golden hour sunlight" |
| Overcast | "soft diffused cloud light, no harsh shadows" |
| Blue hour | "blue twilight, cool tones" |
| Studio | "soft box light from the left, fill from the right" |
| Hard contrast | "hard chiaroscuro, deep shadows" |
| Neon/urban | "neon reflections on wet pavement" |
| Window | "soft natural window light from the right" |
| Night ops | "faint window spill, moonlight, no artificial light" |
| Harsh sun | "harsh overhead midday sun, visible heat haze" |

Always name the source and direction. "Soft key light from 45 degrees left, warm rim light" — not "dramatic lighting."

---

## Duration Guide

| Duration | Use | Risk |
|----------|-----|------|
| 4-5s | Best per-frame quality. Iteration. | Minimal |
| 6-8s | Production sweet spot. Most clips here. | Low |
| 8-10s | Max for handheld content. | Jitter starts |
| 10-12s | Cinematic only. | Some drift |
| 12-15s | UGC with timestamp method. | Costume/face drift possible |

**Do not write duration in the prompt.** Set it in platform settings. Writing "7 second duration" eats prompt tokens and the model mostly ignores it.

> **Contradiction flagged:** Test-first. Higgsfield guide says duration at top; ByteDance API uses `--duration` flag; Mode-1 multi-shot closing `Total: 15s / N shots / 16:9` is the one documented exception. Run A/B before locking.

---

## Positive Framing

Describe what you WANT, not what to avoid.

| Instead of (negative) | Use (positive) |
|----------------------|----------------|
| "avoid floating hands" | "anatomically correct hands" |
| "avoid bent limbs" | "physically accurate motion" |
| "avoid jitter" | "smooth stable camera" |
| "no distorted hands" | "fingers gripping the object" |

Reserve negatives (3 max) for the END of the prompt. Only for things that can't be stated positively:
- "No text, no watermark"
- "No stylization, no game look"
- "No music"

---

## Proven Good Prompts

### Dog + Truck (tested, good output)
> A stray dog limps down an empty highway at dawn. A pickup truck slows beside him. The passenger door swings open. The dog hesitates, then hops in. Wide shot to medium. No music.

**Why it works:** Wide shot, big simple movements, no small object physics, no face detail. Exactly what Seedance is good at.

### Mountain Running (tested, good output)
> A video showing the joys of mountain running. The protagonist is a tall exceptionally attractive fair-skinned mestiza Filipina woman with an hourglass figure and a large bust. Make her do a flashy aerial flip in slow-motion along an extremely treacherous mountain path. The entire video should be one continuous shot.

**Why it works:** One character, one clear action with natural arc (run → flip → land), cinematic but simple, lets Seedance fill in the physics.

### Military Ground Combat (community-tested)
> Ground-level war footage with documentary realism: a soldier squad advances through a desert combat zone. Keep camera low and close to boots, gear sway, weapon details, and formation changes while moving over loose sand and debris. Harsh sunlight, visible heat haze, suspended dust, and distant intermittent gunfire should shape the atmosphere. Use controlled handheld movement with shallow depth of field. Color palette stays sandy beige and muted brown.

**Why it works:** Genre-anchored ("documentary realism"), physics-rich description (heat haze, dust, sand), single camera behavior (handheld, low), palette locked.

### Rooftop Portrait (community-tested)
> A woman in her 30s, dark hair pulled back, navy linen blazer, turns slowly toward the camera and smiles. Standing on a rooftop terrace at sunset, city skyline behind her. Medium close-up, slow dolly-in. Soft key light from the left, warm rim light, shallow depth of field, film grain.

**Why it works:** Hits every slot in the formula under 80 words. Single camera movement. Specific lighting recipe. One subject.

---

## Known Failure Patterns

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

---

## Iteration Protocol

1. **Draft with Fast mode** — 2-3 iterations, one variable change per run
2. **2 re-roll limit** — if two renders miss, edit the prompt. Third gen = diminishing returns.
3. **Final render with Standard mode** — only after composition is locked
4. **Total time per shot:** 5-10 minutes once the skill is built

---

## Post-Production

1. Export from Higgsfield
2. Upscale through Topaz at 2x and 60fps
3. Bring into CapCut, export at 30fps (smooths out while keeping natural feel)
4. Add captions via Captions.ai or CapCut
5. Add music via Suno AI (**never bake music into generation**)

---

## Words to Never Use

- "cinematic" alone
- "epic" / "amazing" / "beautiful"
- "fast" (unqualified)
- "lots of movement"
- "dynamic" / "flowing"

---

## See Also

- [[Skills/kling-video/SKILL|Kling 3.0 Prompting (Higgsfield)]]
- [[Skills/nbp/notes|Nano Banana Pro Prompting]]
- [[Skills/NBPHero/notes|NBPHero — product hero specialist]]
- [[Skills/voice-elevenlabs/notes|ElevenLabs Voice Design]]
- [[Skills/mikos/notes|Mikos / MIKOSLAB V2]] — lost head-to-head on Seedance rendering; kept for reference
