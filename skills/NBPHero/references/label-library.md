---
type: skill-reference
skill: NBPHero
topic: label-library
---

# Saved Label Transcriptions

User-editable library of product label transcriptions. Each entry is a verbatim breakdown of a product's label copy so NBPHero can reproduce it faithfully.

## Schema for each entry

```
### [Product name] — [product type]

"[hero wordmark]" in [color/position], then "[secondary line]" [hierarchy note],
then "[tertiary line]" [hierarchy note], ...

Below that, [graphic element description]. To the right of that, [additional element]
— render the [element A] and the [element B] as two separate elements with visible
clear space between them.

Reference image: [[path/to/product-reference.png]]
```

## Library

_Empty. Populate as Yasir ships products with hero-rendering needs._

### Example template (replace with real entries)

```
### [Example] — 30-capsule supplement bottle

"BRAND" vertically along the left side, then "[hero wordmark]" as the primary
in white, then "[short tagline]" beneath in small caps, then "with [key
ingredient]" in italic script.

Below that, a round orange graphic badge contains the large numeral "30" in
bold white, positioned as a standalone circular icon. To the right of that
badge, on the same horizontal line, the words "CAPSULES" appear in bold white
sans-serif capitals. On the line directly below, "DIETARY SUPPLEMENT" appears
in smaller bold white capitals, centered under "CAPSULES."

Reference image: [[path/to/example-bottle.png]]
```

## Usage

When NBPHero is asked to render a product label, it checks this library first. If found, use the transcription verbatim in the prompt. If not found, ask the user to provide one and save it here for reuse.
