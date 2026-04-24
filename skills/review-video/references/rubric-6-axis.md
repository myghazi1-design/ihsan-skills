---
type: skill-reference
skill: review-video
source: notes (1).md (video production:ai source corpus)
tags: [review, rubric, qa, video]
---

# 6-Axis Review Rubric

Apply per frame. Aggregate to verdict.

## Axis 1 — Realism

**Question:** Does it pass as real at a scroll? Does it scream AI?

Checks:
- Skin texture — does it have natural pores, imperfection, light reactivity?
- "Plastic doll" smoothness → AI artifact
- Eyes — do they reflect light? Are they aligned?
- Cheap-CGI vibe (flat lighting, plastic skin) → FAIL

Common failures: "soft plastic skin", "smiling cartoonish", "video game aesthetic".

## Axis 2 — Hands & Body

**Question:** Finger count, proportions, continuity?

Checks:
- Finger count — always count on the screen
- Finger merging, extra digits, missing nails
- Arm joints — elbows, wrists bending correctly
- Weight transfer realism in standing / walking
- Bent limbs mid-action
- Hands anchored to objects (per Anchor Hands rule in [[physics-translations]])

Common failures: "extra fingers", "merged fingers", "floating hands", "distorted knuckles", "six fingers".

## Axis 3 — Face & Expression

**Question:** Symmetry, gaze, mouth, expression consistency?

Checks:
- Face symmetric left ↔ right
- Eye gaze aligned (both eyes tracking same point)
- Mouth sync with audio (lip-sync offset)
- Consistent expression across adjacent frames
- Identity preserved across a clip (no morphing mid-clip)
- For dialogue: lip-sync offset, mouth out of sync, silent speaking

Common failures: "face morphing", "identity drift", "asymmetric features", "distorted eyes", "warped mouth".

## Axis 4 — Background & Environment

**Question:** Stability, object permanence, lighting continuity?

Checks:
- Objects in background stay put (no teleporting props)
- Lighting temperature consistent across frames
- Shadows in correct direction per light source
- No glitching walls / disappearing doors
- Text on products — legible or garbled?
- For chains: lighting temp drift between clips

Common failures: "warped text", "unreadable typography", "distorted reflections", "floating objects".

## Axis 5 — Composition & Framing

**Question:** Subject placement, camera behavior matches prompt?

Checks:
- Subject in intended part of frame
- Camera move matches prompt (if prompt said "slow dolly push", is it a slow dolly push?)
- Camera move NOT specified → did the model invent one?
- Framing matches aspect ratio target

Common failures: camera move compound (jitter), "micro-tilt" when user asked for static.

## Axis 6 — AI Artifacts

**Question:** Texture swimming, distortion, text garbling, temporal inconsistency?

Checks:
- Texture swim on clothing, hair, skin
- Flickering between adjacent frames
- Clothing shape-shifting mid-action
- Cross-clip world-state resets (crowd density, prop positions)
- On-screen text garbled

Common failures: "texture swimming", "flickering", "morphing textures", "temporal flicker".

## Aggregation

Convert per-axis findings into a verdict:

- All 6 axes clean → **Ship it**
- 1 – 2 axes show fixable issues (wrong camera move, one bad frame of hands) → **One more pass**
- 3+ axes fail OR a structural issue (3+ characters warping, lip-sync fundamentally broken) → **Re-prompt**
