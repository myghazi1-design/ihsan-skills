---
name: hook-picker
description: Use when a user needs a hook for short-form video — "give me a hook", "hook ideas for X", "how should I open this video", "scroll-stopping intro", "pick a hook for [niche]", "which hook fits this product". Thin retrieval skill over the 250-hook library by Rachel Martinez. Takes niche + funnel stage + format + emotional register, returns 3–5 matching hooks with niche substitutions filled in and an optional visual-plus-spoken layering suggestion.
---

# Hook Picker

## Overview

Thin retrieval over the 9-category, 250-hook library in `Resources/frameworks/hook-library/`. Takes a user's intent and returns 3 – 5 on-fit hooks with `___` slots filled for the niche.

**Core principle:** This is a reference lookup, not a creative skill. The library is the database; this skill is the query layer. Do not invent hooks — surface ones from the library and customize the `___` slots.

## When to Use

- "Give me a hook for [niche]"
- "How should I open this video?"
- "Pick a hook for [product]"
- "What's a good scroll-stopper for [topic]?"
- Called by [[ugc-creator-setup]] during the script phase
- Called by [[shotlist]] when generating Clip 1
- Called by [[review-video]] when auditing a video opener

**Do NOT use for:**
- Writing the rest of the script — [[ugc-creator-setup]] owns the full 4-beat (Hook → Problem → Solution → CTA)
- Inventing original hooks — this is retrieval only; use the 250 in the library

## Core Logic

1. **Parse user intent into 4 variables:**
   - `niche` — what is this about?
   - `funnel_stage` — awareness / consideration / conversion?
   - `format` — visual / spoken / combined?
   - `register` — curiosity / pain / authority / warmth / contrarian?
2. **Map funnel stage → allowed categories** (see Quick Reference).
3. **Load the relevant category file(s)** from `Resources/frameworks/hook-library/`.
4. **Filter by format + register.**
5. **Return 3 – 5 hooks** with `___` slots filled for the niche.
6. **If user wants combined format:** offer to layer a visual hook + one spoken hook per the "Layer visual + spoken hooks" rule.

## Quick Reference — Funnel Stage → Category Map

| Funnel stage | Categories to pull |
|---|---|
| **Awareness** (stop the scroll) | [[01-visual]] + [[03-curiosity]] + [[04-relatable-pov]] + [[09-contrarian]] |
| **Consideration** (hold attention) | [[02-pain-point]] + [[05-authority]] + [[06-problem-solution]] |
| **Conversion** (drive action) | [[08-direct-response]] + [[07-soft-sell]] + [[06-problem-solution]] |

## Combination Rules (preserved from source)

- **Always layer** 1 visual + 1 spoken hook at the open.
- **Mix 2 categories** for curiosity + urgency (e.g., Curiosity spoken + Pain Point payoff line 1.5s in).
- **Pair hook with a mini story beat** — hook grabs, story keeps.
- **Test + track winners** — ask user to log high-performers back to `Resources/frameworks/hook-library/winners.md` (create if needed).

## Output format

```
## Hooks for [niche] — [funnel stage], [format], [register]

1. **[Hook text with ___ filled]** — [category]
   Why it fits: [1-line gloss]

2. **[Hook text]** — [category]
   Why it fits: ...

3. **[Hook text]** — [category]
   Why it fits: ...

## Layer suggestion (if user wants visual + spoken)

**Visual** (from [[01-visual]]): [physical action]
**Spoken** (first 0.5s after gesture): "[line]"
```

## Example invocation

**User:** "I need a hook for a busy-mom skincare line, conversion-stage ad."

**hook-picker response:**

```
## Hooks for busy-mom skincare line — conversion, spoken, warmth

1. **"I've been loving this routine lately."** — soft-sell
   Why it fits: warm / authentic, converts on warm audiences that have seen the product before.

2. **"This is your sign to try it."** — direct-response
   Why it fits: soft CTA that doesn't jar busy-mom warmth.

3. **"This replaced three products in my bathroom."** — problem-solution
   Why it fits: busy-mom register = "I don't have time for 10-step routines".

## Layer suggestion

**Visual** (from 01-visual): Walking into frame mid-sentence, holding the product.
**Spoken** (first 0.5s after step): "I've been loving this routine lately."
```

## Reference files

Uses the files in `Resources/frameworks/hook-library/` — no separate skill references needed. This is intentionally thin.

## Common Mistakes

- Inventing a new hook instead of retrieving from library — always pull from the 250.
- Not filling the `___` slots for the niche — always substitute niche-specific keywords.
- Returning all 9 categories for a single query — filter by funnel stage first.
- Forgetting to offer visual + spoken layering — the source explicitly recommends it.

## See Also

- [[hook-library]] — the 9-category, 250-hook library (the database)
- [[ugc-creator-setup]] — uses this skill during the script phase
- [[shotlist]] — uses this skill for Clip 1 hook
- [[Rachel Martinez]] — author
