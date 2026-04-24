---
type: skill-reference
skill: shotlist
source: notes.md + notes (4).md (video production:ai source corpus)
tags: [shotlist, routing, seedance, kling, nano-banana]
---

# Tool Routing Table

The central routing rule for any shot. Comes from [[2026-04-20-seedance-vs-kling-routing]].

## Full routing matrix

| Use case | Seedance 2.0 | Kling 3.0 |
|---|---|---|
| Action / explosions / debris | **Best** | Decent |
| Dynamic camera movement | **Best** | Good |
| Animals / creatures / fantasy | **Best** | Decent |
| Environmental storytelling | **Best** | Good |
| UGC with timestamp method | **Best** | Good |
| Wide shots (no face detail) | **Best** | Good |
| Talking head / dialogue | Decent with timestamps | **Best** |
| Static / locked camera | Avoid (micro-tilt bug) | **Best** |
| Literal camera instructions | Avoid (reinterprets) | **Best** |
| 3+ characters | Avoid (face warp) | Better but still risky |
| Small object detail | Avoid | Better |
| Lip sync precision | Decent | **Best** |

## Routing rule (one-liner)

> Write Seedance prompts as **scene descriptions**. Write Kling prompts as **shot instructions**.

Seedance reinterprets camera instructions through ByteDance / Douyin genre conventions. Kling follows instructions literally.

## Still-image routing

| Use case | Tool |
|---|---|
| Character start frame (4K) | Nano Banana Pro |
| Product hero / label | Nano Banana Pro (via `/nbp-hero` subclass) |
| Reset frame for a chain | Nano Banana Pro |
| Background swap, character preservation | Nano Banana Pro JSON method |

## UGC-specific routing

- UGC creator on camera, talking head → Kling 3.0 (dialogue mode)
- UGC product demo, B-roll → Seedance 2.0 (cinematic mode)
- UGC before/after with narration → Seedance 2.0 timestamp mode
- Product close-up hero still → NBP (may not need video)

## When to NOT render video at all

Sometimes the best answer is "this is a still, not a video":
- Product close-up with no motion intent
- Logo reveal (static, add motion in CapCut)
- Simple text overlay (CapCut, not gen)

Decide this BEFORE generating.
