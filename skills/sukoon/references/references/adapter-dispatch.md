---
type: skill-reference
skill: sukoon
topic: adapter dispatch
date: 2026-04-20
---

> [!info] Adapter dispatch (Stage 2)
> Directors never call external APIs directly. Every API operation goes through a thin Bash adapter at `Projects/ihsan/sukoon/adapters/{service}.sh`. This reference documents the contract, the dispatch pattern, dependency-aware parallelism, and per-adapter capability matrix.

## 1. Adapter CLI contract

Every adapter honors the same interface:

```bash
./higgsfield.sh  --mode=stub|live --input=<path-or-yaml> > response.yaml
./elevenlabs.sh  --mode=stub|live --input=<path-or-yaml> > response.yaml
./suno.sh        --mode=stub|live --input=<path-or-yaml> > response.yaml
./ffmpeg.sh      --mode=stub|live --input=<path-or-yaml> > response.yaml
```

- `--mode` toggles between real API calls (`live`) and deterministic placeholder output (`stub`).
- `--input` accepts a file path or an inline YAML string.
- Output is valid YAML to stdout. All errors go to stderr and result in non-zero exit.
- Adapters live at `Projects/ihsan/sukoon/adapters/{service}.sh`. Never call them from a path outside this directory.

See [[Skills/sukoon/references/director-contract]] for the broader director-to-orchestrator contract.

### Input schema

```yaml
mode: live | stub
run_id: 2026-04-20-coffee-ritual-ugc
stage: visual | motion | audio-voice | audio-music | editor
budget_remaining_usd: 17.20
requests:
  - id: shot-01-subject-before
    params:
      # service-specific (see §3)
    depends_on: []               # empty → wave 0
  - id: shot-03-mid-transition
    params: { ... }
    depends_on: [shot-01-subject-before]   # wave 1
```

### Output schema

```yaml
status: completed | partial | failed | in_flight
started_at: ISO-8601
finished_at: ISO-8601 | null     # null when status = in_flight
results:
  - id: shot-01-subject-before
    status: completed | failed | in_flight
    asset_path: stages/visual/images/shot-01-subject-before-v1.png
    credits_used: 14
    usd_cost: 0.42
    job_id: hg_abc123            # service job ID; needed for poll-only re-invocation
    attempts: 1
    metadata: { ... }            # service-specific
  - id: shot-03-mid-transition
    status: failed
    error: "identity-drift-detected-by-nbp-quality-check"
    credits_used: 14
    usd_cost: 0.42
    retriable: true
total_credits_used: 28
total_usd: 0.84
errors:
  - id: shot-03-mid-transition
    class: asset-quality-below-threshold
    detail: "..."
```

## 2. Director-to-adapter mapping

| Director | Adapter | `stage` value in YAML |
|---|---|---|
| Visual Director | `higgsfield.sh` | `visual` |
| Motion Director | `higgsfield.sh` | `motion` |
| Audio Director (voice) | `elevenlabs.sh` | `audio-voice` |
| Audio Director (music) | `suno.sh` | `audio-music` |
| Editor Director | `ffmpeg.sh` | `editor` |

The Brief Interpreter, Shot Director, and Review Director do not call adapters — they are pure reasoning stages with no API side-effects.

## 3. Per-adapter capability matrix

| Adapter | Live mode | Stub mode | Sync / Async | Notes |
|---|---|---|---|---|
| `higgsfield.sh` | Shipped | Shipped | Async (poll) | Visual → `_submit_image`; Motion → `_submit_video`. `_make_poll_fn` is reusable for poll-only re-invocation. |
| `elevenlabs.sh` | Shipped | Shipped | Sync (~3-8s) | Single `xi-api-key` header. `$0.000167 × chars`. |
| `suno.sh` | Code shipped; live test deferred until sukoon-suno Keychain entry set | Shipped | Async (~2-3 min) | Best-guess API schema from public docs — first real contact may need adjustment. |
| `ffmpeg.sh` | Shipped | Shipped | Sync (local) | No network. Audio mixing + brand-lockup overlay included. SFX multi-track and sidechain ducking are known debt (Task 4.2 build log). |

