---
type: skill-reference
skill: NBPHero
topic: product-types
---

# Saved Product Type Descriptions

User-editable library of reusable one-line product-type descriptions (material + texture + shape + branding accent).

## Schema

```
### [Product name] — [brief type]
*"[one-line description: material, shape, key texture, branding accent]"*
Reference image: [[path/to/product-reference.png]]
```

## Library

_Empty. Populate as Yasir ships products._

### Example template (replace with real entries)

```
### [Example] — Quilted zipper pouch
*"quilted zipper pouch — match the puffy pillow-quilted texture, grid stitching,
metal zipper, and small woven label tag shown in the reference exactly"*
Reference image: [[path/to/example-pouch.png]]
```

```
### [Example] — Silk sleep eye mask
*"Mustard-gold satin, [wordmark] in white on right lens area."*
Reference image: [[path/to/example-mask.png]]
```

## Usage

When NBPHero needs to render a product with a specific material/shape, it pulls the description from here. New product types are saved here after first successful render so subsequent jobs are reproducible.
