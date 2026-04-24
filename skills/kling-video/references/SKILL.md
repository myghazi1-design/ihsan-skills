---
name: kling-video
description: Use when the user asks to write, generate, or improve a Kling 3.0 (Kuaishou) AI video generation prompt. Enforces the Five-Layer Formula (Scene → Characters → Action → Camera → Audio), substitutes vague lighting for named light sources, formats multi-character dialogue with fine-tuned token syntax, caps multi-shot prompts at 6 shots, and appends scene-appropriate negative prompts. Based on research report at ~/Desktop/My Vault/Research/AI-and-Agents/kling-3-prompt-engineering.md.
version: 1.0.0
tested_on: Kling 3.0 (April 2026)
---

# Kling 3.0 Video Prompt Generator

Auto-generate production-quality Kling 3.0 prompts from user intent. The skill enforces the Five-Layer Formula by construction, substitutes vague language for anchor tokens the model recognizes, and validates platform constraints.

Quick reference: [[kling-3-prompt-formula]] (in `Resources/prompts/`).

## When to Use

Activate this skill when the user:
- Asks to write a Kling 3.0 prompt ("write me a Kling prompt for...")
- Provides a video concept and asks to turn it into a generation prompt
- Has an existing prompt and wants it improved or restructured
- Asks about Kling 3.0 syntax, dialogue format, or multi-shot structure
- Mentions Kling AI, Kuaishou video generation, or `kling-v3` / `kling-v3-omni`

Do NOT activate for:
- Other video models (Runway, Sora, Veo, Pika) — their prompt formats differ
- Still image generation — use image-specific tools
- Video editing or post-production questions

## How to Use

### Step 1 — Classify the prompt type

From user intent, determine which mode to use:

| Mode | Trigger | Output |
|---|---|---|
| **Quick** | Simple 5s clip, no dialogue | Layers 1–4 + negatives |
| **Full** | Narrative scene, 10–15s | All 5 layers + negatives |
| **Dialogue** | Spoken lines mentioned | All layers + `[Character:]` format |
| **Multi-shot** | "multiple shots", "storyboard", "sequence" | Master Prompt + numbered shots (max 6) |
| **Image-to-video** | User provides source image | Animate + preserve instructions |

### Step 2 — Parse user intent into the five layers

Extract or ask for each layer. If the user gives a vague description, ask targeted questions — one per missing layer, batched in a single turn.

| Layer | What to extract |
|---|---|
| **Scene** | Location, time of day, atmosphere, named light sources |
| **Characters** | Identity, physical description, clothing, objects they interact with |
| **Action** | Physical mechanics, temporal arc (beginning → middle → end) |
| **Camera** | Shot size, movement, angle, style |
| **Audio** | Dialogue (if any), ambient sounds, music, SFX |

### Step 3 — Apply the enforcement rules

See `references/enforcement-rules.md` for the full list. The critical rules:

1. **Named light sources only** — Reject "dramatic lighting", "cinematic lighting", "moody lighting". Substitute with specific sources from `references/light-sources.md`.
2. **Named camera movements** — Reject "camera moves closer", "camera follows". Substitute with terms from `references/camera-vocabulary.md`.
3. **Physics-over-action for motion** — When the user describes human motion, translate action verbs into physical mechanics. See `references/physics-translations.md`. **Mark as [Test first] in output if user hasn't validated on their setup.**
4. **Anchor hands to objects** — If hands appear in the shot, require an anchor object (cup, railing, device).
5. **Character label consistency** — For dialogue or multi-character, use `[Character A: Role, tone]:` format throughout.
6. **Multi-shot cap at 6** — Hard constraint from kling.ai docs. Refuse to output 7+ shots.
7. **Negative prompt budget 5–7 items** — Keep targeted, not exhaustive.

### Step 4 — Output the prompt in the correct format

Emit the prompt using one of the templates in `references/prompt-template.md`. Label confidence tiers on any claim-dependent guidance the user should validate.

### Step 5 — Attach a self-test suggestion (first use only)

If this is the user's first Kling prompt in the conversation, append a short note with 2–3 things they should A/B test on their setup before committing to defaults:

> First-use note: this prompt uses the Five-Layer Formula (verified across 66 production prompts) plus physics-over-action descriptions (single-source, mechanistically plausible — test yourself). To validate, generate this prompt once as-is, then once with the physics descriptions replaced by generic verbs, and compare motion quality.

---

## Confidence Tiering

Every claim in this skill is tagged. When generating output, propagate the relevant tier labels where the user would benefit from knowing confidence:

- **[Verified]** — First-party kling.ai documentation. Platform constraints.
- **[High]** — Mechanistic backing + convergence across 66 production prompts.
- **[Medium]** — Single-source practitioner observation, mechanistically plausible.
- **[Test first]** — Worth validating on user's own setup before treating as default.

## Reference Files

The skill's knowledge base lives in `references/`:

- `prompt-template.md` — Fill-in-the-blank templates for each mode
- `enforcement-rules.md` — Full list of substitution and validation rules
- `light-sources.md` — Named light source vocabulary with visual character
- `camera-vocabulary.md` — Named camera movements and what they produce
- `physics-translations.md` — Action verb → physics mechanics translations
- `negative-prompts.md` — Baseline + scene-specific negative prompt patterns
- `dialogue-syntax.md` — Character labels, tone tags, temporal keywords
- `production-prompts/` — 66 verbatim reference prompts from cliprise + imagine.art

## Examples

