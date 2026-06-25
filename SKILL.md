---
name: high-fidelity-html-reconstruction
description: High-fidelity screenshot/design-to-HTML reconstruction using a strict GPT img2img green-source asset workflow, Python chroma-key extraction, manifest generation, and browser verification. Use when the user asks to convert an uploaded screenshot, design mockup, UI image, landing page, app screen, or web page screenshot into HTML with high visual fidelity, especially when they mention green_source.png, img2img, chroma key, asset extraction, screenshot to HTML, design to HTML, high fidelity, or strict asset provenance.
---

# High Fidelity HTML Reconstruction

## Core Rule

Treat the uploaded screenshot/design as the only visual reference. For image-heavy reconstructions, create assets through this exact chain:

`reference screenshot only -> GPT img2img green_source.png -> Python extracts assets from green_source.png -> build HTML from extracted assets + real text/CSS -> browser verify`

Do not skip, reorder, or fake any step when the user requests this strict workflow.

## Required References

Before acting, read:

- `references/strict-green-source-workflow.md` for the full green-source rules.
- If available and relevant, also read `C:\Users\JT\.codex\skills\draw-ui\SKILL.md` and its `references/html-reconstruction.md` for the general screenshot-to-HTML reconstruction strategy.

## Workflow

1. **Identify asset strategy**
   - Decide which regions must remain complete image assets: banners, ads, hero cards, promotional cards, video covers, product/person/scenery images, artistic text, badges, special labels, and composite UI artwork.
   - Rebuild ordinary navigation labels, descriptions, simple buttons, prices, metadata, and layout with HTML/CSS when they are not part of a complete visual asset.

2. **Generate `green_source.png` first**
   - Use GPT/img2img or the best available image generation tool with the screenshot as the reference.
   - Generate one high-resolution green-background sprite sheet named exactly `green_source.png`.
   - The background must be uniform pure `#00FF00`.
   - Place every needed independent visual asset directly on the green background with generous gaps.
   - Generate assets large enough in this stage. Do not rely on later upscaling.
   - If no real img2img/image-generation path is available, stop and report the blocker. Do not create a local substitute with Python, Canvas, SVG, screenshots, or manual drawing.

3. **Extract assets with Python**
   - Python may read only `green_source.png` for image asset extraction.
   - Do not read/crop the original screenshot for assets.
   - Detect independent regions from the green-source sheet.
   - Preserve full rectangular visual regions as complete images without internal chroma key.
   - For standalone icons/transparent elements, remove only the external connected green background, compute a smooth alpha mask from color distance, clean green spill, preserve transparent padding, and export RGBA PNG.
   - Do not upscale extracted assets. Only keep original generated size or shrink with high-quality filters.
   - Export every usable asset to `assets/`.
   - Export `asset_manifest.json` with filename, type, usage, source bbox, source size, crop size, output size, scale, transparency method, and padding.

4. **Build the page**
   - Create `index.html`.
   - Use only assets extracted from `green_source.png` for image regions.
   - Do not use the original screenshot, direct crops from it, network images, placeholders, or local redraws as page assets.
   - Use HTML/CSS for ordinary text, layout, grid, cards, controls, spacing, colors, radius, fixed/floating layers, and responsive behavior.
   - Display high-resolution assets at the correct visual size through CSS so they shrink cleanly.
   - Keep modules, counts, hierarchy, scrolling, and aspect ratio aligned with the screenshot.
   - Do not add modules, decorative content, images, text, or interactions that are not present unless the user explicitly asks.

5. **Verify**
   - Open `index.html` in a browser at the screenshot-like viewport.
   - Save a verification screenshot.
   - Check that all image sources in HTML point only to `assets/` derived from `green_source.png`.
   - Check for layout overlap, wrong aspect ratios, missing assets, green remnants, jagged edges, and text/container overflow.

## Output Checklist

Deliver:

- `green_source.png`
- `assets/*.png`
- `asset_manifest.json`
- `index.html`
- any necessary local CSS/JS files
- verification screenshot when possible

Keep the final user response concise and avoid extra explanation when the user asks for strict artifact delivery.
