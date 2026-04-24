---
type: skill-reference
skill: sukoon
topic: manifest schema
date: 2026-04-20
---

> [!info] `sukoon.yaml` — single source of truth for a Sukoon run
> Every run lives at `Projects/ihsan/sukoon/runs/{run-id}/` with `sukoon.yaml` at its root. Directors read + update this file at every stage. Yasir can edit it directly during approval gates.

## Full schema

```yaml
# ───── identity ─────
run_id: 2026-04-21-coffee-ritual-ugc     # format: {YYYY-MM-DD}-{kebab-slug}[-vN]
codename: sukoon
parent_project: ihsan
version: 1                                # schema version, increment on breaking changes
created_at: 2026-04-21T09:00:00-07:00     # ISO 8601 with TZ
updated_at: 2026-04-21T12:30:00-07:00

# ───── brief (input) ─────
brief:
  raw: |                                  # Yasir's original input, verbatim
    30-second UGC ad for CoffeeBrand X...
  goal: string                            # one-line distilled goal
  platform: enum                          # TikTok | Instagram | YouTube | X | Generic
  duration_s: integer                     # target clip length in seconds
  aspect: enum                            # 9:16 | 1:1 | 16:9 | 4:5
  creator_persona: string | null          # e.g., "Rachel Martinez", null if not a UGC run
  tool_hint: enum | null                  # kling-video | seedanceX | mixed | null
  budget_usd_cap: float                   # default 15.00 (decision #5)
  revision_budget_per_stage: integer      # default 3 (decision #1)

# ───── stages (pipeline state) ─────
stages:
  - name: enum                            # brief | shotlist | visual | motion | audio | review | editor
    director: string                      # director slug, e.g., "shot-director"
    status: enum                          # pending | in_progress | ready_for_review | approved | revise | failed
    started_at: ISO 8601 | null
    finished_at: ISO 8601 | null
    revisions_used: integer               # count toward revision_budget_per_stage
    revision_notes: string | null         # populated by Yasir on `revise: {notes}`
    input_ref: string | null              # optional pointer to previous stage's output field
    output: { ... }                       # stage-specific structured data (see per-director specs)
    handoff_notes: string                 # 1-3 sentences the next director must know
    preview_html: relative-path           # e.g., stages/shotlist/preview.html
    artifacts: [ { path, type, description, shot_id? } ]

# ───── artifact roots ─────
artifacts_dir: relative-path              # Projects/ihsan/sukoon/runs/{run-id}/

# ───── cost tracking ─────
cost_tracker:
  tokens_used: integer                    # cumulative agent tokens
  api_usd: float                          # cumulative external API spend
  budget_usd_remaining: float             # brief.budget_usd_cap - api_usd

# ───── event log ─────
events:
  - "{ISO 8601}: {event description}"     # append-only human-readable log

# ───── escalations (populated on breach) ─────
escalations:
  - at: ISO 8601
    stage: string
    reason: enum                          # revision-budget-exhausted | cost-cap-exceeded | api-failure | timeout
    detail: string
    notified:                             # decision #7: halt chat + Discord mirror
      chat: true
      discord: true
    resolution: string | null             # Yasir's response, filled after
```

## Status enum semantics

| Status | Meaning | Next transition |
|---|---|---|
| `pending` | Stage exists but hasn't started | → `in_progress` (when orchestrator dispatches director) |
| `in_progress` | Director actively working | → `ready_for_review` (on success) or `failed` (on adapter error) |
| `ready_for_review` | Director done, awaiting Yasir | → `approved` or `revise` or `aborted` |
| `approved` | Yasir approved, pipeline advances | terminal for this stage |
| `revise` | Yasir requested revision | → `in_progress` (re-dispatch with revision_notes) |
| `failed` | Director hit unrecoverable error | → escalation |

## Stage-specific `output` shapes

### brief

```yaml
output:
  refined_brief: string                   # cleaned, single-paragraph brief
  constraints: [string]                   # e.g., ["dialogue-heavy", "kling-native-audio", "TikTok-9:16"]
  tool_routing:
    images: enum                          # nbp | NBPHero | external | skip
    motion: enum                          # kling-video | seedanceX | mixed | external
    voice: enum                           # voice-elevenlabs | kling-native | external | skip
    music: enum                           # external | skip
  assumptions: [string]                   # e.g., ["creator matches @ugcwithrach biography"]
```

### shotlist

```yaml
output:
  shots:
    - id: shot-01
      duration_s: integer
      purpose: enum                       # hook | setup | payoff | beat | outro
      visual: string                      # camera + composition description
      motion: string                      # what moves and how
      dialogue: string | null             # spoken lines if any
      audio_cues: [string]                # sfx, music beats
  asset_requirements:
    - type: enum                          # reference-image | motion-clip | voice-line | music-bed
      count: integer
      source: enum                        # nbp | NBPHero | kling-video | seedanceX | voice-elevenlabs
```

### visual

```yaml
output:
  prompts:                                # one per shot
    - shot_id: shot-01
      tool: enum                          # nbp | NBPHero
      prompt_text: string
      parameters: { ... }                 # tool-specific
  artifacts:                              # placeholders in Stage 1, real in Stage 2
    - path: stages/visual/images/shot-01-v1.png
      type: reference-image
      shot_id: shot-01
      description: string
```

### motion

```yaml
output:
  prompts:
    - shot_id: shot-01
      tool: enum                          # kling-video | seedanceX
      prompt_text: string
      parameters: { ... }
      chain_context: string | null        # for multi-clip chains
  artifacts:
    - path: stages/motion/clips/shot-01.mp4
      type: motion-clip
      shot_id: shot-01
      duration_s: float
      description: string
```

### audio

```yaml
output:
  voice_lines:
    - shot_id: shot-01
      text: string
      voice_id: string                    # elevenlabs voice id
      reference_audio: string | null      # for voice-lock
  music_cues:
    - start_s: float
      duration_s: float
      mood: string
      source: enum                        # external | suno | skip
  artifacts:
    - path: stages/audio/voice/shot-01.mp3
      type: voice-line
      shot_id: shot-01
```

### review

```yaml
output:
  verdicts:
    - asset: relative-path
      axis: { realism, hands, face, background, composition, artifacts }  # 1-5 scores
      verdict: enum                       # ship | one-more-pass | re-prompt
      notes: string
  escalations: [shot_id]                  # assets Review Director rejects
  thumb_recommendation: relative-path | null
```

### editor

```yaml
output:
  edit_list:                              # Stage 1: CapCut-importable JSON
    format: enum                          # capcut | ffmpeg | both
    timeline:
      - source: relative-path
        in_s: float
        out_s: float
        track: enum                       # video | voice | music | sfx
  ffmpeg_commands: [string] | null        # Stage 2+
  artifacts:
    - path: final.mp4                     # Stage 2+
      type: final-cut
```

## Obsidian rendering

`sukoon.yaml` opens cleanly in Obsidian's properties view thanks to frontmatter-compatible syntax (no top-level `---` needed — it IS the whole file). Wikilinks in `handoff_notes` / `events` resolve in the graph.

## Versioning

Breaking schema changes bump `version`. Migration scripts live at `Projects/ihsan/sukoon/plans/migrations/`. Non-breaking additions (new optional fields) don't require version bumps but are noted in `events`.
