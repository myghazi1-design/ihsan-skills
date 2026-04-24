---
name: review-video
description: Use when auditing a rendered AI video against its prompt — "review this video", "check this render", "ship or reroll", "audit this clip", "is this on-brand", post-render critique. Extracts frames via ffmpeg, applies a 6-axis rubric, cross-references tool-specific failure patterns, and returns a Ship / One-more-pass / Re-prompt verdict with specific prompt-change suggestions.
---

# Review Video

## Overview

Post-render QA for AI video output. Extracts frames, walks a 6-axis rubric frame-by-frame, applies tool-aware failure-pattern checks, and delivers a verdict. Does not generate, edit, or re-render — that's what the verdict routes back to.

**Core principle:** Frame extraction + multimodal inspection beats watch-the-whole-clip intuition. Apply the rubric per frame, then aggregate.

## When to Use

- A Seedance, Kling, Mikos, or Nano Banana render just completed and needs sign-off
- User drops a video file path with "look at this" / "does this match the prompt"
- Pre-ship QA before a chain advances to Clip 2
- Benchmarking a model head-to-head (e.g., Seedance vs Mikos)

**Do NOT use for:**
- Generating new clips — that's [[seedanceX]] / [[kling-video]] / [[nbp]]
- Editing or color-grading — that's CapCut / DAW post-production
- Reviewing still images only — apply the rubric subset (axes 3-6); skip frame extraction

## Core Logic

1. **Extract frames.** Default 1 fps; use 2 fps for clips under 5s. (Opus 4.7 with 1M context handles 30s clips at 1fps trivially — don't be conservative.)
2. **Build a contact sheet** + per-frame view.
3. **Walk the 6-axis rubric** per frame.
4. **Cross-reference tool-specific failure table.**
5. **Aggregate into a verdict.**
6. **Output:** Verdict + per-axis findings + specific prompt-change suggestions for "One more pass."

## The 6-Axis Rubric

| # | Axis | What to look for |
|---|---|---|
| 1 | **Realism** | Does it pass as real at scroll speed? Does it scream "AI"? |
| 2 | **Hands & body** | Finger count, proportions, continuity, bent limbs, floating hands |
| 3 | **Face & expression** | Symmetry, gaze alignment, mouth sync, expression consistency across frames |
| 4 | **Background & environment** | Stability, object permanence, lighting continuity |
| 5 | **Composition & framing** | Subject placement, camera behavior vs prompt |
| 6 | **AI artifacts** | Texture swimming, clothing distortion, text garbling, temporal inconsistency |

## The 3-Verdict Framework

| Verdict | Meaning | Next action |
|---|---|---|
| **Ship it** | Passes the scroll test; minor issues only | Export → Topaz 2x → CapCut 30fps → captions → Suno |
| **One more pass** | Fixable issues; specific prompt changes suggested | Edit prompt (per 2-reroll limit rule), re-render |
| **Re-prompt** | Fundamental issues; different approach needed | Rethink model choice, camera move, or subject count |

## Tool-Specific Failure Patterns to Flag

| Tool | Known weaknesses to watch |
|---|---|
| **Seedance 2.0** | Subtle camera moves fail at short durations; lip-sync drift at clip boundaries; mid-clip physics re-inference; 3+ character face warp; stacked camera moves → jitter; "cinematic" alone = gray flat |
| **Kling 3.0** | Harder consonants in native audio; secondary subject drift; compound camera moves still compute but degrade |
| **Mikos** | UGC constraint breaks past 8s ceiling (lip-sync death zone) |

Expand this table as renders accumulate failure data.

## Chain-Drift Checks (multi-clip)

For chained Seedance clips, also compare across clip seams:

- **Lighting temperature drift** (warm 3200K clip A vs warm-ish clip B = visible color shift)
- **Fabric crinkle / skin tone re-inference** (mid-clip physics does NOT carry across seams)
- **Background density drift** (crowds, window light angle, prop positions reset per generation)
- **Identity drift across 3+ clips** (compounding — coin flip at clip 4)

Chain-specific verdicts: if a seam fails, the verdict is on the seam, not the interior.

## Opus 4.7 Capability Notes

| 4.7 capability | Effect on review quality |
|---|---|
| **1M context** | Load full contact sheet + every individual frame without truncation; 30s clips at 1fps fit trivially, 60s comfortable |
| **Better vision** | Finer artifact detection — finger fusion, skin texture, gaze misalignment, temporal inconsistencies |
| **Improved multimodal reasoning** | Tool-aware verdicts ("known Seedance weakness at 8s" vs generic "AI artifact detected") |

**Practical implication:** extract aggressively; 4.7 handles the context.

## Reference files

- `rubric-6-axis.md` — the 6-axis rubric with per-axis checklists and example failures
- `tool-failure-table.md` — tool-specific failure patterns, expandable as data accumulates
- `chain-drift-signals.md` — what to look for at clip seams in chained Seedance
- `verdict-framework.md` — Ship / One-more-pass / Re-prompt decision logic

## Common Mistakes

- Watching the clip at normal speed only — use frame extraction
- Generic "AI artifact detected" verdict — always cite the specific axis and the specific frame(s)
- Skipping tool-specific failure table — leads to reviewer bias toward generic issues
- "Ship it" when the verdict is actually "One more pass" — cost of one more render < cost of shipping a bad clip

## See Also

- [[shotlist]] — consumes verdicts per shot in the shot table
- [[seedanceX]] (Overseer A) — primary source of clips under review
- [[kling-video]] — talking-head / dialogue clips under review
- [[2026-04-20-2-reroll-limit|Decision: 2-reroll limit]] — if two renders miss, edit the prompt
- [[2026-04-18-seedance-chaining-protocol|Decision: Master-Reference-Reuse over exit-frame chaining]]
