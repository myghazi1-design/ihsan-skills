---
name: sukoon-edit
description: Use when the Editor Director stage of a Sukoon run needs to assemble approved assets into an edit-list (Stage 1, CapCut-importable JSON) or a rendered final cut (Stage 2+, ffmpeg). Triggers on /sukoon-edit, "assemble the Sukoon run", "build the edit-list", "finalize the pipeline", or when the Sukoon orchestrator dispatches the Editor Director. Do NOT trigger for general video editing outside a Sukoon run — use the user's native editor or ffmpeg directly for that.
---

# Sukoon-edit — assemble approved Sukoon artifacts into a final cut

This skill is invoked by the Editor Director ([[Skills/sukoon/directors/7-editor-director]]) as the last stage of a Sukoon run. It reads approved motion, voice, music, and SFX artifacts from the run's `stages/` subdirs and emits:

- **Stage 1**: `edit-list.json` (CapCut-importable) + `ffmpeg-assemble.sh` (fallback) + `NEXT-STEPS.md` (manual finish instructions)
- **Stage 2+**: renders `final.mp4` directly via ffmpeg, uploads to Google Drive via MCP

## Inputs

A valid Sukoon run directory:
```
Projects/ihsan/sukoon/runs/{run_id}/
├── sukoon.yaml                 # manifest
├── stages/
│   ├── motion/clips/           # approved .mp4 per shot
│   ├── audio/voice/            # voice .mp3 per shot (if voice-elevenlabs routed)
│   ├── audio/music/            # music bed (if external routed)
│   └── audio/sfx/              # SFX files or cue sheet
```

## Preflight — assets check

Before emitting anything:

1. Read `sukoon.yaml`
2. For every shot in `stages.shotlist.output.shots`:
   - Verify a matching clip exists at `stages/motion/clips/shot-{NN}.mp4`
   - Verify the clip has a `ship` verdict in `stages.review.output.verdicts`
3. For every dialogue-bearing shot with `tool_routing.voice == voice-elevenlabs`:
   - Verify matching voice file at `stages/audio/voice/shot-{NN}.mp3`
4. If music routed `external`, verify `stages/audio/music/bed.{mp3,wav}` exists OR note in output that music is pending manual drop-in
5. If any asset missing or unapproved → FAIL with `{"error": "missing-asset", "detail": ...}` and a list of specific gaps

## Stage 1 — emit edit-list.json

Build CapCut-compatible JSON:

```json
{
  "project_name": "{run_id}",
  "duration_s": {brief.duration_s},
  "aspect": "{brief.aspect}",
  "fps": 30,
  "tracks": [
    {
      "type": "video",
      "clips": [
        {
          "source": "../motion/clips/shot-01.mp4",
          "in": 0,
          "out": {shot-01.duration_s},
          "timeline_start": 0
        }
        // ...one clip per shot, timeline_start cumulative
      ]
    },
    {
      "type": "audio",
      "role": "voice",
      "clips": [
        {
          "source": "../audio/voice/shot-01.mp3",
          "timeline_start": 0,
          "gain_db": 0
        }
        // ...one per dialogue-bearing shot (skip if kling-native)
      ]
    },
    {
      "type": "audio",
      "role": "music",
      "clips": [
        {
          "source": "../audio/music/bed.mp3",
          "timeline_start": 0,
          "gain_db": -12,
          "fade_in": 0.5,
          "fade_out": 1.0
        }
      ]
    },
    {
      "type": "audio",
      "role": "sfx",
      "clips": [
        // ...per SFX cue
      ]
    }
  ]
}
```

### Rules

- **Video track**: cumulative timeline_start — `sum(prior shot durations)`. No gaps, no overlaps by default.
- **Voice track**: `timeline_start` matches the shot's `timeline_start` for that shot's video clip. Apply any `lead_in_silence_ms` from Audio Director's handoff.
- **Music track**: `timeline_start: 0`, `gain_db: -12` default (mixed under voice), fade rules: 0.5s in / 1.0s out unless Audio Director specified otherwise.
- **SFX track**: per cue's `start_s`.
- **Aspect / duration / fps**: from `brief`.

Write to `stages/editor/edit-list.json`.

## Stage 1 — emit ffmpeg fallback

Produce `stages/editor/ffmpeg-assemble.sh` — a deterministic shell script that could build the same cut via ffmpeg. Useful as a reference for Stage 2 adapter implementation, or for users who prefer CLI.

Template:

