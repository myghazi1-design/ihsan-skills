---
type: skill-reference
skill: sukoon
topic: cost estimate
date: 2026-04-20
---

> [!info] Pre-flight cost estimation + budget enforcement
> Before each adapter-calling stage, the orchestrator computes expected USD spend and compares against the remaining budget. This reference documents the estimate flow, cap comparisons, actual-cost append, and the rate-table maintenance protocol.

## 1. Pre-flight estimate flow

Before dispatching an adapter for any stage that incurs external API cost:

```
1. Director synthesizes the request list
      ↓
2. For each request, call lib/cost-tracker.sh:
     estimate_image  {model} {resolution}              → credits
     estimate_video  {model} {duration_s} {resolution} → credits
      ↓
3. Convert credits → USD: credits × currency_per_credit_usd
   (rate from lib/higgsfield-rates.yaml)
      ↓
4. Write stages[N].cost_estimate_usd to sukoon.yaml
      ↓
5. Cap comparison (see §2)
      ↓
6. If clear → dispatch adapter
```

`lib/cost-tracker.sh` functions:
- `estimate_image {model} {resolution}` — looks up `image_models.{model}.credits_per_image.{resolution}` from `lib/higgsfield-rates.yaml`.
- `estimate_video {model} {duration_s} {resolution}` — looks up `video_models.{model}.credits_per_clip.{duration_s}s-{resolution}`.
- `add_actual_usd {run_id} {usd}` — adds to `cost_tracker.api_usd` in the manifest.
- `stale_check` — reads `snapshot_date` from rate table; warns if older than 90 days.

## 2. Cap comparison logic

```
projected_usd = cost_tracker.api_usd + stages[N].cost_estimate_usd
```

| Condition | Action |
|---|---|
| `projected_usd > budget_usd_cap` | Hard halt → escalate (reason: `cost-cap-exceeded`). Offer `budget +N` / `replan-from-{stage}` / `abort`. |
| `projected_usd > budget_usd_cap × 0.80` | Warning gate (non-halting). Surface callout in chat: "80% of budget consumed. Proceeding unless you reply `replan`." Pipeline continues automatically after 60s if no reply. |
| `projected_usd ≤ budget_usd_cap × 0.80` | Silent — no gate, no message. |

> [!warning] $20 default cap
> `budget_usd_cap` defaults to `20` in `config.yaml`. Yasir can override per-run via `brief.budget_usd_cap` in `sukoon.yaml`. The cap covers external API spend only — Claude token costs are tracked separately via `cost_tracker.tokens_used` and do not count toward the cap.

## 3. Actual-cost append

After an adapter returns, regardless of success or partial failure:

1. Sum all `results[].usd_cost` from the adapter response.
2. Call `lib/cost-tracker.sh add_actual_usd {run_id} {sum}` → adds to `cost_tracker.api_usd` in manifest.
3. Log estimate-vs-actual variance to `{run_id}/audit.jsonl`:

```json
{"ts": "ISO-8601", "event": "cost_actual", "stage": "visual", "estimate_usd": 1.20, "actual_usd": 1.43, "variance_pct": 19.2}
```

Variance is logged per-stage for calibration. The spec targets ±15% accuracy (success criterion #4). If variance consistently exceeds 15%, the rate table in `lib/higgsfield-rates.yaml` should be updated.

## 4. Rate table maintenance

**Source of truth:** `Projects/ihsan/sukoon/lib/higgsfield-rates.yaml`

Key fields:
- `snapshot_date` — date the rate table was last verified against Higgsfield pricing.
- `currency_per_credit_usd` — current conversion rate (Creator plan: approximately $0.033/credit based on $149/mo ÷ ~4500 credits).
- `image_models.{model}.credits_per_image.{resolution}` — per-image credit cost.
- `video_models.{model}.credits_per_clip.{duration}s-{resolution}` — per-clip credit cost.

**Staleness check:** `lib/cost-tracker.sh stale_check` warns if `snapshot_date` is older than 90 days. The warning appears in the `events[]` log and in chat at run start. It does not block the run.

**Update protocol:** When Higgsfield changes pricing:
1. Update `snapshot_date` and affected rate fields in `higgsfield-rates.yaml`.
2. Re-run the golden stub fixture (tier-1 test) — cost estimates should be deterministic; if they change, update the golden fixture.
3. Append a note to `plans/_build-log.md`.

## 5. Per-service pricing (adapters own their own USD)

`lib/cost-tracker.sh` and `lib/higgsfield-rates.yaml` cover Higgsfield exclusively. The other adapters compute their own USD internally:

| Adapter | Pricing model | USD computation |
|---|---|---|
| `elevenlabs.sh` | Per character | `$0.000167 × char_count` (Starter plan) |
| `suno.sh` | Per credit | `$0.001 × credits_used` (Pro plan) |
| `ffmpeg.sh` | Free (local) | `0.00` always |

ElevenLabs and Suno adapters emit `usd_cost` in their output; the orchestrator calls `add_actual_usd` with the adapter's returned value, not a rate-table lookup.

## 6. Budget-remaining field

After each adapter call, the orchestrator updates:

```yaml
cost_tracker:
  api_usd: <cumulative>
  budget_usd_remaining: <brief.budget_usd_cap - api_usd>
```

`budget_remaining_usd` is also passed as a field in the adapter input YAML so each adapter can skip expensive optional enhancements (e.g., higher resolution) if budget is tight. Adapters should respect this field as a soft hint, not a hard limit.

## 7. Known debt

- `currency_per_credit_usd: 0.033` is an approximation; the exact rate depends on whether Higgsfield grants exactly 4500 credits for $149. Build log 2.2 notes the first live-image integration test spent $0.17 for 14 credits (~$0.012/credit). Rate table calibration against real runs is required before trusting estimates to ±15%.
- ElevenLabs character pricing (`$0.000167`) is based on the Starter plan as of 2026-04-20. An ElevenLabs rate table analog should be added if the plan or pricing changes.
- The 80% warning gate has a 60-second auto-continue window. This is not yet implemented as a timed prompt — current behavior surfaces the warning callout and relies on Yasir to reply manually. Auto-continue timer is a Stage 2.5 improvement.
