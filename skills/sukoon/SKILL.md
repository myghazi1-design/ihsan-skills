---
name: sukoon
description: Use when starting, resuming, or advancing an end-to-end AI video production run under the Ihsan skill library. Sukoon is the director-agent pipeline that chains Brief → Shot → Visual → Motion → Audio → Review → Editor stages with human approval gates at every handoff. Triggers on /sukoon, "start a sukoon run", "resume sukoon", "new video brief", "run the pipeline", mentions of sukoon/pipeline/director-agent in a video production context. Also triggers when the user approves, revises, forks, or aborts at a Sukoon gate.
---

# Sukoon — AI video production pipeline orchestrator

Sukoon is a sub-project of [[Projects/ihsan/README|Ihsan]]. It turns a single video brief into a full production pipeline run, with 7 directors and human approval gates. Read the [[Projects/ihsan/sukoon/README|Sukoon project README]] for full design context.

## When to invoke

- Yasir types a video brief or says "start a Sukoon run"
- Yasir replies at a gate with `approve` / `revise: {notes}` / `fork` / `abort` on an existing run
- Yasir asks "what's the next step on {run-id}" or "resume {run-id}"
- Yasir wants to see the status of all active runs

## Reference material

Read these before acting:
- [[Skills/sukoon/references/manifest-schema|manifest-schema.md]] — the full `sukoon.yaml` structure
- [[Skills/sukoon/references/director-contract|director-contract.md]] — input/output contract every director honors
- [[Skills/sukoon/references/approval-ux|approval-ux.md]] — chat review card + reply verbs + escalation
- [[Skills/sukoon/references/run-naming|run-naming.md]] — `{YYYY-MM-DD}-{kebab-slug}[-vN]` convention
- [[Skills/sukoon/references/preview-template.html|preview-template.html]] — Apple-themed HTML gate preview

## Pipeline

```
Brief Interpreter → Shot Director → Visual Director → Motion Director → Audio Director → Review Director → Editor Director
```

Each director's brief lives at `Skills/sukoon/directors/{N}-{name}.md`. You (the orchestrator) dispatch the director as a subagent with that brief loaded.

## Mode: new run

When Yasir provides a brief:

