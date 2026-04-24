---
name: ugc-creator-setup
description: Use when a user wants to start a UGC side hustle, become a UGC creator, pitch brands, negotiate usage rights, set UGC rates, build a UGC portfolio, sign a first brand deal, or productize AI-generated UGC. Walks Rachel Martinez's 11-phase playbook step-by-step from niche pick to contract negotiation. Optional AI-UGC branch routes scripts through hook-picker + seedanceX / kling-video / nbp / mikos for AI-produced creator content.
---

# UGC Creator Setup

## Overview

Interactive, phased walkthrough of [[Rachel Martinez]]'s UGC Creator Start Up playbook. Walks user from zero to first paid brand deal. Optional branch: AI-UGC — users productize AI-generated creator content using the Ihsan pipeline.

**Core principle:** Every phase has concrete next steps. Never give "just pitch more brands" advice — always specify which brand, which channel, which template.

## When to Use

- "Start a UGC side hustle" / "become a UGC creator"
- "Pitch brands" / "UGC portfolio" / "first UGC brand deal"
- "What should I charge for UGC" / "UGC rates" / "usage rights"
- "UGC contract" / rate negotiation question
- User pivots from "I want to be a UGC creator" to "I want to productize AI-UGC as a service"

**Do NOT use for:**
- Prompt generation for a single video — route to [[shotlist]] + tool skills
- Hook selection alone — route to [[hook-picker]]
- Running paid ads — route to `ads:*` plugin skills

## Core Logic

1. **Detect entry phase.** Ask: "Where are you? [picking a niche / have samples / pitching / negotiating / under contract]"
2. **Load matching phase** from [[ugc-playbook]] — coach with concrete next step.
3. **Rate / contract phase** pulls from [[ugc-rates-and-monetization]]. Always ask "what's your target niche and deliverable type" before quoting numbers.
4. **Pitch phase** offers Rachel's template; offer to draft a personalized pitch for a named brand.
5. **AI-UGC branch:** if user says "I want to use AI for this", route to combo with [[seedanceX]] / [[kling-video]] / [[nbp]] / mikos-mode (this skill).

## The 11 Phases (from Rachel Martinez's playbook)

| # | Phase | Key decision |
|---|---|---|
| 1 | What UGC Is and Why It Works | Organic vs whitelisting understanding |
| 2 | Key UGC Terms | Whitelisting, Usage Rights, CTA, Retainer, Spark Ads |
| 3 | Getting Started Checklist | Socials → equipment → 2–3 niches → 5 samples → portfolio → pitch |
| 4 | 7 Video Types to Master | B-roll+VO, talking head, tips, lifestyle, unboxing, before/after, story |
| 5 | Video Structure Formula | Hook → Problem → Solution → CTA |
| 6 | Equipment (minimal) | Phone + tripod + ring light; CapCut / TikTok editor |
| 7 | Filming Tips | Natural light, quick cuts, captions, 15 – 60s length |
| 8 | Landing Brand Deals | #UGCWanted; email > DM; personalize to recent campaign |
| 9 | Collaboration Types | Short-term vs retainer |
| 10 | Rates | Benchmarks from [[ugc-rates-and-monetization]] |
| 11 | Contract Essentials | Always contract; 2 free revisions then $50; 10-day payment; walk from lowballs |

Full phase content in [[ugc-playbook]].

## AI-UGC branch (the Ihsan pipeline)

When user asks "can I use AI for UGC?" — yes, and here's how the phases map:

- **Phase 3 (portfolio):** generate 5 AI-UGC samples across 3 niches using [[shotlist]] → [[seedanceX]] / [[kling-video]] / [[nbp]]
- **Phase 4 (video types):**
  - Talking head → [[kling-video]] dialogue mode
  - B-roll + VO → [[seedanceX]] cinematic mode + [[voice-elevenlabs]]
  - Unboxing / demo → [[seedanceX]] + NBP product hero
  - Before / after → [[seedanceX]] timestamp mode
- **Phase 5 (Hook → Problem → Solution → CTA):** [[hook-picker]] generates the hook; the 4 beats become the 4 clips in [[shotlist]].
- **Phase 10 (rates):** AI-UGC lets you undercut human-UGC benchmarks while preserving retainer structure — see [[ugc-rates-and-monetization]] strategic implication section.
- **Authenticity premium warning:** AI-UGC must NOT read as polished / AI-generated to compete. Tune [[nbp]] subjects toward relatable, not hyper-polished.

## Mikos-mode (8-section UGC prompt structure)

Inside the AI-UGC branch, when generating the actual prompt to pair with a hook + 4-beat script, use the **MIKOSLAB V2 8-section structure WHOLE**:

1. Header
2. Camera
3. Environment
4. Subject
5. Script
6. Audio
7. Anti-Glitch
8. Final Output Feel

==Never condense, merge, or reorder.== See [[2026-04-20-mikos-8-section-structure-used-whole]] for the decision. 16 subject archetypes fill section 4 ("who is the subject"). The 4-beat script (hook → setup → payload → hard cut exit) fills section 5.

## Quick Reference — Phase Entry Points

If user says... | Jump to phase
---|---
"I just decided to start" | Phase 1 (orientation)
"I have my phone, what next?" | Phase 3 (checklist)
"I filmed samples but no pitches yet" | Phase 8 (landing deals)
"A brand responded, what now?" | Phase 9 (collaboration type)
"They offered $30, is that fair?" | Phase 10 (rates)
"Send me a contract template" | Phase 11 (contract)

## Reference files

- `mikos-8-section.md` — the 8-section MIKOSLAB structure preserved whole
- `ai-ugc-branch.md` — AI-UGC mapping of the 11 phases to Ihsan pipeline skills
- `pitch-templates.md` — Rachel's pitch template + personalized variants

## Common Mistakes

- Quoting rates without asking niche + deliverable type
- Skipping Phase 11 (contract) — "just send an invoice" loses fights with brands who ghost
- Generating 5 samples in 1 niche → no niche validation; do 2 – 3 niches
- AI-UGC without authenticity tuning → looks AI → price gets undercut to nothing
- Condensing the Mikos 8-section structure — proven regression

## See Also

- [[Rachel Martinez]] — author / source
- [[ugc-playbook]] — full 11-phase playbook verbatim
- [[ugc-rates-and-monetization]] — rate tables + AI-UGC strategic implications
- [[hook-library]] — 250 hooks, 9 categories
- [[hook-picker]] — thin retrieval over hook library
- [[shotlist]] — converts a UGC script into a shot list
- [[seedanceX]] (Overseer A) — AI-UGC video generation
- [[kling-video]] — AI-UGC talking-head generation
- [[nbp]] (Overseer A) — 4K creator / product reference images
- [[2026-04-20-mikos-8-section-structure-used-whole]]
