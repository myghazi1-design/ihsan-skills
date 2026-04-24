---
type: skill-reference
skill: seedanceX
topic: formulas
---

# Seedance Prompt Formulas

Canonical templates for each mode and format. Use verbatim.

## Mode 1 Cinematic — 6-Step Formula

```
[Subject] + [Action] + [Environment] + [Camera Movement] + [Style/Lighting] + [Constraints]
```

Target: 60-80 words. One camera move. 2 characters max.

## Mode 1 Multi-Shot — Header + Shots + Closing

```
Montage, multi-shot [genre/purpose], don't use one camera angle or single cut,
cinematic lighting, photorealistic, 35mm film quality, ARRI ALEXA aesthetic.

[Shot 1 description.]
[Shot 2 description.]
...

Total: 15s / N shots / 16:9
```

## Mode 2 UGC — Global Context Block + Timestamp Segments

```
Scene: [environment], [visual style], [lighting], [quality descriptors]

@Image1 is the starting frame. @Image2 is [the product/prop].
For dialogue and audio reference, use @Audio1.

[0:00-0:04] [beat title].
Dialogue: "[line]"
Visuals: [physical, observable action]

[0:04-0:08] [beat title].
Dialogue: "[line]"
Visuals: [physical, observable action]

[0:08-0:12] [beat title].
Dialogue: "[line]"
Visuals: [physical, observable action]

No music.
```

Bracket format is `[M:SS-M:SS]`. Segments 2-5s, 4-6 per generation.

## 4-Layer Timeline UGC (structural)

1. **Global Context** — scene, visual style, lighting, 4K, DoF
2. **Timestamp Segments** — `[0:00-0:03]` bracket format, 2-5s each, 4-6 per generation
3. **Camera Direction** — shot type + movement + speed, ONE instruction per segment
4. **Subject Action** — physical / concrete / observable; no emotional abstractions

## Global Context Block (identical across chain + multi-segment)

```
Scene: [environment], [visual style], [lighting], [quality descriptors]
— applied to entire clip.
```

## POV Format (Higgsfield)

```
[scene + subject eye POV]. The camera IS [their] eyes.
[action / beat 1]. [action / beat 2].
No cuts, no zoom, natural head movement.
1 shot / 15s / 16:9.
```

## Orbs (POV with VFX inline)

```
POV of [subject]. The camera IS [their] eyes.
[action]. [VFX: branching electric circuits pulsing with white-blue current]
filling the frame as [subject] [reacts].
No cuts, no zoom, natural head movement.
1 shot / 15s / 16:9.
```

## Fights

```
[Location]. [Subject A] vs [Subject B] with power mismatch.
Beat 1: [concrete choreography].
Beat 2: [escalation].
Beat 3: [climax].
1 shot / 15s / 16:9.
```

## Animation (keyframe + segmented)

```
Keyframe: [single reference image description].
0–3s: [particle physics + motion beat]
3–6s: ...
6–9s: ...
9–12s: ...
12–15s: ...
```

## Transformations (6-shot escalation arc)

```
[Global Context Block]

Shot 1 (0-2.5s): [calm baseline]
Shot 2 (2.5-5s): [first disturbance]
Shot 3 (5-7.5s): [threat emerges]
Shot 4 (7.5-10s): [transformation begins]
Shot 5 (10-12.5s): [transformation peaks]
Shot 6 (12.5-15s): [aftermath / new state]

Total: 15s / 6 shots / 16:9
```

## Physical-not-Emotional rewrite rule

Every Mode 2 Visual line must describe concrete observable actions, not emotional abstractions.

- Bad: "She contemplates her past with a heavy heart."
- Good: "She sits on a bench, hands clasped, head slightly bowed, not moving."
