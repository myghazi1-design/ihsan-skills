---
type: skill-reference
skill: sukoon
topic: gating rules
date: 2026-04-20
---

> [!info] Adaptive gating (Stage 2)
> Stage 2 defaults to 2 human gates per run: shotlist and final cut. All other stages auto-advance when conditions are clean. This reference documents which triggers insert intermediate gates, the decision flow, and the Stage 3 trust-score hint.

## 1. Default gates (never skipped)

| Gate | Stage | Why human eyes required |
|---|---|---|
| **Shotlist gate** | After Shot Director | Creative structure — shot count, durations, purposes. Downstream stages (Visual, Motion, Audio, Editor) all derive from this. A bad shotlist compounds across every stage. |
| **Editor / Final cut gate** | After Editor Director | Last human checkpoint before a file is declared shippable and uploaded to Drive + posted to Discord. Always gates, even if Review Director passed every asset. |

These two gates fire unconditionally. `auto_advance: true` in `sukoon.yaml` does NOT skip them (that field is reserved for other stages only).

## 2. Adaptive escalation triggers

All other stages auto-advance silently when conditions are clean. The following triggers insert an intermediate gate:

| Trigger | Condition | Gate verbs offered |
|---|---|---|
| Review non-ship | `stages[review].output.verdicts[*].verdict ∈ [one-more-pass, re-prompt]` for any asset | `approve` (ship anyway) / `revise: {notes}` / `fork` / `abort` |
| Cost breach | `projected_api_usd > budget_usd_cap` | `budget +N` / `replan-from-{stage}` / `abort` |
| Cost warning (80%) | `projected_api_usd > budget_usd_cap × 0.80` | `continue` / `replan` — non-halting; auto-continues after 60s if no reply |
| Policy rejection | Any adapter returned `status: failed` with `error: content-policy-rejected` | `revise: {notes}` / `abort` — no auto-retry |
| Manual pause | Yasir types `/sukoon pause` | Standard verbs: `approve` / `revise: {notes}` / `fork` / `abort` |
| Revision budget exhausted | `revisions_used == revision_budget_per_stage` AND Yasir replies `revise` | `budget +N` / `replan` / `abort` |

## 3. Decision flow

After each stage completes, the orchestrator runs this check in order:

```
Stage just finished
       │
       ▼
Is this the shotlist stage?
  YES → ALWAYS GATE (shotlist gate)
  NO  ↓
Is this the editor stage?
  YES → ALWAYS GATE (final cut gate)
  NO  ↓
Is this the review stage?
  YES → Did any verdict = one-more-pass or re-prompt?
          YES → ESCALATION GATE (review non-ship)
          NO  → auto-advance silently
  NO  ↓
Pre-flight cost check for next stage:
  projected > cap      → ESCALATION GATE (cost breach)
  projected > 80% cap  → WARNING GATE (non-halting)
  projected ≤ 80% cap  → continue
       │
       ▼
Did any adapter in this stage return content-policy-rejected?
  YES → ESCALATION GATE (policy rejection)
  NO  ↓
Auto-advance to next stage silently
```

At any point, Yasir may interrupt with `/sukoon pause` to insert a gate before the next stage fires.

## 4. Gate preview rendering

All intermediate gates render `stages/{stage}/preview.html` using the standard preview template (`Skills/sukoon/references/preview-template.html`) before presenting verbs. The review card format from [[Skills/sukoon/references/approval-ux]] applies to all gates — intermediate and default alike.

Gate card header format for intermediate gates:

```markdown
## ⚠️ Intermediate gate: {trigger reason}

**Stage:** {stage_name}
**Trigger:** {trigger description}
**Detail:** {detail}

{standard verbs block}
```

## 5. Stage 3 hint: per-director trust scores (out of scope for Stage 2)

> [!tip] Stage 3 roadmap item
> After Stage 2 accumulates clean runs, a per-director trust score could relax the "any non-ship verdict → gate" rule. For example: if Review Director has passed the last 5 visual stages without a human override, `one-more-pass` verdicts on a low-stakes shot could auto-resolve with a reroll rather than halting for Yasir. This is explicitly a Stage 3 capability — not implemented, not tracked. Mentioning it here so the gating-rules reference serves as the right place to document it when the time comes.

## 6. Auto-advance conditions (explicit)

A stage auto-advances (no gate, no message) when ALL of the following are true:

1. The stage is not shotlist and not editor.
2. The stage is not review, OR review verdicts are all `ship`.
3. `projected_api_usd ≤ budget_usd_cap × 0.80` for the next stage.
4. No adapter in this stage returned `content-policy-rejected`.
5. Yasir has not typed `/sukoon pause`.
6. `revisions_used < revision_budget_per_stage` (or no revise reply was made).

If any condition is false, the appropriate gate from §2 fires.

## 7. Known debt

- The 60-second auto-continue window for cost-warning (80%) gates is not yet implemented as a timed mechanism. Current behavior: warning callout posted in chat; pipeline awaits manual `continue` or `replan` reply. Timed auto-continue is a Stage 2.5 improvement.
- `auto_advance: true` per-stage field (from [[Skills/sukoon/references/approval-ux#Quiet mode]]) is not yet enforced in the decision flow — orchestrator reads it but the enforcement logic is not wired to the gating check. Stage 3 will formalize this.
