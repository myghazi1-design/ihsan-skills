---
type: skill-reference
skill: review-video
source: notes (1).md (verdict framework section)
tags: [review, verdict, decision]
---

# Verdict Framework

Three possible verdicts. Pick one. Cite specific axes and frames.

## Ship it

- Passes the scroll test. No viewer would stop scrolling for a wrong reason.
- Minor issues only — things only visible in side-by-side scrutiny, not at normal speed.
- Next action: export → Topaz 2× / 60 fps → CapCut 30 fps → captions → Suno music layer.
- ==Don't over-polish== — shipping a 90% clip is better than re-rolling for the last 10%.

## One more pass

- Fixable issues. The prompt just needs a specific edit.
- ==Specific change required== — "add `locked tripod` so camera stops drifting", "change `dramatic lighting` to `pendant lamps cast warm amber pools`", "remove character 3".
- Per [[2026-04-20-2-reroll-limit]], if the same edit has already been made twice without success, escalate to Re-prompt.
- Next action: apply the specific change, re-render, re-review.

## Re-prompt

- Fundamental issues. The approach is wrong.
- Common triggers:
  - Model mismatch (Seedance got a literal-camera-instruction shot — route to Kling)
  - Too many characters for the model
  - Dialogue at clip seam (structural)
  - Chain drift past what Master-Reference-Reuse can fix
- Next action: rethink → [[shotlist]] re-routes the shot.

## Decision logic

```
If all 6 axes clean → Ship it

Else if 1 – 2 axes have fixable issues AND the fix is a specific prompt edit → One more pass

Else (3+ axes fail, OR structural issue, OR already re-rolled twice on same edit)
  → Re-prompt
```

## Output format

```
**Verdict:** [Ship it / One more pass / Re-prompt]

**Per-axis findings:**
- Axis 1 (Realism): [clean | issue — specific frame]
- Axis 2 (Hands & Body): ...
- Axis 3 (Face & Expression): ...
- Axis 4 (Background & Env): ...
- Axis 5 (Composition & Framing): ...
- Axis 6 (AI Artifacts): ...

**Tool-specific checks:**
- [Seedance / Kling / Mikos] known failure: [matched | not matched]

**If One more pass — specific prompt change:**
- Before: "..."
- After: "..."

**If Re-prompt — reroute:**
- Current: [model]
- Suggested: [model + reasoning]
```
