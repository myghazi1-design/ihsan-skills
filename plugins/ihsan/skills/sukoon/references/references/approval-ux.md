---
type: skill-reference
skill: sukoon
topic: approval gate UX
date: 2026-04-20
---

> [!info] Approval gate UX
> When a director finishes its stage, the Sukoon orchestrator halts the pipeline and surfaces a review card in chat. Yasir approves, revises, forks, or aborts. Every gate also writes an HTML preview to disk (decision #4).

## Gate lifecycle

```
Director finishes
    ↓
Writes sukoon.yaml updates + preview.html
    ↓
Orchestrator posts chat card → halts
    ↓
Yasir replies with verb
    ↓
Orchestrator parses → routes
```

## Chat review card — canonical shape

```markdown
## 🎬 Stage: {Director Name} — ready for review

**Run:** `{run_id}`
**Revisions used:** {revisions_used} / {revision_budget_per_stage}
**Preview:** `open {artifacts_dir}/stages/{stage_name}/preview.html`
**Handoff notes:** {handoff_notes}

**Inline summary:**
{stage-specific bullets — 5-8 lines max}

**Options:**
- `approve` — proceed to {Next Director Name}
- `revise: {your notes}` — re-run {Director Name}
- `fork` — branch a new run from this stage
- `abort` — mark run failed

— paused —
```

Rules:
- Card is ≤400 words. Long output lives in `preview.html`.
- Always include the full `open` path so Yasir can paste it into Terminal.
- Never include raw artifacts (images, audio) in chat — link to them via preview.
- On final stage (editor), replace "proceed to {Next Director Name}" with "finalize run".

## Reply verb grammar

Orchestrator parses Yasir's reply using these patterns (first match wins):

| Pattern (regex, case-insensitive) | Action |
|---|---|
| `^approve\b` | Advance to next stage |
| `^revise(?:\s*:\s*(.+))?$` | Re-run current stage with captured notes as `revision_notes` |
| `^fork\b` | Create new run branched from current stage, pause parent |
| `^abort\b` | Mark run `failed`, write post-mortem, halt pipeline |
| `^budget\s+\+(\d+)\b` | (Escalation only) extend revision budget by N |
| `^replan(?:-from-)?(brief\|shotlist)?$` | (Escalation only) rewind to named stage |
| anything else | Ask Yasir to clarify — do NOT assume |

### Revise with multi-line notes

```
revise: 
- Tighten shot 4 to 3s
- Rachel's hands visible in shot 2
- Swap "morning ritual" for "daily reset"
```

Everything after `revise:` (including linebreaks) becomes `revision_notes`. Orchestrator trims whitespace but preserves formatting.

## Revision budget & escalation

Decision #1: 3 revisions per stage. Counter increments each time Yasir replies `revise`. When `revisions_used == revision_budget_per_stage`, the next `revise` triggers escalation instead of re-dispatch.

### Escalation flow (decision #7)

On budget exhaustion, cost-cap breach, or adapter failure:

1. Orchestrator appends to `sukoon.yaml` → `escalations: [...]`
2. Posts in chat:

```markdown
## 🚨 Escalation: {reason}

**Stage:** {stage_name}
**Detail:** {detail}

**Options:**
- `budget +N` — extend revision budget by N
- `replan-from-brief` — rewind to brief stage (recomputes all downstream)
- `replan-from-shotlist` — rewind to shotlist
- `abort` — mark run failed
```

3. Mirrors the message to Discord (via `mcp__plugin_discord_discord__reply`) using the user's configured channel — so Yasir gets a push notification even if Terminal is backgrounded.
4. Halts pipeline until Yasir responds.

### Escalation reasons (enum)

| Reason | Trigger |
|---|---|
| `revision-budget-exhausted` | Yasir hits `revise` at max budget |
| `cost-cap-exceeded` | `api_usd` estimate > `budget_usd_cap` (Stage 2+) |
| `api-failure` | Adapter returns non-retryable error (Stage 2+) |
| `timeout` | Stage wall time > 30 min without director finish |
| `schema-violation` | Director output fails schema validation |

## HTML preview rendering rules

Every stage renders `stages/{stage_name}/preview.html`, self-contained, Apple design language (see `preview-template.html`).

Must include:
- Run ID, stage name, director, revisions used
- Structured output table (shots / prompts / artifacts, depending on stage)
- Thumbnail grid for visual/motion/audio stages (linked to artifact files; image `<img src>` paths are relative, so the HTML opens correctly from the runs folder)
- Handoff notes, prominent
- The exact four reply verbs at the bottom — copy-paste friendly

Must NOT include:
- Inline large images (link to artifact files; browser handles rendering)
- External scripts or stylesheets (CSP-safe, offline-openable)
- Any claim beyond what's in `sukoon.yaml` — preview = direct render of manifest state, nothing fabricated

## Fork mechanics

`fork` creates a sibling run inheriting state through the current stage:

```
Projects/ihsan/sukoon/runs/
├── 2026-04-21-coffee-ritual-ugc/          # original (paused at visual stage)
└── 2026-04-21-coffee-ritual-ugc-fork-a/   # fork, starts from visual re-run
```

Fork naming: `{parent-run-id}-fork-{a..z}`, increment alphabetically. Forks copy `sukoon.yaml` up through the current stage, mark later stages `pending`, and start a new event log.

Forks are independent after creation — approving one doesn't approve the other. Yasir can later `abort` either.

## What Yasir cannot do at a gate

- Skip a stage (use `fork` + edit the forked `sukoon.yaml` manually if truly needed)
- Advance without approving the current stage (`approve` is the only forward verb)
- Merge two runs (future work — Stage 3 may add a `merge` verb for selective-best-of-3)

## Quiet mode

Yasir can set `auto_advance: true` on any stage's entry in `sukoon.yaml` before running Sukoon. That stage will skip the gate. Used sparingly — mostly for re-runs where Yasir already approved identical output in a prior run. Stage 3 adds pattern-based auto-advance.
