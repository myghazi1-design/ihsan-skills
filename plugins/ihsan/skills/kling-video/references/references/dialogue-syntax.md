# Dialogue Syntax

**Confidence: [High] on the character-label format, [Test first] on the "Immediately," temporal keyword.**

Kling 3.0 supports native audio with lip-sync across 5 languages (Chinese, English, Japanese, Korean, Spanish) plus dialects. The syntax below is documented by fal.ai (infrastructure provider serving Kling) and echoed across practitioner sources.

## Core Format

```
[Character A: Role name, voice tone descriptor]: "Line of dialogue."
```

### Rules

1. Character label in brackets opens each line
2. Role name after the colon identifies who this is
3. Voice tone/emotion descriptor inside the bracket (comma-separated)
4. Language optional at end of bracket for multilingual
5. Dialogue line in double quotes
6. Character labels must be **identical** every time you reference the same character — no synonyms, no pronouns

## The Four Dialogue Principles (P1–P4) — from fal.ai

### P1 — Structured Naming

Character labels must be unique and consistent. Avoid pronouns or synonyms after introduction.

❌ **Wrong:**
```
[Character A: Woman]: "Let's go."
She smiled.
[Woman]: "I'm ready."
```

✅ **Right:**
```
[Character A: Woman]: "Let's go."
[Character A: Woman, soft tone]: "I'm ready."
```

### P2 — Visual Anchoring

Bind dialogue to a character's unique physical action. Describe the action first, then the dialogue line.

❌ **Wrong:**
```
[Character A: Lead Detective]: "Where is the truth?"
```

✅ **Right:**
```
The black-suited agent slams his hand on the table. Ceramic clinks sharply.
[Character A: Lead Detective, angrily shouting]: "Where is the truth?"
```

### P3 — Audio Details

Assign unique tone and emotion labels inside the character bracket for every line. Don't use the same descriptor twice if the emotion shifts.

❌ **Wrong:**
```
[Character A: Mom]: "Shoes on!"
[Character A: Mom]: "Help."
```

✅ **Right:**
```
[Character A: Mom, fast urgent voice]: "Shoes on!"
[Character A: Mom, exhausted voice]: "Help."
```

### P4 — Temporal Control

Use linking words like "Immediately," "Then," "Suddenly," "A moment later," to sequence speakers and prevent the model from merging speech lines.

❌ **Wrong:**
```
[Character A: Detective]: "Let's stop pretending."
[Character B: Suspect]: "I already told you everything."
```

✅ **Right:**
```
[Character A: Detective, controlled serious voice]: "Let's stop pretending."
Immediately, the suspect shifts in their chair, tense.
[Character B: Prime Suspect, sharp defensive voice]: "I already told you everything."
```

> [!tip] Test first — "Immediately," keyword
> This temporal keyword is documented by fal.ai but not confirmed in first-party Kuaishou documentation. If you're building production workflows, verify behavior with a paired test (with and without the keyword) before committing.

## Temporal Keywords Library

For sequencing speakers or beats:

| Keyword | Use |
|---|---|
| **Immediately,** | Direct rapid response |
| **Then,** | Sequential, short pause |
| **Suddenly,** | Surprise, abrupt change |
| **A moment later,** | Noticeable pause |
| **Pause.** | Full beat, silence |
| **Silence.** | Extended quiet |
| **After a beat,** | Cinematic pause |

## Tone Descriptors Library

Inside the character bracket, after the role name:

### Emotional States

- controlled serious voice
- warm nostalgic voice
- shouting loudly
- voice cracking
- sharp defensive voice
- calm but threatening tone
- exhausted voice
- fast urgent voice
- hesitant voice
- soft tone
- whispering
- trembling frustrated voice
- confident authoritative voice
- sleepy amused voice
- annoyed sarcastic tone
- emotional voice
- cold quiet tone

### Physical Voice Qualities

- raspy, deep voice
- clear fearful voice
- breathy
- booming
- thin high voice
- gravelly
- smoky
- crisp articulate
- mumbling
- rapid-fire

## Multilingual Format

Append the language after the tone:

```
[Character A: Woman, soft tone, French]: "Tu crois que c'est possible?"
[Character B: Man, thoughtful tone, English]: "I think we have to try."
```

Supported languages in Kling 3.0:
- Chinese (+ dialects)
- English
- Japanese
- Korean
- Spanish
- Regional dialects and accents supported

## SFX Notation

For sound effects tied to actions:

```
SFX: A massive power-up sound effect like a turbine spinning at max speed that cuts the silence of the final frame
```

Or inline:

```
The detective stands suddenly, chair scraping back. SFX: Chair scrape, footsteps on wood.
```

## Full Dialogue Scene Template

```
[SCENE: location, atmosphere, ambient sound, light source]

[Character A: Role, physical description]
[Character B: Role, physical description]

[Setup action — who's visible, camera framing]
[Character A's physical anchor action]
[Character A: Role, tone]: "Line."

Immediately, [Character B's reaction]
[Character B: Role, tone]: "Response."

[Optional: environmental beat]
[Character A, different tone]: "Follow-up line."

[Optional: shot change for visual variety]
[Character B: Role, shifted tone]: "Closing line."

Audio: [ambient], [music swells if any], [SFX with timing]
```

## Example — Interrogation Scene (fal.ai reference)

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

## Example — Multilingual Morning Market

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