1. Parse the brief — extract goal, platform, duration if stated
2. Derive run ID per [[Skills/sukoon/references/run-naming]]:
   - Date prefix `{YYYY-MM-DD}` (today's date)
   - Slug: 3-5 kebab-cased content words from the goal
   - Check `Projects/ihsan/sukoon/runs/` for collision → append `-v2` etc.
3. Create `Projects/ihsan/sukoon/runs/{run_id}/`
4. Copy `Projects/ihsan/sukoon/runs/_template/` contents into it
5. Populate `sukoon.yaml` fields: `run_id`, `created_at`, `brief.raw`, `brief.budget_usd_cap: 15`, `brief.revision_budget_per_stage: 3`
6. Confirm the run creation with Yasir: "Created run `{run_id}` at `{artifacts_dir}`. Starting Brief Interpreter..."
7. Dispatch Director 1 (Brief Interpreter) — see **Director dispatch** below

## Mode: resume run

When Yasir refers to an existing run:

1. Read `Projects/ihsan/sukoon/runs/{run_id}/sukoon.yaml`
2. Identify the current stage: first entry in `stages[]` with status `pending`, `in_progress`, or `ready_for_review`
3. If the current stage is `ready_for_review`, show the chat review card (don't re-dispatch)
4. If `pending` or `in_progress`, dispatch that stage's director

## Mode: gate reply

When Yasir replies `approve` / `revise: {notes}` / `fork` / `abort` at a gate:

1. Identify the run and current stage (may be implicit from conversation context; if ambiguous, ask)
2. Parse the reply verb using the grammar in [[Skills/sukoon/references/approval-ux]]
3. Act:
   - **approve**: set `stages[{N}].status = approved`, `events.append("{ISO}: {stage} approved")`, dispatch next director
   - **revise**: set `stages[{N}].status = revise`, `revisions_used += 1`, store `revision_notes`, check budget; if over → escalate; else re-dispatch same director
   - **fork**: copy current run state through current stage to new run ID `{parent}-fork-{a..z}`, pause parent, prompt Yasir on what the fork should do differently
   - **abort**: set root `status = failed`, write `{artifacts_dir}/POST-MORTEM.md` with the reason, halt

## Director dispatch

Dispatch a director as a subagent with:
- **Model**: opus for Brief Interpreter (complex parsing) and Review Director (quality judgment); sonnet for Shot, Visual, Motion, Audio, Editor (mechanical transformation with reference input) — subject to cost-awareness
- **Subagent type**: general-purpose
- **Prompt**: the director's brief (`Skills/sukoon/directors/{N}-{name}.md`) followed by:
  ```
  Run manifest: {absolute path to sukoon.yaml}
  Stage name: {stage}
  Previous stage output: (paste from sukoon.yaml)
  Revision notes: {notes if revision, else null}
  Is revision: {bool}

  Execute per your brief. Write artifacts, render preview.html, update sukoon.yaml, return ≤200-word summary.
  ```

## After director returns

1. Read the updated `sukoon.yaml` to verify the director actually updated it
2. If status is `ready_for_review` and `preview_html` exists:
   - Render the chat review card using the director's return summary (see [[Skills/sukoon/references/approval-ux]])
   - HALT — wait for Yasir's reply verb
3. If status is `failed` or director returned an error → escalate:
   - Write escalation entry in `sukoon.yaml.escalations`
   - Post chat escalation message
   - Mirror to Discord via `mcp__plugin_discord_discord__reply` if configured
   - HALT

## Escalation (decision #7)

On revision-budget-exhausted, cost-cap-exceeded (Stage 2+), api-failure, timeout, or schema-violation:

1. Append `escalations[]` entry in `sukoon.yaml`
2. Post the escalation card in chat (template in [[Skills/sukoon/references/approval-ux#Escalation flow]])
3. Mirror to Discord — use `mcp__plugin_discord_discord__reply` with the same message body, or compose a shortened version linking to the preview HTML
4. Halt until Yasir replies with `budget +N`, `replan-from-{stage}`, or `abort`

## Cost awareness (decision #5)

- `brief.budget_usd_cap` defaults to $15 but aim lower
- For Stage 1 (no API calls), track tokens only
- Before dispatching expensive directors (Motion = Opus+reasoning; Review = Opus), preflight estimate and note in events
- On Stage 2+: before each adapter call, compute estimated cost; abort with escalation if cumulative + next call > budget_usd_cap

## Final delivery

When the Editor Director stage is approved:

1. Set `sukoon.yaml.status = completed` at the root
2. Write `{artifacts_dir}/RUN-SUMMARY.md` with: total time, revisions per stage, tokens used, final asset count, `/review-video` verdict summary
3. Append a line to `Daily/{today}.md` under "Sukoon runs": `- [[{run_id}]] — {one-line summary}`
4. (Stage 2+) upload `final.mp4` to Google Drive via MCP, post link to Discord
5. Post final success card in chat with preview + delivery paths

## Never do these

- Don't invoke the next director yourself — always dispatch a subagent per the director's brief
- Don't approve a stage without Yasir's explicit `approve` reply (auto-advance is Stage 3 only)
- Don't modify `brief` block after run creation
- Don't touch other runs when working on one
- Don't skip rendering the HTML preview — every gate must have one (decision #4)
- Don't make real API calls in Stage 1 — emit prompts + placeholders only

## See also

- [[Projects/ihsan/sukoon/README|Sukoon design spec]] — full architecture, 3-stage roadmap, decisions
- [[Projects/ihsan/README|Ihsan]] — parent project with the 12 skills Sukoon orchestrates
- [[.claude/skills/sukoon-edit/SKILL]] — the Editor Director's skill

---

## Adapter dispatch (Stage 2)

Full detail: [[Skills/sukoon/references/adapter-dispatch|adapter-dispatch.md]]

Directors never call external APIs directly. Every API operation goes through a Bash adapter at `Projects/ihsan/sukoon/adapters/{service}.sh` invoked as:

```bash
./{service}.sh --mode={stage_mode} --input=<path> > response.yaml
```

The `stage_mode` toggle comes from `brief.stage_mode` in `sukoon.yaml`, which defaults to `live` (set in `config.yaml`). Override per-run by setting `brief.stage_mode: stub` before dispatch — stub mode returns placeholder assets with zero credit burn, enabling tier-1 regression testing.

**Default config values** (from `.claude/skills/sukoon/config.yaml`):

| Key | Default | Meaning |
|---|---|---|
| `default_stage_mode` | `live` | Use real APIs unless overridden |
| `default_budget_usd_cap` | `20` | USD hard cap per run |
| `default_revision_budget_per_stage` | `3` | Revisions before escalation |
| `poll_interval_s` | `30` | Seconds between poll attempts |
| `detach_wall_time_s` | `300` | 5 min in-session poll, then detach |
| `total_stage_timeout_s` | `1800` | 30 min hard timeout per stage |

Director-to-adapter mapping:

| Director | Adapter |
|---|---|
| Visual | `higgsfield.sh` |
| Motion | `higgsfield.sh` |
| Audio (voice) | `elevenlabs.sh` |
| Audio (music) | `suno.sh` |
| Editor | `ffmpeg.sh` |

After dispatch, capture adapter stdout to `stages/{stage}/adapter-response.yaml` and persist `results[]` to `sukoon.yaml.stages[N].output.adapter_results`. If any result has `status: in_flight`, follow the detach flow in **Mode: resume detached** below.

---

## Mode: resume detached

Full detail: [[Skills/sukoon/references/resume-semantics|resume-semantics.md]]

When an adapter returns jobs still in flight (e.g., Higgsfield video renders), the orchestrator detaches instead of blocking the session:

1. Save `stages[N].async_jobs: [{id, job_id, submitted_at, last_status}]` to manifest.
2. Keep `stages[N].status = in_progress`.
3. Append to `events[]`: `"{ISO}: Detached from {stage} — {N} job(s) still in flight. Run /sukoon resume {run_id} to continue."`
4. Post detach card in chat: `"⏳ Stage {Name}: {N} render(s) still in flight. Elapsed: {elapsed_s}s. Reply /sukoon resume {run_id} to continue."`
5. HALT — do not block, do not poll further.

On `/sukoon resume {run_id}`:

1. Read manifest → find stages with `async_jobs[]` non-empty.
2. Re-invoke responsible adapter in poll-only mode (Higgsfield: reuses `_make_poll_fn` from `higgsfield.sh`).
3. If all jobs complete → download assets → populate `adapter_results[]` → clear `async_jobs[]` → flip `status = ready_for_review` → run adaptive-gating check.
4. If still in flight → update `last_polled_at`, surface fresh detach card, HALT.
5. Hard timeout: `total_stage_timeout_s` (1800s / 30 min from `config.yaml`) measured from `stages[N].started_at`. On breach → escalate (reason: `timeout`) with Discord mirror.

> [!info] Also covers crash recovery
> `/sukoon resume` handles both detached async jobs AND session crashes. For a stage `in_progress` with no `async_jobs[]` (director was computing), re-dispatch the director (directors are idempotent under same input). See existing **Mode: resume run** section above.

---

## Mode: pause

When Yasir types `/sukoon pause` (optionally with `{run_id}`):

1. **Identify run** — if `{run_id}` omitted, use the active run from context; if ambiguous, ask.
2. **If a stage is `in_progress`** (adapter calls or director still running):
   - Wait for current adapter wave to complete, OR detach if wall time > 5 min (detach card + hold).
   - Once the current stage resolves, render its gate preview (`stages/{stage}/preview.html`).
   - Present standard reply verbs: `approve` / `revise: {notes}` / `fork` / `abort`.
   - HALT.
3. **If the most recent stage is already `ready_for_review`** — render its preview; present standard verbs; HALT.
4. **No-op** if the run is already halted at a gate (nothing pending, already awaiting a reply verb). Respond: "Run `{run_id}` is already paused at the `{stage}` gate."
5. **Write to events**: `"{ISO}: Manually paused before stage {next_stage_name} auto-dispatch"`

Pause inserts a gate; it does not abort, fork, or modify any pipeline state beyond halting auto-advance.

---

## Pre-flight cost estimate

Full detail: [[Skills/sukoon/references/cost-estimate|cost-estimate.md]]

> [!warning] $20 default cap
> `default_budget_usd_cap: 20` in `config.yaml`. Override per-run via `brief.budget_usd_cap`.

Before dispatching any adapter-calling stage:

1. Call `lib/cost-tracker.sh estimate_image` / `estimate_video` for each request in the director's request list.
2. Convert credits → USD via `lib/higgsfield-rates.yaml`. ElevenLabs and Suno adapters compute their own USD (`$0.000167 × chars`; `$0.001 × credits`) — no rate-table lookup needed for them.
3. Write `stages[N].cost_estimate_usd` to manifest.
4. Compare: `projected = cost_tracker.api_usd + stages[N].cost_estimate_usd`
   - `projected > budget_usd_cap` → escalate (reason: `cost-cap-exceeded`). HALT.
   - `projected > budget_usd_cap × 0.80` → warning gate (non-halting). Continue unless Yasir replies `replan`.
5. After adapter returns → sum `results[].usd_cost` → call `add_actual_usd` → log estimate-vs-actual variance to `audit.jsonl`.
6. Rate-table staleness: `stale_check` warns if `lib/higgsfield-rates.yaml snapshot_date` is older than 90 days.

---

## Adaptive gating (Stage 2 default)

Full detail: [[Skills/sukoon/references/gating-rules|gating-rules.md]]

Stage 2 defaults to **2 gates per run** that never skip:

1. **Shotlist gate** — after Shot Director. Creative structure locks here; all downstream stages derive from it.
2. **Editor / final cut gate** — after Editor Director. Always the last human checkpoint before Drive upload + Discord post.

All other stages auto-advance silently when conditions are clean.

**Intermediate gate triggers** (any of these inserts a gate):

| Trigger | Condition |
|---|---|
| Review non-ship | Any `review.verdicts[*].verdict ∈ [one-more-pass, re-prompt]` |
| Cost breach | `projected_api_usd > budget_usd_cap` |
| Cost warning (80%) | `projected_api_usd > budget_usd_cap × 0.80` (non-halting) |
| Policy rejection | Adapter returned `content-policy-rejected` |
| Manual pause | Yasir typed `/sukoon pause` |
| Revision budget exhausted | `revisions_used == revision_budget_per_stage` on a `revise` reply |

Decision flow after each stage completes:
- Shotlist? → GATE.
- Editor? → GATE.
- Review? → any non-ship verdict → GATE; else auto-advance.
- Other stage → pre-flight cost check → policy check → auto-advance if clean.

> [!tip] Stage 3: trust scores
> Per-director trust scores that relax the "any non-ship → gate" criterion after accumulated clean runs are documented in `gating-rules.md` as a Stage 3 roadmap item. Not implemented in Stage 2.
