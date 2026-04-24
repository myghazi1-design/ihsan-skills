---
type: skill-reference
skill: seedanceX
topic: canva-bypass
---

# Canva Character Limit Bypass

Seedance has a 2,000-character prompt limit. For prompts that exceed this, use this emergent-OCR workaround.

## The Method

1. Create an image in Canva with your full detailed prompt rendered as text
2. Include ALL movements, timestamps, dialogue, VFX — everything that doesn't fit in 2000 chars
3. Upload that image to Higgsfield as a reference asset
4. In your actual (short) prompt, write: `Transcribe this image and use the movements described here.`

Seedance reads the text in the reference image via its multimodal OCR and follows those instructions.

## When to Use

- Mode 2 UGC with long dialogue (20+ seconds of scripted lines)
- Multi-shot cinematic with detailed per-shot beats
- Character sheets that specify appearance in detail for chain consistency
- Chain protocol prompts that need identical Global Context + per-clip dialog

## Caveats

- **This is fragile** — depends on emergent OCR, not a documented feature
- Can break silently on platform updates
- Test the render; if Seedance ignores the reference text, fall back to splitting the prompt into 2 clips

## Formatting tips for the Canva image

- Use a clear readable font (Helvetica or similar, not decorative)
- Black text on white background
- Size 14-18pt — too small = OCR fails, too large = image gets clipped
- Layout: sections separated by whitespace, headings bold
- Save as PNG or JPG at 1920×1080 or larger

## Fallback

If Canva bypass fails on a given prompt:
1. Split the prompt into 2 shorter prompts (each <2000 chars)
2. Chain the two outputs via Master-Reference-Reuse
3. Use identical Global Context Block across both
