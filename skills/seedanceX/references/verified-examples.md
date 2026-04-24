---
type: skill-reference
skill: seedanceX
topic: verified-examples
---

# Verified Seedance Prompts (tested, good output)

Four proven prompts from production use. Verbatim.

## 1. Dog + Truck

```
A stray dog limps down an empty highway at dawn. A pickup truck slows
beside him. The passenger door swings open. The dog hesitates, then
hops in. Wide shot to medium. No music.
```

**Why it works:** Wide shot, big simple movements, no small object physics, no face detail. Exactly what Seedance is good at.

## 2. Mountain Running

```
A video showing the joys of mountain running. The protagonist is a tall
exceptionally attractive fair-skinned mestiza Filipina woman with an
hourglass figure and a large bust. Make her do a flashy aerial flip in
slow-motion along an extremely treacherous mountain path. The entire
video should be one continuous shot.
```

**Why it works:** One character, one clear action with natural arc (run → flip → land), cinematic but simple, lets Seedance fill in the physics.

## 3. Military Ground Combat (community-tested)

```
Ground-level war footage with documentary realism: a soldier squad
advances through a desert combat zone. Keep camera low and close to
boots, gear sway, weapon details, and formation changes while moving
over loose sand and debris. Harsh sunlight, visible heat haze, suspended
dust, and distant intermittent gunfire should shape the atmosphere. Use
controlled handheld movement with shallow depth of field. Color palette
stays sandy beige and muted brown.
```

**Why it works:** Genre-anchored ("documentary realism"), physics-rich description (heat haze, dust, sand), single camera behavior (handheld, low), palette locked.

## 4. Rooftop Portrait (community-tested)

```
A woman in her 30s, dark hair pulled back, navy linen blazer, turns
slowly toward the camera and smiles. Standing on a rooftop terrace at
sunset, city skyline behind her. Medium close-up, slow dolly-in. Soft
key light from the left, warm rim light, shallow depth of field, film
grain.
```

**Why it works:** Hits every slot in the formula under 80 words. Single camera movement. Specific lighting recipe. One subject.

## 5. Rain-Soaked City 12s UGC (Mode 2 example)

```
Scene: A rain-soaked city street at night, neon reflections on wet
pavement, cinematic style, high contrast lighting, 4K quality, shallow
depth of field throughout.

[0:00-0:04] Aerial wide shot, camera slowly descending from above the
city skyline, neon signs visible below, no subject in frame.

[0:04-0:08] Street-level medium shot, camera tracks forward through a
neon-lit alley, a figure in a dark coat walks ahead at a steady pace,
back to camera.

[0:08-0:12] Extreme close-up on rain droplets hitting a puddle, camera
completely static, reflection of neon lights visible in water, no human
subject.
```

**Why it works:** Global context block carries style/lighting/quality across all three segments. Bracket format `[0:00-0:04]` is what Seedance was optimized for. Each segment has ONE camera move. Clip 3 is a static anchor frame — both a seam-ready exit point for chaining and a sharp thumbnail candidate. No human face in close-up (avoids real-face blocking).

## Physical-not-Emotional Rewrite

- Bad: "She contemplates her past with a heavy heart."
- Good: "She sits on a bench, hands clasped, head slightly bowed, not moving."

## Good vs Bad Prompt Pair

### Good (6-step formula followed)
```
A skateboarder lands a clean trick in an empty dawn parking lot,
camera low tracking shot then subtle rise, modern cinematic contrast,
6 seconds, 16:9, avoid jitter and bent limbs.
```

### Bad (adjective stack)
```
cool skateboard video, cinematic, fast, amazing tricks, lots of movement, epic style
```

**Why the bad version fails:** No subject specificity. Banned keywords (`fast`, bare `cinematic`, `amazing`, `lots of movement`, `epic`). No camera instruction. Guarantees jitter from "lots of movement" + "fast."
