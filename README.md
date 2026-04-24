# Ihsan — AI Video Production Skills for Claude Code

A 14-skill plugin covering the full AI video stack: image reference generation, i2v animation, dialogue, voice, UGC structure, hook selection, shot planning, review, and the **Sukoon** director-agent pipeline that chains them into an approval-gated production line.

## Install

```
/plugin marketplace add myghazi1-design/ihsan-skills
/plugin install ihsan@myghazi1-design-ihsan-skills
```

Skills activate as `/ihsan:<skill-name>` (e.g. `/ihsan:sukoon`, `/ihsan:seedanceX`).

To update after a new version is pushed:
```
/plugin marketplace update
```

## What's inside

### Tool-specific skills (Overseer A)

| Skill | Purpose | Status |
|---|---|---|
| `seedanceX` | Seedance 2.0 prompt assembly (cinematic, UGC, fights, orbs, transformations, animation) | COMPLETE |
| `kling-video` | Kling 3.0 prompt assembly — talking-head, static shots, lip-sync, multi-character | COMPLETE |
| `nbp` | Nano Banana Pro 4K reference image generation for downstream i2v | COMPLETE |
| `NBPHero` | Hero / label / product-kit image specialist | COMPLETE |
| `mikos` | MIKOSLAB V2 8-section UGC rendering scaffold | STUB — 16 archetypes + Anti-Glitch need source |
| `voice-elevenlabs` | ElevenLabs voice generation, cloning, chunking | STUB — Voice Design internals missing |

### Process / orchestration skills (Overseer B)

| Skill | Purpose |
|---|---|
| `shotlist` | Concept → production plan with tool routing |
| `cinematography-prompting` | Camera / light / physics substitution layer for any video prompt |
| `video-dialogue-prompting` | Kling 3.0 native-audio dialogue syntax (P1 – P4 principles) |
| `review-video` | Post-render QA, 6-axis rubric, 3-verdict framework |
| `ugc-creator-setup` | Rachel Martinez 11-phase UGC playbook + AI-UGC branch |
| `hook-picker` | Retrieval over a 250-hook library |

### Sukoon pipeline

| Skill | Purpose |
|---|---|
| `sukoon` | End-to-end director-agent pipeline: Brief → Shot → Visual → Motion → Audio → Review → Editor |
| `sukoon-edit` | Editor stage — assembles approved assets into a CapCut edit-list or ffmpeg final cut |

## Repo layout

```
.claude-plugin/
  plugin.json              Plugin manifest
skills/
  {skill-name}/
    SKILL.md               Executable skill (frontmatter + body)
    references/            Reference material (strategy, voice, templates)
    config.yaml            Optional per-skill config (sukoon only)
```

## Caveats

- **Vault-coupling**: some `SKILL.md` files contain `[[Skills/<name>/references/...]]` wikilinks that assume an Obsidian vault with a mirrored `Skills/` folder. The plugin bundles the same references at `skills/<name>/references/` so the content is available; a future version will rewrite the paths to relative (`./references/...`) for full portability outside Obsidian.
- **`mikos` and `voice-elevenlabs` are STUB-flagged** — functional but light on source material. Usable, not final.
- **Private install**: if you fork this to a private repo, installers must set `GITHUB_PERSONAL_ACCESS_TOKEN` in Claude Code settings.

## Author

Built by Yasir Ghazi as part of the Ihsan AI-video project.
