---
type: skill-reference
skill: cinematography-prompting
source: Physics Translations.md (Domain-Specific Vocabulary section)
tags: [cinematography, domain-terms, physics, kling]
---

# Domain-Specific Vocabulary

Use professional terminology when the action has a known domain. Kling was trained on captioned footage that uses these terms. Diffusion models recognize domain tokens — "pirouette" beats "spins."

## Table

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

## Usage rule

Only apply when the user's scene is in-domain. Don't force "pirouette" into a casual-walk clip — that's just noise.

## Expansion

When a new domain appears in production (e.g., barbering, welding, surgery), add a row here and keep the 3 – 5 most common verbs. Kept short deliberately — this file is meant to grow over time without bloating.