```bash
#!/usr/bin/env bash
set -euo pipefail

# Sukoon ffmpeg assembly — run_id: {run_id}
# Generated: {ISO timestamp}

cd "$(dirname "$0")"

# 1. Concat video
ffmpeg -y \
  -f concat -safe 0 -i <(for f in ../motion/clips/shot-*.mp4; do echo "file '$f'"; done) \
  -c copy video-joined.mp4

# 2. Build voice mix (if any)
ffmpeg -y -i ../audio/voice/shot-01.mp3 -af "adelay=0|0" voice-01.mp3
# ...repeat per shot with cumulative delay
ffmpeg -y -i voice-01.mp3 -i voice-02.mp3 ... -filter_complex "[0:a][1:a]amix=inputs=N:duration=longest" voice-mix.mp3

# 3. Mix music under voice (if any)
ffmpeg -y -i voice-mix.mp3 -i ../audio/music/bed.mp3 \
  -filter_complex "[1:a]volume=-12dB,afade=t=in:st=0:d=0.5,afade=t=out:st={duration-1}:d=1.0[music];[0:a][music]amix=inputs=2:duration=longest" \
  audio-mix.mp3

# 4. Mux video + audio
ffmpeg -y -i video-joined.mp4 -i audio-mix.mp3 -c:v copy -c:a aac -shortest final.mp4

echo "Final cut: $(pwd)/final.mp4"
```

## Stage 1 — emit NEXT-STEPS.md

```markdown
# Next steps — manual finish for {run_id}

You have two paths. Pick one:

## Path A: CapCut (recommended for UGC / TikTok)

1. Open CapCut (desktop).
2. File → Import Project → select `edit-list.json` from this directory.
3. Verify tracks align. Video clips should snap back-to-back; voice should sit in sync with video; music should be -12dB under voice.
4. Optional: apply a color-grade preset (suggested "Apple ProRes" style for warm tones, "Bleached" for cooler).
5. Export: H.264, 30fps, {brief.aspect}, target 15-20 Mbps, AAC 192kbps audio.
6. Save final as `stages/editor/final.mp4`.
7. Reply `approve` in Sukoon to finalize the run.

## Path B: ffmpeg (CLI, for automation-friendly runs)

1. From this directory: `bash ffmpeg-assemble.sh`
2. Verify `final.mp4` looks correct (use `ffplay final.mp4` or `open final.mp4`).
3. Reply `approve` in Sukoon to finalize the run.

## If something's off

- **Asset missing**: rewind to the relevant director via `fork` from that stage, or re-run manually.
- **Sync wrong**: edit `edit-list.json` (CapCut) or `ffmpeg-assemble.sh` (CLI), then re-import or re-run.
- **Quality issue**: rewind to Review Director with `revise: {shot_id: reason}`.
```

## Stage 2 — auto-execution

> [!info] When this path activates
> Trigger condition: `brief.stage_mode == live` AND all prior pipeline stages have `status: approved` in `sukoon.yaml`. When this is true, the Editor Director runs auto-execution instead of stopping at the edit-list + NEXT-STEPS.md. Stage 1 outputs (edit-list.json, ffmpeg-assemble.sh, NEXT-STEPS.md) are still emitted first — Stage 2 builds on top of them.

### 1. Preflight

Before invoking the adapter, the orchestrator verifies every asset path referenced in `edit-list.json` exists on disk. Missing files are listed with their full absolute paths in a pre-flight error message per spec §6.3. The adapter itself repeats this check — but the orchestrator surfaces it earlier so the error appears in the conversation rather than buried in a log file.

Preflight covers:
- All `tracks[].clips[].source` paths for `type=video`, `type=audio` (role=voice and role=music), and `type=image`
- All `image_overlays[].source` paths
- The `brand_lockup` path from the adapter-input params

If any file is missing → **halt with a checklist**; never proceed to render a partial or broken cut.

### 2. Adapter dispatch

The orchestrator synthesizes `stages/editor/assemble-request.yaml` from the run manifest's `brief` + `edit-list.json`, then invokes:

```bash
Projects/ihsan/sukoon/adapters/ffmpeg.sh \
  --mode=live \
  --input=stages/editor/assemble-request.yaml
```

The `assemble-request.yaml` shape follows the [[Projects/ihsan/sukoon/specs/2026-04-20-stage2-automation-design|Stage 2 spec]] §4.3.4 adapter contract:

```yaml
mode: live
run_id: {run_id}
stage: editor
budget_remaining_usd: {cost_tracker.remaining}
requests:
  - id: assemble-1
    params:
      edit_list_file: stages/editor/edit-list.json
      output_file: stages/editor/final.mp4
      brand_lockup: stages/editor/brand-lockup.png
      duration_s: {brief.duration_s}
      output_encoding:
        codec: h264
        profile: high
        fps: 30
        crf: 18
        pix_fmt: yuv420p
        audio_codec: aac
        audio_bitrate: 192k
        faststart: true
```

The adapter handles:
- **Video**: concat filter (all clips sorted by `timeline_start`)
- **Voice track** (`role=voice`): mixed at 0 dB
- **Music track** (`role=music`): mixed at `gain_db` (default −12 dB) with 0.5s fade-in and 1.0s fade-out
- **Brand-lockup overlay**: PNG overlaid at last 1.5s of timeline, bottom-center, 32px margin (`x=(W-w)/2:y=H-h-32`)
- **Audio-only-missing variants**: voice-only, music-only, or no-audio (silent mute via `anullsrc`) all produce valid output

