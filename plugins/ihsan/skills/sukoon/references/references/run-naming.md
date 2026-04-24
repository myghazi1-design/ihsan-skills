---
type: skill-reference
skill: sukoon
topic: run naming
date: 2026-04-20
---

> [!info] Run naming convention — locked 2026-04-20
> Every Sukoon run has an immutable ID of the form `{YYYY-MM-DD}-{kebab-slug}[-vN]`.

## Format

```
{YYYY-MM-DD}-{kebab-slug}[-vN]

where:
  YYYY-MM-DD    — date the run was created, local TZ
  kebab-slug    — 3-5 words, lowercase, hyphen-separated, derived from brief.goal
  -vN           — optional version suffix on same-day collisions (v2, v3, ...)
```

Examples:
- `2026-04-21-coffee-ritual-ugc`
- `2026-04-21-coffee-ritual-ugc-v2`  (second run with same slug on same day)
- `2026-04-22-product-hero-launch`
- `2026-04-22-brand-explainer-b2b`

## Slug derivation

From `brief.goal`, orchestrator derives the slug:

1. **Lowercase** the goal string
2. **Strip punctuation** (keep alphanumerics, spaces, hyphens)
3. **Drop stop-words**: `a, an, the, for, of, to, with, and, or, in, on, at, by, from`
4. **Keep 3-5 most content-bearing words** (prefer nouns + key adjectives)
5. **Kebab-case** them (join with hyphens)
6. **Truncate** to 40 characters max

Examples:

| brief.goal | → slug |
|---|---|
| "30s UGC ad, morning ritual, cozy + authentic" | `ugc-morning-ritual-cozy` |
| "Hero shot for new espresso machine product page" | `hero-espresso-machine-product` |
| "Brand explainer for B2B SaaS onboarding" | `brand-explainer-b2b-saas` |
| "Rachel Martinez talking-head ad for CoffeeBrand" | `rachel-talking-head-coffee` |

## Collision rule

If `{YYYY-MM-DD}-{kebab-slug}` already exists in `Projects/ihsan/sukoon/runs/`, append `-vN` starting at `-v2`:

```
First run    2026-04-21 → 2026-04-21-coffee-ritual
Second run   2026-04-21 → 2026-04-21-coffee-ritual-v2
Third run    2026-04-21 → 2026-04-21-coffee-ritual-v3
Next day     2026-04-22 → 2026-04-22-coffee-ritual  (fresh, no suffix)
```

## Fork naming

Forks don't use `-vN`. They use `-fork-{a..z}`:

```
Parent:   2026-04-21-coffee-ritual
Fork A:   2026-04-21-coffee-ritual-fork-a
Fork B:   2026-04-21-coffee-ritual-fork-b
```

A fork-of-a-fork appends another `-fork-{a..z}`:

```
2026-04-21-coffee-ritual-fork-a-fork-a
```

In practice, deep fork trees are rare — if Yasir needs more than two levels, that's a signal to `abort` and start a fresh brief.

## Why not UUID

Opaque. `ls` output would be unreadable, Obsidian graph nodes would be meaningless, and Yasir would always need a lookup table to remember which run was which.

## Why not pure slug

Collides on re-runs. Date prefix makes collisions impossible across days and resolvable within a day via `-vN`.

## Why date prefix first (not goal first)

`ls` sort order. `ls Projects/ihsan/sukoon/runs/` shows runs chronologically, newest last — which matches how Yasir reviews past work.

## Obsidian integration

Run IDs are wikilink-friendly. In daily notes, meetings, decisions:

```markdown
Kicked off [[2026-04-21-coffee-ritual-ugc]] this morning.
Forked from visual stage: [[2026-04-21-coffee-ritual-ugc-fork-a]].
```

Each run has a README at `{run-id}/README.md` that serves as the wikilink target, summarizing the brief + final outcome.

## Validator regex

```regex
^\d{4}-\d{2}-\d{2}-[a-z0-9]+(-[a-z0-9]+){2,4}(-v\d+|-fork-[a-z](-fork-[a-z])*)?$
```

Matches:
- `2026-04-21-coffee-ritual-ugc` ✓
- `2026-04-21-coffee-ritual-ugc-v2` ✓
- `2026-04-21-coffee-ritual-ugc-fork-a` ✓
- `2026-04-21-coffee-ritual-ugc-fork-a-fork-b` ✓
- `Coffee_Ritual` ✗ (underscores, caps)
- `2026-4-21-x` ✗ (unpadded date, slug too short)
