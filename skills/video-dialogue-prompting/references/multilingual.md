---
type: skill-reference
skill: video-dialogue-prompting
source: Dialogue Syntax.md (Multilingual Format section)
tags: [dialogue, multilingual, kling]
---

# Multilingual Dialogue

Kling 3.0 supports 5 primary languages plus regional dialects. Append the language AFTER the tone descriptor inside the character bracket.

## Format

```
[Character A: Role, tone, LANGUAGE]: "Line in that language."
```

## Supported languages

- Chinese (+ dialects — Cantonese, Mandarin, regional)
- English
- Japanese
- Korean
- Spanish
- Regional dialects and accents supported per the language

## Example — mixed-language scene

```
[Character A: Woman, soft tone, French]: "Tu crois que c'est possible?"
[Character B: Man, thoughtful tone, English]: "I think we have to try."
```

## Example — multilingual Tokyo night market

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

## Notes

- French is NOT in Kling 3.0's first-party supported list but is shown in the fal.ai reference. Treat as [Test first] until validated.
- Accent-appropriate pronunciation ("slightly accented Japanese") is described in the **narrative**, not the bracket — the bracket only names the target language.
