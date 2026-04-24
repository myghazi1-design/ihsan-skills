---
type: skill-reference
skill: video-dialogue-prompting
source: Dialogue Syntax.md (Temporal Keywords section)
confidence: test-first
tags: [dialogue, temporal-keywords, kling, p4]
---

# Temporal Keywords Library

For sequencing speakers or beats per the **P4 — Temporal Control** principle. Place the keyword between speaker changes or at the start of a new beat.

## Library

| Keyword | Use |
|---|---|
| **Immediately,** | Direct rapid response |
| **Then,** | Sequential, short pause |
| **Suddenly,** | Surprise, abrupt change |
| **A moment later,** | Noticeable pause |
| **Pause.** | Full beat, silence |
| **Silence.** | Extended quiet |
| **After a beat,** | Cinematic pause |

## Usage pattern

```
[Character A: Role, tone]: "Line 1."

<temporal keyword>, [Character B physical reaction]
[Character B: Role, tone]: "Line 2."
```

## Test-first status

- `Immediately,` is fal.ai-documented but not confirmed in first-party Kuaishou docs.
- The other keywords (`Then,`, `Suddenly,`, `A moment later,`, `After a beat,`) inherit the same status — documented by practitioner sources but not first-party.
- Run a paired A/B before committing for any production-grade workflow.
