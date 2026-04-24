---
type: skill-reference
skill: video-dialogue-prompting
source: Dialogue Syntax.md (worked examples from fal.ai reference)
tags: [dialogue, examples, kling]
---

# Dialogue Prompt Examples (verbatim)

Two worked scenes from fal.ai's reference, preserved verbatim.

## Example 1 — Interrogation Scene

```
A sleek modern interrogation room with cold LED lighting. Muted gray walls,
a glass window, security cameras blinking red. Low atmospheric suspense
music hums with deep bass drones.

A detective in a navy suit leans forward slowly. His hands rest calmly
on the table.

[Character A: Lead Detective, controlled serious voice]: "Let's stop pretending."

Immediately, the suspect shifts in their chair, tense.

[Character B: Prime Suspect, sharp defensive voice]: "I already told you everything."

The detective slides a folder across the table. Paper scraping sound.

[Lead Detective, calm but threatening tone]: "Then explain why your fingerprints are here."

The suspect's breathing quickens.

[Prime Suspect, voice trembling]: "That's impossible…"

The detective stands suddenly, chair scraping back. Music tightens with
a rising pulse.
```

### Why it works

- **P1:** `Lead Detective` and `Prime Suspect` labels are reused exactly.
- **P2:** Every line has a physical anchor ("leans forward slowly", "shifts in their chair", "slides a folder").
- **P3:** Tone shifts every beat — `controlled serious` → `calm but threatening`; `sharp defensive` → `voice trembling`.
- **P4:** `Immediately,` sequences the response between speakers.

## Example 2 — Multilingual Tokyo Night Market

```
A small night market street in Tokyo illuminated by hanging lanterns.

[Character A: Vendor, friendly tone, Japanese]: "今日は寒いですね。"

A tourist responds with slightly accented Japanese.

[Character B: Tourist, hesitant but warm, Japanese]: "はい、でも雰囲気が素敵です。"

Her companion adds softly in English.

[Character C: Companion, wonder in his voice, English]: "This feels like a movie scene."

Audio: Natural mouth movement, accurate lip sync, gentle crowd noise,
light wind brushing fabric and paper lanterns.
```

### Why it works

- **Multilingual append pattern:** language placed after tone inside bracket.
- **Mixed languages in one scene:** Japanese for the local exchange, English for the observer — the model handles all three voices independently.
- **P2 anchors:** each line has a physical beat ("responds with slightly accented Japanese", "adds softly").
- **Audio layer:** explicit ambient sound + lip-sync instruction at bottom.

## Pattern notes for new examples

When adding new worked examples, annotate which P1 – P4 principle each bracket demonstrates. Keep failing-example rewrites (Before → After) adjacent, not stored separately.
