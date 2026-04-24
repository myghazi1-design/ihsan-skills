---
name: shotlist
description: Use when turning a rough AI video concept into a complete production plan — "shot list", "break this concept into clips", "plan the video", "concept to shots", "production plan". Outputs a concept summary, hook, shot table with tool assignments (Kling / Seedance / Nano Banana), NBP prompts, video prompts, parallel-generation plan, and post-production notes. Thin orchestrator — delegates actual prompt generation to seedanceX, kling-video, nbp.
---

# Shotlist

## Overview

The handoff layer between creative direction and production. Takes a concept, emits a structured shot list with tool assignments per clip, and hands off to the per-tool skills for actual prompt generation.

**Core principle:** Every clip gets a tool assignment up front. No ambiguity. NBP carries appearance; video prompts handle motion.

## When to Use

- User has a rough concept and wants a production-ready shot plan
- Converting a UGC script (from [[ugc-creator-setup]]) into the render pipeline
- Planning a chain of clips before any generation starts
- Re-planning after [[review-video]] verdicts force a shot redesign

**Do NOT use for:**
- Writing individual Seedance or Kling prompts — that's delegated
- Generating images — [[nbp]] handles 4K starting / reset frames
- Reviewing finished clips — [[review-video]] handles QA

## Core Logic

1. **Intake the concept.** Extract: what the video is, who it's for, what the viewer should feel.
2. **Generate Clip 1 = the hook.** First 1 – 2 seconds. ==Never an establishing shot.== Pull from [[hook-picker]] if needed.
3. **Build the shot table.** Columns: #, duration (≤ 6s each; 8s = lip-sync death zone), tool, shot type, dependency.
4. **Route each shot per the Seedance vs Kling table** (see [[2026-04-20-seedance-vs-kling-routing]]):
   - Talking head → Kling 3.0 (literal camera, lip-sync precision)
   - Action / cinematic → Seedance 2.0 (reinterprets into genre conventions)
   - Product close-up → NBP still (may not need video)
   - Start / reset frame → NBP at 4K
5. **Generate NBP prompts** for Clip 1 and any reset points.
6. **Generate per-shot video prompts** — delegate to [[seedanceX]] or [[kling-video]] per clip.
7. **Parallelize.** Identify islands (independent clips) vs chains (dependent). Kick off islands first.
8. **Append post-production plan:** transitions, music (Suno, post step), captions.

## Hard Rules

- **Max 6s per clip** — 8s is the lip-sync death zone (see [[2026-04-20-max-6-second-clips]]).
- **Hook is Clip 1** — never an establishing shot.
- **NBP carries appearance** — video prompts handle motion / dialogue / tone only (see [[2026-04-20-nbp-carries-appearance]]).
- **Islands first** — generate independent clips while planning chains.
- **Every clip gets a tool assignment** — no ambiguity.
- **Never bake music into the generation** — post step via Suno (see [[2026-04-20-never-bake-music-into-generation]]).

## Output Structure

```
## Concept
[What the video is, who it's for, what the viewer should feel — 2–3 sentences]

## Hook (Clip 1, first 1–2s)
[Pulled from hook library; includes visual + spoken pairing if applicable]

## Shot Table
| # | Duration | Tool | Shot type | Dependency | Notes |
|---|---|---|---|---|---|
| 1 | 6s | Seedance | Hook / visual | — | NBP-1 |
| 2 | 5s | Kling | Talking head | Clip 1 anchor | NBP-1 (same subject) |
| 3 | 4s | Seedance | Product close-up | Independent | NBP-2 (product) |
| ... | | | | | |

## NBP Prompts
- NBP-1: [4K character start frame prompt]
- NBP-2: [4K product hero prompt]

## Video Prompts
- Clip 1: [Delegate to seedanceX, assembled prompt here]
- Clip 2: [Delegate to kling-video, assembled prompt here]
- Clip 3: [Delegate to seedanceX, assembled prompt here]

## Parallel Gen Plan
- Islands (run first): [list independent clips]
- Chains (plan seams): [list dependent clip sequences + chaining method per 2026-04-18-seedance-chaining-protocol]

## Post-Production
- Topaz 2x / 60fps
- CapCut export at 30fps
- Captions (CapCut or Captions.ai)
- Suno music layer (added post, never baked into gen)
```

## Quick Reference — Tool Routing

| Shot type | Tool | Why |
|---|---|---|
| Talking head / dialogue | Kling 3.0 | Literal camera, lip-sync precision, native audio |
| Action / cinematic / explosions | Seedance 2.0 | Genre-aware reinterpretation, dynamic camera |
| Wide environmental shot | Seedance 2.0 | Great at wide + lighting + atmosphere |
| UGC timestamp `[0:00-0:04]` | Seedance 2.0 | Trained on this bracket convention |
| Static / locked camera | Kling 3.0 | Seedance has micro-tilt bug |
| Small object detail | Kling 3.0 | Better fidelity |
| 3+ characters | Kling 3.0 (still risky) | Seedance face warps at 3+ |
| Product close-up (still) | NBP | 4K text rendering |
| Character start frame | NBP | Carries appearance downstream |

## Chain Planning

For videos > 15s, plan the chain per [[2026-04-18-seedance-chaining-protocol]]:
- Re-upload the NBP master asset every clip (Master-Reference-Reuse)
- Identical global context block in every clip
- Shared `@Audio1` as voice anchor
- Anchor-frame exit on each clip (2 – 3s static segment)
- ==Stop planning at 3 clips== without a validation render — 4th is coin flip.

## Reference files

- `output-template.md` — canonical shot-list output format
- `tool-routing-table.md` — full routing table extracted verbatim
- `chain-planning.md` — 2-to-4 clip chain planning logic

## Common Mistakes

- Starting with an establishing shot → scroll-bypass in first 2s
- Skipping NBP for Clip 1 → video prompt carries appearance AND motion, compounding errors
- No tool assignment on a clip → creative has to decide at gen time, introducing drift
- Chains past 3 clips without validation → compound drift surfaces live
- Mixing music into the gen → can't re-cut without recomposing

## See Also

- [[hook-picker]] — feeds Clip 1 hook
- [[ugc-creator-setup]] — feeds full UGC scripts through this skill
- [[seedanceX]] (Overseer A) — per-shot prompt generator (action / cinematic / UGC)
- [[kling-video]] — per-shot prompt generator (talking head / dialogue)
- [[nbp]] (Overseer A) — 4K start frame / reset frame generator
- [[cinematography-prompting]] — substitution layer called per shot
- [[video-dialogue-prompting]] — dialogue syntax for talking-head shots
- [[review-video]] — post-render QA with verdicts feeding back into shot-list iteration
- [[2026-04-20-seedance-vs-kling-routing]]
- [[2026-04-20-max-6-second-clips]]
- [[2026-04-20-nbp-carries-appearance]]
- [[2026-04-18-seedance-chaining-protocol]]
