# Physics-Over-Action Translations

**Confidence: [Medium] — Single source (atlascloud.ai), mechanistically plausible. Test on your own setup before treating as default.**

The premise: describing HOW motion happens physically produces better results than describing WHAT happens, because video diffusion models learned motion from captioned footage where captions use physical descriptors.

## The Rule

When a user says an action, translate it into physical mechanics if human motion realism matters for the shot.

## Translation Examples

### Walking

| ❌ Generic | ✅ Physics |
|---|---|
| She walks | Each step lands heel-first, then rolls forward through the ball of the foot |
| He walks confidently | Long strides, arms swinging in counter-rhythm to legs, weight planted through each heel |
| They stroll | Relaxed pace, arms loose at sides, minimal arm swing, weight settling into each step |
| She walks nervously | Short quick steps, arms stiff at sides, weight on balls of feet |
| He walks away | Deliberate steps, back to camera, gait shortening slightly with each step |

### Running / Sprinting

| ❌ Generic | ✅ Physics |
|---|---|
| He runs | Full stride extension, feet striking ground with visible impact, arms pumping in counter-rhythm, forward lean |
| She sprints | Maximum stride length, feet barely contacting ground, arms driving forward and back aggressively, muscles visibly tensing and releasing |
| He jogs | Moderate pace, feet landing mid-foot, arms at 90° bent and swinging naturally |
| They chase | Urgent fast strides, eyes forward, breathing visible |

### Sitting / Standing

| ❌ Generic | ✅ Physics |
|---|---|
| She sits down | Lowers weight slowly into the chair, cushion compresses beneath her, adjusts clothing as she settles |
| He stands up | Pushes off with his hands, weight transferring to his legs, straightening slowly |
| She leans back | Weight shifting rearward, shoulders meeting the seatback, head tilting up |
| He sits down hard | Drops into chair, cushion compresses sharply, slight bounce |

### Hand Actions

| ❌ Generic | ✅ Physics |
|---|---|
| She picks up the cup | Fingers close around the ceramic, thumb resting on top, lifts from saucer with steady grip |
| He writes | Pen gripped in right hand, wrist resting on paper, fingers making small controlled movements |
| She types | Fingers striking keys in rapid succession, wrists elevated slightly, eyes on screen |
| He opens the door | Hand grips the handle, turns it, pulls with arm extension |
| She waves | Arm raises to shoulder height, palm facing forward, fingers spread, hand swinging side to side |

### Facial Expressions (stage anatomically)

| ❌ Generic | ✅ Staged |
|---|---|
| She smiles | Corners of mouth curve upward, cheeks lift slightly, eyes crinkle at edges |
| He looks surprised | Eyebrows lift first, eyes widen, then mouth opens slightly |
| She looks sad | Lower lip pushes forward slightly, eyebrows draw together, eyes soften |
| He laughs | Mouth opens, head tilts back, shoulders shake slightly with each breath |
| She cries | Eyes well up, lower lip trembles, single tear traces down cheek, shoulders drop |

### Head Movements

| ❌ Generic | ✅ Physics |
|---|---|
| She turns her head | Head rotates slowly left to right, hair follows just behind the motion, catching light as it moves |
| He nods | Head drops slightly, then returns, repeating twice |
| She shakes her head | Small lateral rotations, hair swaying with the movement |
| He looks up | Chin lifts, eyes rising first, then head tilts upward |
| She looks down | Head drops forward, eyes lowering, slight shoulder slump |

### Object Interaction

| ❌ Generic | ✅ Physics |
|---|---|
| He pours water | Pitcher tilts, water streams out in a steady arc, glass fills with rising liquid line |
| She drops the book | Fingers release, book falls through air, impacts surface with visible bounce |
| He slams the door | Hand pushes hard, door swings rapidly, impacts frame with vibration |
| She catches the ball | Hands extend, ball contacts palms, fingers close around it, arms absorb momentum |

## Anchor Hands to Objects

**Single biggest hand-stability fix**: hands anchored to objects are more stable than free-floating hands.

| ❌ Free-floating | ✅ Anchored |
|---|---|
| She moves her hands | Her fingers firmly grip the ceramic coffee cup |
| He gestures | His hand rests on the railing, thumb tapping once |
| She reaches out | Her hand grasps the doorknob, turning it slowly |
| He scratches his head | His fingertips run through his hair above the ear |

**Rule for the skill:** If hands appear in the frame and the user hasn't specified what they're doing, ask what they're holding or touching before generating.

## Domain-Specific Vocabulary

Use professional terminology when the action has a known domain. Kling was trained on captioned footage that uses these terms.

| Domain | Example terms |
|---|---|
| **Ballet** | Second position, arabesque, pirouette, demi-plié |
| **Martial arts** | Roundhouse kick, front stance, guard up, haymaker |
| **Cooking** | Julienne, deglaze, fold in, sauté |
| **Coffee / barista** | Pull a shot, steam milk, latte art pour |
| **Construction** | Frame a wall, mix mortar, lay a course |
| **Piano** | Arpeggio, glissando, chord progression |
| **Tennis** | Forehand, backhand, serve motion, follow-through |
| **Yoga** | Downward dog, warrior pose, sun salutation |

## When NOT to Translate

Don't translate action verbs when:
- The user is describing abstract/emotional beats (use emotional language)
- The shot is too wide for physics to be visible
- The action is brief and background (B-roll extras)
- The user explicitly wants loose interpretation

## Self-Test Protocol

Validate this rule yourself:

1. Generate the same scene twice:
   - Prompt A: "A woman walks through the park"
   - Prompt B: "A woman walks through the park, each step landing heel-first, weight rolling forward through the ball of her foot, arms swinging in counter-rhythm"
2. Compare motion quality in a side-by-side review.
3. If no noticeable difference, the physics-over-action rule doesn't apply to your use case and can be downgraded or removed from defaults.
