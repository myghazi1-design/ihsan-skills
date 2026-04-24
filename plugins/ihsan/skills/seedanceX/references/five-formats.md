---
type: skill-reference
skill: seedanceX
topic: formats
---

# The Five Optimized Formats (Higgsfield on Seedance 2.0)

Higgsfield's format library documents five recurring format shapes that Seedance 2.0 handles well. Pick a format BEFORE writing. Each has a specific structure, input spec, and signature "move."

## Quick Reference

| Format | Structure | Inputs | Signature |
|---|---|---|---|
| **Transformations** (highest-performing) | 6 shots / 15s / 16:9 | Image × 3-4 | Escalation arc: calm → threat → transformation → aftermath |
| **Orbs (POV)** | 1 shot / 15s / 16:9 | Image × 1 | "The camera IS her eyes" + inline `[VFX:...]` + "No cuts, no zoom, natural head movement" |
| **POV** | 1 shot / 15s / 16:9 | Image × 1 | "No cuts, no zoom, natural head movement" required |
| **Fights** | 1 shot / 15s / 16:9 | Image × 2 | Location + power mismatch + escalation arc + beat-by-beat action |
| **Animation** | 1 shot / 15s / 16:9 | Image × 1 (keyframe) | 0-3s, 3-6s, 6-9s, 9-12s, 12-15s segmented with particle physics |

## Routing Tip

Transformations is the highest-performing format on Higgsfield's leaderboard. When the concept allows (subject undergoes visible change), bias toward Transformations over single-shot cinematic.

---

## Transformations Template

```
[Global Context Block]

Shot 1 (0-2.5s): [calm baseline, subject at rest]
Shot 2 (2.5-5s): [first disturbance arrives]
Shot 3 (5-7.5s): [threat emerges]
Shot 4 (7.5-10s): [transformation begins — visible physical change]
Shot 5 (10-12.5s): [transformation peaks]
Shot 6 (12.5-15s): [aftermath / new state]

Total: 15s / 6 shots / 16:9
```

Provide 3-4 reference images — typically the calm state, the peak transformation state, and 1-2 intermediate anchor images.

---

## Orbs Template

```
POV of [subject]. The camera IS [their] eyes.

[Global Context Block]

[Beat 1: setting + subject reaction]
[VFX: branching electric circuits pulsing with white-blue current]
filling the frame as [subject] [reacts].

[Beat 2: VFX escalates]
[VFX: crystalline structures form from the current, refracting light]

[Beat 3: resolution or peak]

No cuts, no zoom, natural head movement.
1 shot / 15s / 16:9.
```

Single image input = the subject reference (first-person embodiment).

---

## POV Template

```
[Global Context Block]

POV of [subject]. The camera IS [their] eyes.

[Beat 1: what they see first]
[Beat 2: action / movement]
[Beat 3: reveal or resolution]

No cuts, no zoom, natural head movement.
1 shot / 15s / 16:9.
```

Use the `No cuts, no zoom, natural head movement.` constraint VERBATIM.

---

## Fights Template

```
[Location: specific environment with physical affordances]

[Subject A reference] vs [Subject B reference] with power mismatch.
[Describe the mismatch: size, weapon, skill, supernatural ability.]

Beat 1: [concrete choreography — first exchange]
Beat 2: [escalation — mismatch becomes apparent]
Beat 3: [climax — resolution or cliffhanger]

[Camera: one dominant movement across the whole fight]

1 shot / 15s / 16:9.
```

Two image inputs = Subject A + Subject B references.

---

## Animation Template

```
Keyframe: [detailed description of the single reference keyframe image]

0-3s: [opening motion + particle physics of environment]
3-6s: [action escalation]
6-9s: [beat change or reveal]
9-12s: [continued action]
12-15s: [resolution]

1 shot / 15s / 16:9.
```

Particle physics language drives quality. Describe: dust, smoke, sparks, mist, water droplets, fabric sway, hair movement. Single keyframe input.

## Why Formats Matter

Seedance's training distribution has strong priors for these shapes (ByteDance/Douyin genre conventions). Format-following produces outputs closer to reference material than free-form prompting.
