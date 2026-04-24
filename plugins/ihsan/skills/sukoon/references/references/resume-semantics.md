---
type: skill-reference
skill: sukoon
topic: resume semantics
date: 2026-04-20
---

> [!info] `/sukoon resume` and detach semantics
> When an adapter returns jobs still in flight, the orchestrator detaches rather than blocking the session indefinitely. This reference documents the detach trigger, the manifest state it writes, the detach card surfaced to Yasir, and the full resume flow.

## 1. Detach trigger

Detach fires when either condition is true:

1. **In-flight adapter result**: any entry in `stages[N].output.adapter_results[]` has `status: in_flight` after the poll-loop wall time expires.
2. **Poll-loop timeout**: `lib/poll-loop.sh`'s `poll_until_done` reaches its `detach_wall_time_s` limit (default 300s / 5 min, from `config.yaml`) without all jobs completing.

The two conditions overlap — in practice, `poll_until_done` is the primary mechanism and sets `in_flight` status on whatever remains when it times out.

## 2. Manifest state written on detach

When detach fires, the orchestrator writes to `sukoon.yaml`:

```yaml
stages:
  - name: motion
    status: in_progress          # stays in_progress until all jobs resolve
    output:
      adapter_results:
        - id: shot-01
          status: completed
          asset_path: stages/motion/clips/shot-01.mp4
          job_id: hg_abc123
        - id: shot-04
          status: in_flight      # still running
          job_id: hg_def456
          submitted_at: 2026-04-20T18:30:00-07:00
    async_jobs:
      - id: shot-04              # one entry per in-flight job
        job_id: hg_def456
        submitted_at: 2026-04-20T18:30:00-07:00
        last_status: in_flight
        last_polled_at: 2026-04-20T18:35:00-07:00
events:
  - "2026-04-20T18:35:10-07:00: Detached from motion stage — 1 job still in flight (hg_def456). Run /sukoon resume 2026-04-20-coffee-ritual-ugc to continue."
```

`async_jobs[]` is the authoritative list the resume flow uses. It is cleared (set to `[]`) when all jobs resolve.

## 3. Detach card (chat message)

When detach fires, the orchestrator posts this message in chat:

```
⏳ Stage {Name}: {N} render(s) still in flight. Elapsed: {elapsed_s}s.

Reply `/sukoon resume {run_id}` to continue when renders complete.
Higgsfield job IDs remain valid for hours — no rush.
```

- `{Name}` is the stage display name (e.g., "Motion").
- `{N}` is the count of in-flight jobs.
- `{elapsed_s}` is seconds since stage start.

The orchestrator does NOT mirror the detach card to Discord — it is a low-urgency pause, not an escalation. Discord gets the escalation only if the hard timeout fires (see §6).

## 4. Resume verb grammar

```
/sukoon resume {run_id}
```

`{run_id}` follows the format in [[Skills/sukoon/references/run-naming]]. If `{run_id}` is omitted and there is exactly one active run with `async_jobs[]` non-empty, the orchestrator infers it. If there is ambiguity, ask Yasir to specify.

## 5. Resume flow (7 steps)

When Yasir invokes `/sukoon resume {run_id}`:

1. **Read manifest** — load `{run_id}/sukoon.yaml`. Identify all stages with `async_jobs[]` non-empty.
2. **Re-invoke adapter in poll-only mode** — for each in-flight job in `async_jobs[]`, call the responsible adapter's poll path:
   - Higgsfield: reuses `_make_poll_fn` from `higgsfield.sh` (confirmed in build log 2.3). This is equivalent to `--poll-only --job-id={job_id}` semantics.
   - ElevenLabs: synchronous; no async jobs expected — if `in_flight` appears, treat as transient and re-invoke normally.
   - Suno: poll using stored job ID.
   - ffmpeg: no async; if `in_flight` appears, re-invoke (idempotent).
3. **Check resolution**:
   - If all jobs complete → proceed to step 4.
   - If still in flight → update `last_polled_at` and `last_status` in `async_jobs[]`, compute elapsed time, surface a fresh detach card (§3), and halt.
4. **Download missing assets** — for each job that completed, ensure `asset_path` file exists on disk. If adapter returned a URL, download to the expected local path.
5. **Populate adapter_results** — update `stages[N].output.adapter_results[]` with final status, asset_path, credits_used, usd_cost for each resolved job.
6. **Clear async_jobs** — set `stages[N].async_jobs: []`.
7. **Flip stage status and advance**:
   - Set `stages[N].status = ready_for_review`.
   - Run adaptive-gating check (see [[Skills/sukoon/references/gating-rules]]).
   - If gate fires → render preview, post review card, halt.
   - If no gate → auto-advance to next stage.

## 6. Hard timeout

`total_stage_timeout_s` from `config.yaml` (default: 1800s / 30 min) is the outer wall time per stage, measured from `stages[N].started_at`.

If Yasir does not `/sukoon resume` and the stage remains unresolved for 30 min:

1. Orchestrator auto-escalates per [[Skills/sukoon/references/approval-ux#Escalation flow]].
2. Escalation reason: `timeout`.
3. Discord mirror fires (unlike detach card).
4. Yasir must reply with `replan-from-{stage}` or `abort` — no auto-retry.

The 30-min total breaks down as: up to 5 min in-session polling (`detach_wall_time_s: 300`) + up to 25 min detached. Higgsfield job IDs remain valid well beyond this window, so a manual resume after 30 min is still possible via `abort` + `replan-from-motion` if Yasir decides to continue.

## 7. Resume-from-crash semantics

`/sukoon resume` is also the recovery verb for session crashes (not just detach). If a stage is `in_progress` with no `async_jobs[]` (director was computing, not polling), the orchestrator re-dispatches the director (directors are idempotent under same input). This is documented in spec §7.4 and SKILL.md "Mode: resume run".

## 8. Known debt

- `--poll-only --job-id=X` is not yet a formal CLI flag on adapters — `_make_poll_fn` in `higgsfield.sh` covers the Higgsfield case internally, but a unified `--poll-only` interface is not yet standardized across all adapters. Formalizing this would simplify the resume flow.
- Suno poll-only path is untested in live mode (credential not yet provisioned — see build log 3.2).
