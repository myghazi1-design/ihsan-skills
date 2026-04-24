---
name: seedanceX
description: Use when the user asks to write, improve, or audit a Seedance 2.0 video prompt on Higgsfield — cinematic shots, action, UGC-timestamp dialogue videos, POV/Orbs/Transformations/Fights/Animation formats, multi-clip chains beyond 15s, or any "/seedance" / "/seedanceX" request. Also use when the user mentions ByteDance Seedance, 8-second cinematic clip, timestamp-bracket UGC video, or Higgsfield Start & End Frames chaining.
version: 2.0.0
tested_on: Seedance 2.0 on Higgsfield (April 2026)
---

# Seedance 2.0 Prompt Builder (Unified)

Generate correct Seedance 2.0 prompts on Higgsfield — one skill, multiple modes. Resolves the locked `/seedanceX` spec from `notes.md`. Do NOT split this into separate skills; the mode router lives inside this skill.

## When to Use

Activate when the user:
- Says "/seedance", "/seedanceX", "seedance prompt", "write a seedance prompt", "8 second cinematic clip"
- Requests action, explosions, dynamic camera, animals/creatures, environmental storytelling, wide shots
- Requests UGC with timestamp method, product demo with dialogue broken by timestamps
- Asks for Transformations, Orbs, POV, Fights, or Animation format (Higgsfield's Five Optimized Formats)
- Wants to chain clips past 15s (multi-clip long-form)

Do NOT activate for:
- Talking head / static-camera dialogue → route to `kling-video`
- Still-image generation → route to `nbp` or `NBPHero`
- UGC creative strategy (hooks, script beats) → `ugc` or `ugc-creator-setup` (Overseer B's scope)

## Mode Router — pick ONE before writing

| Mode | When to use | Output shape |
|---|---|---|
| **Mode 1 Cinematic / Action** | Action, B-roll, creatures, environment, wide shots | 6-step formula, 60-80 words, one camera move |
| **Mode 1 Multi-Shot** | ≥2 shots in one 15s generation | Shot-structure-first header + shots + closing `Total: 15s / N shots / 16:9` |
| **Mode 2 UGC / Dialogue** | Timestamped dialogue, product demo, branded UGC | Global Context Block + `[0:00-0:04]` timestamp segments |
| **Format: Transformations** (preferred when subject changes) | Subject undergoes visible change across 6 shots | 6 shots / 15s / 16:9, 3-4 images, escalation arc |
| **Format: Orbs / POV** | First-person, "camera IS eyes" | 1 shot / 15s, 1 image, inline `[VFX:...]`, "No cuts, no zoom, natural head movement" |
| **Format: Fights** | Two-subject power-mismatch combat | 1 shot / 15s, 2 images, beat-by-beat escalation |
| **Format: Animation** | Keyframe + segmented animation | 1 shot / 15s, 1 keyframe, 0-3s/3-6s/6-9s/9-12s/12-15s beats with particle physics |

**Routing rule (verbatim from notes.md):** "Seedance *reinterprets* camera instructions through genre conventions from its ByteDance/Douyin training data. Kling follows instructions literally. Write Seedance prompts as scene descriptions. Write Kling prompts as shot instructions."

See `references/seedance-vs-kling-routing.md` for the full deny-list of shot types that should route to `kling-video` instead.

## How to Use — step-by-step

### Step 1 — Route or Refuse

Check the Seedance vs Kling table in `references/seedance-vs-kling-routing.md`. If the shot is in the Kling column (talking head / static camera / 3+ characters / lip-sync precision / small object detail / literal camera instructions), stop and suggest `kling-video` instead.

### Step 2 — Pick the Mode

Use the Mode Router table above. Most requests land in Mode 1 Cinematic OR Mode 2 UGC. Multi-shot and the Five Optimized Formats are narrower cases.

### Step 3 — Build the prompt using the right formula

**Mode 1 Cinematic — 6-step formula:**
```
[Subject] + [Action] + [Environment] + [Camera Movement] + [Style/Lighting] + [Constraints]
```
Target 60-80 words. One camera move. 2 characters max. Named light source.

**Mode 1 Multi-Shot — header + shots + closing:**
```
Montage, multi-shot action Hollywood movie, don't use one camera angle
or single cut, cinematic lighting, photorealistic, 35mm film quality,
ARRI ALEXA aesthetic.

[Shot 1 description]
[Shot 2 description]
...

Total: 15s / N shots / 16:9
```

**Mode 2 UGC — Global Context Block + timestamp segments:**
```
Scene: [environment], [visual style], [lighting], [4K quality, DoF]

@Image1 is the starting frame. @Image2 is [the product].
For dialogue and audio reference, use @Audio1.

[0:00-0:04] [beat 1]
Dialogue: "[line]"
Visuals: [physical, observable action]

[0:04-0:12] [beat 2]
Dialogue: "[line]"
Visuals: [physical, observable action]

No music.
```

Bracket format is `[M:SS-M:SS]` — Seedance 2.0 was trained on THIS convention. Do NOT use `0-4 seconds:` or `[0-4s]`.

### Step 4 — Apply required extensions

- **Global Context Block** (1-3 sentences, identical across every multi-segment or chained prompt) — prefixes Mode 2 multi-segment and all chain clips
- **Inline `[VFX: ...]` brackets** — for powers/effects; ONE per shot beat (or one per Mode 2 timestamp segment)
- **POV / Orbs negative-camera constraint** — append `No cuts, no zoom, natural head movement.` to any POV/Orbs format
- **`@` asset tagging** — every uploaded asset MUST be tagged (`@Image1`, `@Image2`, `@Audio1`, `@Video1`)

### Step 5 — Enforce the hard rules

| Rule | Where it lives |
|---|---|
| ONE camera movement per shot (compound = jitter) | All modes |
| 2 characters max (3+ = face warp, extra limbs) | All modes |
| Named light source, not "cinematic lighting" | All modes |
| Physical / observable actions, not emotional abstractions | Mode 2 especially |
| End Mode 1 with "No music." | Mode 1 |
| Mode 2 segments 2-5s each, 4-6 per generation | Mode 2 |
| 60-100 word budget (60-80 for Mode 1 action) | Mode 1 |
| Rhythmic descriptors (slow, smooth, gradual) not technical (fps, f-stop, ISO) | All modes |

See `references/enforcement-rules.md` for the full list with refusal conditions.

### Step 6 — Check banned keywords

Scan the output for banned vocabulary. Replace with specifics from `references/banned-keywords.md`:
- `cinematic` alone → spell out the lighting recipe
- `epic` / `amazing` / `beautiful` → specific visual effect
- `fast` (unqualified) → make only ONE element fast, name it
- `lots of movement` → name the single specific motion
- `dynamic` / `flowing` → rig words (dolly, track, handheld)

### Step 7 — Add negatives (positive framing first, 3 max at end)

Describe what you WANT, not what to avoid. Reserve negatives (max 3) for the END, only for things that can't be stated positively:
- `No text, no watermark`
- `No stylization, no game look`
- `No music`

See `references/negatives.md`.

### Step 8 — For multi-clip chains

If the output exceeds 15s, follow the Master-Reference-Reuse protocol (re-upload the NBP master asset every clip). This is the CORRECTED protocol (2026-04-18) — NOT the older exit-frame chaining which causes quality degradation as Seedance re-processes already-AI-generated frames.

See `references/chaining-protocol.md` for the full protocol, empirical chain ceiling (2 solid / 3 reliable / 4 coin-flip / 5+ visible drift), and what frame-lock does NOT solve.

### Step 9 — Emit output + Notes

Return:
1. The final prompt
2. Word count (flag if over budget)
3. Mode and Format used
4. Which rules / extensions were applied
5. If chain: per-clip prompts + the shared Global Context Block + `@Audio1` plan
6. Post-production reminder: Higgsfield export → Topaz 2x/60fps → CapCut 30fps → Captions → Suno (NEVER bake music into generation)

## Empirical Contradictions — TEST-FIRST items (do NOT lock)

Three items flagged by research as unresolved. Document in output when relevant; do not claim either side:

1. **Duration in prompt** — Higgsfield guide says YES at top; `notes.md` says NO (set in platform); ByteDance API uses `--duration` flag. Mode-1 multi-shot closing `Total: 15s / N shots / 16:9` is the one documented exception. Run an A/B before locking.
2. **`[VFX: ...]` brackets** — Skeptic says marketing cargo cult; Optimist says triangulated. Run one Orbs test.
3. **Chain ceiling** — Skeptic 2-3 clips; Optimist 3-4 with mitigations. Validation render required before declaring 3-clip as "locked."

## Skill lock context

`/seedanceX` won head-to-head vs `/mikos` on 2026-04-14 (8s + 15s renders passed audience test). Additive template extensions applied 2026-04-18. The 30-day structural-change lock is preserved as optional discipline — Yasir can adopt or ignore. The Frame-Lock exit-frame chaining section was SUPERSEDED 2026-04-18 — use Master-Reference-Reuse instead.

## Reference files (read as needed)

Living in `Skills/seedanceX/`:

- `notes.md` — the complete canonical reference (extended from `notes.md` in source corpus)
- `formulas.md` — 6-step formula, Mode 2 structure, five optimized formats
- `enforcement-rules.md` — hard rules + refusal conditions
- `camera-vocabulary.md` — camera move library with per-shot constraint
- `lighting-recipes.md` — named light-source recipes
- `banned-keywords.md` — words-to-never-use → replacements
- `negatives.md` — positive-framing table + 3-max negative list
- `seedance-vs-kling-routing.md` — the full deny-list for routing
- `chaining-protocol.md` — Master-Reference-Reuse (canonical) + legacy frame-lock (SUPERSEDED) + empirical ceiling
- `five-formats.md` — Transformations, Orbs, POV, Fights, Animation signatures
- `verified-examples.md` — Dog+Truck, Mountain Running, Military, Rooftop Portrait (verbatim, tested)
- `failure-patterns.md` — the known failure table + mitigations
- `canva-bypass.md` — 2000-char workaround
- `post-production.md` — Topaz → CapCut → Captions → Suno stack

## See also

- `kling-video` — peer skill for talking-head / static / lip-sync-precision shots (the deny-list routing destination)
- `nbp` — upstream skill for the 4K reference image that carries character appearance
- `NBPHero` — product-hero subclass of `nbp` (labels, flat-lay kits)
- `voice-elevenlabs` — upstream skill for `@Audio1` pre-generation (stub; build once user confirms scope)
- `mikos` — parallel UGC prompting system; lost head-to-head against `/seedanceX` for Seedance rendering (stub)
- `review-video` (Overseer B) — downstream QA
- `shotlist` (Overseer B) — orchestrator that calls this skill per-shot
- `ugc` (Overseer B) — creative-strategy upstream