### Example 1: Quick mode

**User:** "Write me a Kling prompt for a woman walking through Tokyo at night."

**Skill output:**

```
Narrow Tokyo alley at 2 AM. Steam rises from manhole grates. Flickering
neon signs cast magenta and cyan across wet pavement. Rain just stopped.

A woman in a red trench coat walks toward the camera. Hands in pockets.
Heels clicking on wet cobblestone with each step landing heel-first,
weight rolling forward through the ball of the foot.

Handheld shoulder-cam drifts with her at walking pace, subtle sway.
35mm lens, shallow depth of field. Neon reflections sharp in foreground,
distant alley falling to soft bokeh.

Audio: Distant city hum, rain dripping from awnings, her heels on stone,
faint synth music from a nearby bar.

Negative: sliding feet, distorted face, morphing limbs, floating hands,
flickering textures, cartoonish.
```

**Notes to user:**
- Layer order: Scene → Character → Action → Camera → Audio → Negative ✓
- Named light source: "flickering neon signs" (not "dramatic lighting")
- Physics-over-action: "heel-first, weight rolling forward through the ball of the foot" [Medium — test if this noticeably improves motion for your use case]
- Anchored hands: "hands in pockets" (stable, won't deform)
- Negative prompt: 6 targeted items

### Example 2: Multi-shot mode

**User:** "I want a 15-second product ad for a luxury watch, three shots."

**Skill output:**

```
Master Prompt: Premium luxury watch showcase with dramatic studio lighting
and intimate product detail reveal.

Shot 1 (0-5s): Extreme macro close-up of watch face. Sapphire crystal
catches a single spotlight from above. Slow dolly push from 20cm to 5cm
over 5 seconds. Gear teeth and dial markers in crisp focus. Brushed metal
reflection. Background falls to deep shadow.

Shot 2 (5-10s): Cut to medium shot of watch on matte black velvet pedestal.
Camera performs slow 180° orbit. Key light from upper right, rim light
from behind. Shallow depth of field, velvet texture visible.

Shot 3 (10-15s): Wide shot pulling back to reveal the watch in context —
a minimalist dark display case. Single overhead pendant light. Camera
cranes upward as it pulls back.

Audio: Deep ambient tone, subtle mechanical ticking throughout, soft
orchestral swell at 10s mark.

Negative: lens flare, reflections on crystal, blurry gear details,
warped text on dial, moving background, extra hands.
```

**Notes to user:**
- Multi-shot structure: 3 shots, within the 6-shot cap [Verified]
- Time-coded: each shot has duration in parentheses
- Named camera movements: dolly push, 180° orbit, crane pull-back
- Named light sources: spotlight, key light, rim light, pendant light

### Example 3: Dialogue mode

**User:** "Two friends reuniting on a park bench. Emotional."

**Skill output:**

```
A quiet park bench in late afternoon golden hour. Birds chirping
softly. Wind rustling through autumn leaves. Distant children laughing.

Two old friends sit side by side on weathered wood.

Shot 1 (0-4s): Wide establishing shot. Golden hour backlighting creates
warm rim light on both figures. Camera slowly pushes in.

Shot 2 (4-8s): Medium shot of [Character A: Old Friend 1, warm nostalgic
voice]: "It's been... what, ten years?"

Immediately, [Character B: Old Friend 2, emotional voice, slight tremor]:
"Too long."

Shot 3 (8-12s): Close-up on Old Friend 1's face. Eyes glistening.
[Old Friend 1, softly speaking]: "I missed you."

Shot 4 (12-15s): Two-shot. Old Friend 2 nods slowly. [Old Friend 2,
whispering]: "Me too."

Audio: Soft acoustic guitar music building gently, wind through leaves,
distant park ambience.

Negative: identity drift between shots, lip-sync offset, morphing faces,
extra people, floating hair.
```

**Notes to user:**
- Dialogue syntax: `[Character X: Role, tone]:` format enforced [High]
- Temporal keyword: "Immediately," sequences speakers [Test first — documented by fal.ai but not first-party confirmed]
- Character labels reused identically: "Old Friend 1", "Old Friend 2"
- Four shots (within 6 cap)

---

## Failure Modes to Watch For

1. **User insists on "dramatic lighting"** — Explain that named sources produce better results and offer 2–3 alternatives. Don't override without explanation.

2. **User wants more than 6 shots** — Refuse with explanation. Suggest splitting into two generations.

3. **User describes action without physics** — Translate in output, flag as [Test first] if it's for human motion.

4. **User provides an existing prompt to "fix"** — Parse it into the five layers, identify which are missing or weak, rewrite applying rules.

5. **User asks "does Kling support X"** — Check `references/platform-specs.md` before answering. Don't guess.

6. **Kling model update** — If user mentions Kling 3.5 or 4.0, flag that templates are version-pinned to 3.0 and may need re-testing.

## Self-Test Protocol (for users)

Before committing to all recommendations as defaults, users should run this 30-minute validation:

1. Generate a scene twice: once with physics descriptors ("heel-to-toe, weight rolling forward"), once with generic verbs ("walks naturally"). Compare motion quality.
2. Generate a dialogue scene twice: once with "Immediately," sequencing, once without. Check if speakers merge.
3. Generate with a 3-item negative prompt and a 15-item one. Check if the long one causes stiffness.
4. Add "tracking camera" to a static prompt and compare motion naturalness.

If results don't match this skill's defaults, update the user's personal overrides in `Skills/kling-video/references/user-overrides.md`.
