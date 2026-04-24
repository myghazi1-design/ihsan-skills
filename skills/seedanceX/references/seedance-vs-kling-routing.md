---
type: skill-reference
skill: seedanceX
topic: routing
---

# Seedance vs Kling Routing

## The Core Rule

Seedance *reinterprets* camera instructions through genre conventions from its ByteDance/Douyin training data. Kling follows instructions literally.

- Write Seedance prompts as **scene descriptions**
- Write Kling prompts as **shot instructions**

## Routing Table

| Use Case | Seedance 2.0 | Kling 3.0 |
|----------|-------------|-----------|
| Action / explosions / debris | **Best choice** | Decent |
| Dynamic camera movement | **Best choice** | Good |
| Animals / creatures / fantasy | **Best choice** | Decent |
| Environmental storytelling | **Best choice** | Good |
| UGC with timestamp method | **Best choice** | Good |
| Wide shots (no face detail) | **Best choice** | Good |
| Talking head / dialogue | Decent with timestamps | **Best choice** |
| Static / locked camera | Avoid (micro-tilt bug) | **Best choice** |
| Literal camera instructions | Avoid (reinterprets) | **Best choice** |
| 3+ characters | Avoid (face warp) | Better but still risky |
| Small object detail | Avoid | Better |
| Lip sync precision | Decent | **Best choice** |

## Deny-List (route these AWAY from Seedance)

If the user's request contains ANY of these, stop and suggest `kling-video`:

1. **Talking head with tight face framing + dialogue** — lip-sync precision matters
2. **Static / locked tripod shot** — Seedance has a micro-tilt bug
3. **Literal camera instructions** — "Camera performs 360° orbit at 2m radius" — Kling follows, Seedance reinterprets
4. **3+ characters in frame** — face warp guaranteed
5. **Small object precision detail** — Seedance smudges small text / fine object detail
6. **Dialogue-centric scene with no timestamps** — route to Kling's dialogue mode

## Hybrid Chain Strategy

For 30-60s content with dialogue sections:
- **Dialogue-heavy sections** → Kling (lip-sync precision)
- **Cinematic / B-roll / environmental sections** → Seedance
- **Chain them** via Master-Reference-Reuse (see `chaining-protocol.md`)

## Tool-selection questions to ask upfront

1. Is there on-camera dialogue? → If yes, evaluate lip-sync stakes → Kling likely
2. Is camera static or locked? → If yes, Kling
3. Are there 3+ people on-screen? → If yes, Kling (still risky but better)
4. Is this a timestamp-driven UGC scene? → Seedance's native convention
5. Is this action / environment / wide shot / creature? → Seedance
