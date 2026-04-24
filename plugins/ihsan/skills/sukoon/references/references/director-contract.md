---
type: skill-reference
skill: sukoon
topic: director contract
date: 2026-04-20
---

> [!info] Director contract
> Every Sukoon director subagent honors this input/output contract. Contract stability means directors are swappable — we can upgrade one without touching the others.

## Director input (what the orchestrator passes)

```yaml
# From orchestrator to director
run_manifest_path: /Users/.../Projects/ihsan/sukoon/runs/{run_id}/sukoon.yaml
stage_name: string              # e.g., "shotlist"
previous_stage_output: { ... }  # the `output` object from stage immediately before
revision_notes: string | null   # populated on revise
constraints: [string]           # brief.constraints, always passed through
is_revision: boolean            # false on first dispatch, true on revise
```

## Director behavior — 7-step protocol

1. **Read** `run_manifest_path` → verify `stage_name` status is `in_progress` (or transition from `pending` → `in_progress`)
2. **Read** the referenced Ihsan skill(s) at `.claude/skills/{skill}/SKILL.md` and reference files at `Skills/{skill}/`
3. **Execute** skill logic using `previous_stage_output` + `revision_notes`:
   - First dispatch: synthesize from scratch using previous stage + constraints
   - Revision: apply `revision_notes` as targeted patches, preserving work not being revised
4. **Write artifacts** to `{artifacts_dir}/stages/{stage_name}/` per stage-specific schema
5. **Render preview** at `stages/{stage_name}/preview.html` using the template (`Skills/sukoon/references/preview-template.html`) — populate all `{{tokens}}`
6. **Update** `sukoon.yaml`:
   - `stages[{name}].status = ready_for_review`
   - `stages[{name}].output = { ... }`
   - `stages[{name}].handoff_notes = string`
   - `stages[{name}].preview_html = stages/{stage_name}/preview.html`
   - `stages[{name}].artifacts = [ ... ]`
   - `stages[{name}].finished_at = ISO-now`
   - `stages[{name}].revisions_used += 1` IF `is_revision`
   - append to `events`
7. **Return** a ≤200-word summary to the orchestrator

## Director output (what's written to sukoon.yaml)

```yaml
stages:
  - name: {stage_name}
    director: {director-slug}
    status: ready_for_review
    started_at: ISO
    finished_at: ISO
    revisions_used: int
    output:
      # stage-specific structured fields — see manifest-schema.md
    handoff_notes: "1-3 sentences the next director must know"
    preview_html: stages/{stage_name}/preview.html
    artifacts:
      - path: relative-path
        type: reference-image | motion-clip | voice-line | music-bed | edit-list | final-cut
        shot_id: string | null
        description: string
```

## Hard rules

### Directors MUST
- Only read + write fields in their own `stages[{name}]` entry. Do not touch sibling stages.
- Preserve the `brief` block verbatim (no edits, even on revision).
- Write atomic updates — either all fields land, or none. On error, leave the manifest as-is and return the error to the orchestrator.
- Populate `preview_html` path and ensure the file actually exists at that path.
- Honor `revision_notes` as targeted patches — do not regenerate content untouched by the notes.
- Strip "Alex" references from any imported skill content (per Ihsan convention).
- Use wikilinks for any cross-vault references in `handoff_notes`.

### Directors MUST NOT
- Invoke the next director. That's the orchestrator's job.
- Halt the pipeline directly. They `return` with status; the orchestrator decides what to halt.
- Write to artifacts outside their stage subdirectory.
- Emit status `approved` — only Yasir (via orchestrator) approves.
- Read or modify global state (shell env, other runs).
- Generate real API assets in Stage 1 (placeholders only).

### Directors MAY
- Emit optional `sub_artifacts` beyond the required list (e.g., debug logs, intermediate prompt variants) — stored at `stages/{stage_name}/_debug/`.
- Request a dependency not in `previous_stage_output` by failing with an explicit "needs X from stage Y" message; orchestrator decides whether to rewind or override.
- Mark partial output with `status: in_progress` if hitting a soft limit (wall time, token ceiling); orchestrator decides whether to extend or escalate.

## Revision handling

Revisions are targeted patches, not rewrites:

```
revision_notes: "Tighten shot 4 to 3s. Rachel's hands visible in shot 2. Swap outro music."

→ Director updates:
  stages.shotlist.output.shots[3].duration_s = 3
  stages.shotlist.output.shots[1].visual += ", hands visible on mug"
  stages.shotlist.output.shots[-1].audio_cues updated

Untouched shots remain identical (same IDs, same fields).
```

If revision notes are ambiguous or contradictory:
- Director surfaces the ambiguity in `handoff_notes` and returns with a single best-faith interpretation.
- Escalation is the orchestrator's call, not the director's.

## Failure modes + returns

| Error | Director action | Return to orchestrator |
|---|---|---|
| Missing previous stage output | Do not write artifacts; do not update manifest | `{"error": "previous-stage-missing", "needed": "stage-name"}` |
| Schema validation fail | Do not write artifacts; do not update manifest | `{"error": "schema-violation", "detail": "..."}` |
| Soft budget hit (tokens/time) | Write partial artifacts; set `status: in_progress` | `{"error": "budget-hit", "partial": true}` |
| Adapter failure (Stage 2+) | Do not write artifacts | `{"error": "api-failure", "service": "...", "detail": "..."}` |

## Testability

Every director can be exercised in isolation by:
1. Creating a mock `sukoon.yaml` with `stages[{N-1}]` approved
2. Invoking the director agent with that path
3. Asserting `stages[{N}].status = ready_for_review` and schema compliance

Reference fixtures live at `Projects/ihsan/sukoon/runs/_template/fixtures/{stage}.yaml`.

## Summary return to orchestrator

Keep it under 200 words. Required fields:

```
Director: {name}
Stage: {stage_name}
Status: ready_for_review | in_progress | failed
Revisions used: X / 3
Preview: {artifacts_dir}/stages/{stage_name}/preview.html

Top 3 outputs: (1-2 bullets on what was produced)
Handoff: (copy of handoff_notes)
Flags: (anything the orchestrator should surface to Yasir — warnings, assumptions)
```

Orchestrator uses this summary to compose the chat review card.
