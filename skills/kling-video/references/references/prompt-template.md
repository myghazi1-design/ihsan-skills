# Kling 3.0 Prompt Templates

Fill-in-the-blank templates for each mode. Copy the appropriate one and substitute the bracketed values.

---

## Quick Mode (3–5 seconds, no dialogue)

```
[LOCATION at TIME OF DAY]. [ATMOSPHERE CUE]. [NAMED LIGHT SOURCE].

A [CHARACTER DESCRIPTOR] [PHYSICAL ACTION with mechanics].
[Anchored object interaction if hands matter].

[CAMERA MOVEMENT] [LENS/SHOT SIZE], [DEPTH OF FIELD].

Audio: [AMBIENT SOUND 1], [AMBIENT SOUND 2], [OPTIONAL MUSIC].

Negative: [5-7 targeted artifacts]
```

### Example

```
Narrow Tokyo alley at 2 AM. Steam rising from manhole grates. Flickering
neon signs casting magenta and cyan.

A woman in a red trench coat walks toward the camera. Hands in pockets.
Heels clicking on wet cobblestone, each step landing heel-first.

Handheld shoulder-cam drifts at walking pace, 35mm lens, shallow depth
of field.

Audio: Distant city hum, rain dripping, heels on stone.

Negative: sliding feet, distorted face, morphing limbs, flickering, cartoonish.
```

---

## Full Mode (5–15 seconds, narrative)

```
[SCENE: location, time of day, weather, atmosphere]
[LIGHTING: named light source(s), direction, quality]

[CHARACTER A: physical description, clothing, demeanor]
[CHARACTER B if multi-character: distinct from A]

[ACTION TIMELINE]:
- Beginning (0-Xs): [physical setup, first movement]
- Middle (X-Ys): [core action with physics]
- End (Y-15s): [resolution, final beat]

[CAMERA: shot type, movement, lens, angle]
[DEPTH: what's sharp vs soft]

Audio: [ambient], [SFX if any], [music style if any]

Negative: [5-7 targeted artifacts]
```

---

## Dialogue Mode (with native audio + lip sync)

```
[SCENE + LIGHTING]

[CHARACTER A: Role name, physical description]
[CHARACTER B: Role name, physical description]

[Shot 1 setup - who's visible, camera, lighting]
[Character A's physical action]
[Character A: Role name, tone descriptor]: "Line of dialogue."

Immediately, [Character B's physical reaction]
[Character B: Role name, tone descriptor]: "Response."

[Additional shots or lines...]

Audio: [ambient sounds], [music], [SFX tied to actions]

Negative: identity drift, lip-sync offset, morphing faces, [additional]
```

### Dialogue Format Rules

- Character labels **must** be unique and consistent: `[Character A: Lead Detective]`, `[Character B: Prime Suspect]`
- Tone descriptor **inside** the bracket: `[Character A: Lead Detective, controlled serious voice]`
- Temporal keywords sequence speakers: "Immediately,", "Then,", "Suddenly,", "A moment later,"
- Bind dialogue to a physical action first, then the line
- Multilingual supported: `[Character A: Mom, stressed voice, English]`, `[Character B: Vendor, friendly tone, Japanese]`

---

## Multi-Shot Mode (up to 6 shots)

```
Master Prompt: [Overall scene/narrative intent - one sentence]

Shot 1 (0-Xs): [Framing] [Camera movement] [Subject action] [Lighting detail]
Shot 2 (X-Ys): [Framing] [Camera movement] [Subject action] [Lighting detail]
Shot 3 (Y-Zs): [Framing] [Camera movement] [Subject action] [Lighting detail]
...
[Max 6 shots total]

Audio: [ambient for whole sequence], [music], [SFX with timing]

Negative: [5-7 targeted artifacts]
```

### Rules

- Maximum 6 shots per generation (platform cap, enforced)
- Each shot has its own framing, camera, and action
- Total duration across shots must be ≤ 15 seconds
- Audio field is for the whole sequence, not per-shot
- Use `@Character` references for identity consistency across shots: `@Male lead`, `@Female lead`

---

## Image-to-Video Mode

```
Animate with [CAMERA MOVEMENT]. Preserve [IDENTITY/LAYOUT/TEXT from source].

[WHAT MOVES]:
- [Primary motion: subject action]
- [Secondary motion: fabric, hair, steam, etc.]
- [Environmental motion: clouds, water, light]

[WHAT STAYS STATIC]:
- [Elements to preserve exactly from source]

Negative: style drift, morphing [preserved elements], identity change
```

### Rules

- Image-to-video should **preserve** before it **animates**
- Always explicitly list what to keep stable from the source
- Subtle motion usually beats dramatic motion in I2V
- Camera movement in I2V is relative to the source framing

---

## Output Format Checklist

Before returning a prompt to the user, verify:

- [ ] Scene layer present with named light source (not "dramatic lighting")
- [ ] Character layer with clear descriptor (not "someone")
- [ ] Action layer with physics or timeline
- [ ] Camera layer with named movement (not "camera moves")
- [ ] Audio layer (even if just ambient)
- [ ] Negative prompt with 5–7 targeted items
- [ ] If dialogue: `[Character A: Role, tone]:` format used consistently
- [ ] If multi-shot: ≤ 6 shots, time-coded, total ≤ 15s
- [ ] If hands visible: anchored to an object
- [ ] Total prompt length: 150–250 words for full mode, 80–150 for quick mode