> [!tip] Known debt (polish pass)
> SFX cue mixing is deferred — only voice + music in v1. VO sidechain ducking (dynamic attenuation) is deferred; fixed −12 dB music is the MVP. Freeze-frame fallback for drift-flagged clips is deferred.

### 3. Verify render result

After the adapter returns, check `stages[6].output.adapter_results[0]`:

- **`status: completed`** → `asset_path` is the absolute path to `final.mp4`. Proceed to Drive upload.
- **`status: failed`** → escalate per spec §7.3. Do NOT proceed to upload. Write `POST-MORTEM.md` at run root if `error` is a hard failure class (ffmpeg-exit-nonzero, output-too-small, duration-mismatch).

### 4. Drive upload

Call `mcp__claude_ai_Google_Drive__create_file` with:
- Source file: the adapter's `asset_path` (absolute path to `final.mp4`)
- Destination: `Sukoon-runs/{run_id}/final.mp4`

Store the returned Drive URL in `sukoon.yaml`:

```yaml
final_delivery:
  local_path: stages/editor/final.mp4
  drive_url: https://drive.google.com/...
```

### 5. Discord mirror

Condition: `brief.notify_discord: true` (default `true`). Skip if `brief.notify_discord: false`.

Steps:
1. Extract first frame as thumbnail:
   ```bash
   ffmpeg -i stages/editor/final.mp4 \
     -vf "select=eq(n\,0)" -frames:v 1 \
     stages/editor/thumb.png
   ```
2. Call `mcp__plugin_discord_discord__reply` with:
   - `files: ["stages/editor/thumb.png"]`
   - Caption text containing the Drive URL and run duration (measured from `ffprobe`)
3. Set `discord_posted: true` in the final-delivery block.

### 6. Final-delivery block in `sukoon.yaml`

Written once all steps complete:

```yaml
final_delivery:
  local_path: stages/editor/final.mp4
  drive_url: https://drive.google.com/file/d/...
  discord_posted: true
  duration_s: 30.02          # measured via ffprobe
  size_bytes: 18432710
  completed_at: 2026-04-20T19:45:12-05:00
```

### 7. Failure modes that halt the run

Each condition writes `POST-MORTEM.md` at run root and escalates per spec §7.3:

| Condition | Error | Action |
|---|---|---|
| ffmpeg exits non-zero | `ffmpeg-exit-nonzero` | Write post-mortem, halt, escalate |
| `final.mp4` duration deviates from `brief.duration_s` by >0.5s | `duration-mismatch` | Alignment issue upstream; escalate |
| `final.mp4` < 500 KB | `output-too-small` | Render corrupted; escalate |
| Drive upload fails | MCP error | Log error, halt, escalate — do NOT post to Discord without a Drive URL |

### Known debt for polish pass

- SFX cue multi-track mixing (only voice + music mixed in v1; multi-cue SFX deferred)
- VO sidechain ducking (currently fixed −12 dB music; `sidechaincompress` dynamic attenuation deferred)
- Freeze-frame fallback for drift-flagged clips (Motion adapter `drift_detected: true` flag not yet wired into Editor)
- Motion-director drift detection integration (`last_clean_frame_s` metadata → freeze-extend in Editor)

Stage 2 deliverables are tracked in [[Projects/ihsan/sukoon/README#9. Stage 2 — Service-integrated]].

## Output

Written to `stages.editor.output` in `sukoon.yaml`:

```yaml
output:
  edit_list:
    format: capcut              # Stage 1
    file: stages/editor/edit-list.json
  ffmpeg_commands:
    - file: stages/editor/ffmpeg-assemble.sh
      purpose: fallback
  next_steps_file: stages/editor/NEXT-STEPS.md
  # Stage 2+:
  # final_cut: stages/editor/final.mp4
  # drive_link: https://...
  artifacts:
    - path: stages/editor/edit-list.json
      type: edit-list
    - path: stages/editor/ffmpeg-assemble.sh
      type: ffmpeg-script
    - path: stages/editor/NEXT-STEPS.md
      type: instructions
```

## Never do these

- Don't run outside a Sukoon run context — this skill assumes the full pipeline structure
- Don't assemble if any motion clip is missing or unapproved (fail, don't silently skip shots)
- Don't overwrite `final.mp4` if one already exists — emit as `final-v2.mp4` etc. (preserve user edits)
- Don't upload to external services in Stage 1 (Drive/Discord are Stage 2+ only)

## See also

- [[Skills/sukoon/directors/7-editor-director]] — the director that dispatches this skill
- [[Skills/sukoon/references/manifest-schema]] — `sukoon.yaml` stage structure
- [[.claude/skills/sukoon/SKILL|/sukoon orchestrator]] — parent pipeline