> [!warning] Suno live mode
> `suno.sh` live mode is stub-only pending a valid `sukoon-suno` Keychain entry. Use `--mode=stub` until that credential is added.

## 4. Dispatch pattern (per stage)

The orchestrator follows this pattern for every adapter-calling stage:

```
1. Director synthesizes the request list
      ↓
2. Write stages/{stage}/adapter-request.yaml
      ↓
3. exec  adapters/{service}.sh --mode={stage_mode} \
            --input=stages/{stage}/adapter-request.yaml
      ↓
4. Capture stdout → stages/{stage}/adapter-response.yaml
      ↓
5. Persist relevant fields to:
     sukoon.yaml → stages[N].output.adapter_results[]
     sukoon.yaml → stages[N].async_jobs[] (if any result.status = in_flight)
      ↓
6. If status = completed → continue pipeline
   If status = in_flight → detach per resume-semantics.md
   If status = failed    → escalate per approval-ux.md §Escalation
```

`stage_mode` comes from `brief.stage_mode`, which defaults to `live` (see `config.yaml`).

## 5. Dependency-aware parallelism

Directors use `lib/dep-graph.sh`'s `topo_waves` to sort the request list into ordered waves before submitting to the adapter:

```
Wave 0 (no deps):  shot-01, shot-02           → exec in parallel (background)
                                                wait for both
Wave 1 (deps satisfied): shot-03, shot-04     → exec in parallel
                                                wait for both
Wave 2 ...
```

Within a single wave, each request is submitted as a background job. The orchestrator waits for the wave to complete before submitting wave N+1.

This means:
- Independent shots render concurrently, minimising wall-clock time.
- Chained shots (e.g., soul-mode reference carries through) are serialised correctly.
- Partial failure in wave N halts waves N+1 and beyond (do not proceed with broken dependencies).

`lib/dep-graph.sh` implementation is a Kahn's-algorithm topological sort; see the lib README for ID-collision and eval-injection guards required under macOS bash 3.2.57.

## 6. Stub-mode invariant

`--mode=stub` must produce byte-identical output for the same input (modulo timestamps):

- `asset_path` points to `PLACEHOLDER-{id}.md` files, not real assets.
- `credits_used: 0`, `usd_cost: 0.00` on every result.
- No network calls, no Keychain access, no ffmpeg invocation.

This is the foundation of the tier-1 regression test (spec §8.1): re-running the golden looksmaxxing fixture in stub mode must diff cleanly against the committed golden output.

## 7. Error handling

Orchestrator-level response per error class (adapters surface these in `errors[].class`):

| Class | Orchestrator response |
|---|---|
| `transient` | Retry once; on second failure, escalate |
| `quota-exceeded` | Halt; escalate with `retry in N min` / `abort` verbs |
| `content-policy-rejected` | Halt; surface rejection detail; offer `revise` only (no auto-retry) |
| `asset-quality-below-threshold` | Mark `retriable: true`; Review Director decides whether to reroll |
| `schema-violation` | Halt; treat as bug; write post-mortem |

Adapters retry exactly once on `transient` errors internally. On second internal failure they return `retriable: true` and surface the error for the orchestrator to decide. Adapters never silently loop.

## 8. Known debt

- `suno.sh` live API schema is best-effort from public docs. First real live call may surface schema mismatches requiring adapter body edits.
- SFX multi-track mixing and sidechain ducking in `ffmpeg.sh` are not yet implemented (see build log 4.2). Audio duck-under-VO is simulated via `amix` volume scaling.
- Freeze-frame fallback in `ffmpeg.sh` (for `drift_detected: true` clips) is not yet implemented.
- Poll-only adapter invocation (`--poll-only --job-id=X`) for `/sukoon resume` is documented in [[Skills/sukoon/references/resume-semantics]] but relies on `_make_poll_fn` in `higgsfield.sh` being reusable — confirmed in build log 2.3, formalized here.
