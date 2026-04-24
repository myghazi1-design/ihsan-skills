---
type: skill-reference
skill: ugc-creator-setup
source: notes (3).md (/ugc creative strategy) + MIKOSLAB V2 Prompting System reference
tags: [ugc, mikos, prompt-structure, 8-section]
---

# MIKOSLAB 8-Section UGC Prompt Structure

> [!important] Used WHOLE — never condense, merge, or reorder
> See [[2026-04-20-mikos-8-section-structure-used-whole]] for the decision. Condensing sections caused observed quality regressions in head-to-head renders.

## The 8 sections (canonical order)

### 1. Header
Concept summary in a single line. What kind of UGC, for what niche, at what duration.

### 2. Camera
Camera move + shot size + lens + angle. One primary camera instruction (per [[camera-vocabulary]]).

### 3. Environment
Where the scene is. Named light source + color language (per [[light-sources]]). Ambient sound cue.

### 4. Subject
Who the subject is. Uses one of the 16 MIKOSLAB subject archetypes. Physical description carried forward from [[nbp]] start frame — never re-described here.

### 5. Script
4-beat pattern:
- **Hook** (first 1 – 2s) — from [[hook-library]]
- **Setup** (context, the problem)
- **Payload** (the reveal, solution, or punch)
- **Hard cut exit** (end on impact, not drift)

### 6. Audio
Dialogue delivery (tone, pace), SFX ties to actions, music = `None` (post step only per [[2026-04-20-never-bake-music-into-generation]]).

### 7. Anti-Glitch
Targeted negatives for the known failure modes of the chosen tool (per [[tool-failure-table]]). 5 – 7 items max.

### 8. Final Output Feel
One-sentence vibe check. Does the assembled prompt *feel* like the intended deliverable? If not, iterate before rendering.

## Full-prompt pattern

```
## Header
[Concept line]

## Camera
[Named camera move + lens + shot size]

## Environment
[Location + named light + color + ambient]

## Subject
[Archetype + anchor to NBP reference]

## Script
Hook: "[from hook-library]"
Setup: "[the context line]"
Payload: "[the reveal]"
Hard cut exit: [closing visual]

## Audio
Dialogue: [tone + pace]
SFX: [ties]
Music: None (post step)

## Anti-Glitch
[5 – 7 targeted negatives]

## Final Output Feel
[One-sentence vibe check]
```

## 16 subject archetypes

Subject archetypes fill section 4. They answer "who is the subject" with a specific, repeatable character template. The list is referenced by the `/mikos` skill in the legacy notes archive — confirm the full enumeration with Overseer A's `mikos` skill when built, or treat as "use the archetype that matches the product's target avatar."

## See also

- [[2026-04-20-mikos-8-section-structure-used-whole]]
- [[hook-library]]
- [[camera-vocabulary]]
- [[light-sources]]
- [[tool-failure-table]]
