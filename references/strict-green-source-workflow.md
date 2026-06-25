# Strict Green-Source Workflow

Use this reference when the user requests high-fidelity screenshot/design-to-HTML conversion with a green-source asset pipeline.

## Non-Negotiable Order

1. Original screenshot is only a visual reference.
2. Generate `green_source.png` with GPT/img2img.
3. Use Python to extract assets only from `green_source.png`.
4. Output independent PNG assets and `asset_manifest.json`.
5. Build `index.html` using those assets.
6. Verify in browser.

Never create a local fake green source with Python, Canvas, SVG, CSS, screenshot crops, or manual drawing. Never crop final assets directly from the original screenshot.

## `green_source.png` Rules

- Name the generated file exactly `green_source.png`.
- Use the highest practical resolution supported by the current image model/tool.
- Use one uniform pure `#00FF00` background: no gradient, shadow, noise, texture, lighting variation, labels, borders, crop marks, or watermarks.
- Place all reusable visual assets directly on the green background.
- Keep generous green spacing so automatic or semi-automatic detection can separate assets.
- Generate each asset large enough in the model output:
  - complete banners/cards/covers: at least 2x final display size when possible;
  - complex text/badges/fine detail: 2x-3x final display size when possible;
  - independent small icons: at least 128x128, preferably 160x160 or 192x192 for fine-line or badge icons.
- Do not shrink assets to force everything into one small canvas. Prefer a larger green-source canvas.
- Do not add elements that are absent from the screenshot.
- Do not include ordinary text that can be rebuilt with HTML/CSS unless it is part of an inseparable image asset.

## What Must Stay As Complete Image Assets

Preserve as whole rectangular assets:

- banners, ads, promotional cards, hero images, activity cards, video covers, product covers;
- any region whose internal text, badges, tags, corner marks, rounded visual effects, or artwork are part of the image;
- screenshots/product/person/scenery/illustration/decorative images;
- special artistic text, graphic logos, badges, status marks, and composite UI art.

Do not split internal elements of a complete visual card unless the screenshot clearly uses them independently.

## What Usually Becomes HTML/CSS

Rebuild as real page elements:

- ordinary navigation/menu/category labels;
- simple titles, descriptions, dates, prices, metadata, stats, and button text;
- simple boxes, tabs, grids, cards, search inputs, normal buttons, dividers, spacing, shadows, and layout.

## Python Extraction Rules

Python may:

- read `green_source.png`;
- identify independent asset regions;
- distinguish full rectangular images from standalone transparent assets;
- generate smooth alpha from chroma key;
- remove green spill at semi-transparent edges;
- crop with safe transparent padding;
- shrink assets with high-quality filtering;
- lightly sharpen only when needed;
- write PNG assets and `asset_manifest.json`.

Python must not:

- read original screenshot pixels for assets;
- redraw, repaint, repair, generate, replace, recolor, or alter asset shapes;
- uniformly upscale extracted assets;
- simply delete every pixel equal/similar to green without considering external connected background;
- destroy valid green/teal content inside an asset;
- use nearest-neighbor or low-quality resizing;
- over-sharpen or create halos, black/white edges, jaggies, noise, or line thickening.

Recommended extraction method:

1. Read `green_source.png`.
2. Flood-fill from canvas edges to identify external connected green background.
3. Detect non-background connected components.
4. Classify complete rectangular visual regions vs transparent standalone assets.
5. For transparent assets, compute alpha from green distance only in relation to external/background regions.
6. Feather alpha about 0.5-1px when needed.
7. Despill green only on semi-transparent edges.
8. Preserve safe padding and export RGBA PNG.
9. Export full rectangular assets without internal keying.
10. Write manifest fields:
   - `file`
   - `type`
   - `usage`
   - `source_bbox`
   - `source_size`
   - `crop_size`
   - `output_size`
   - `scale`
   - `transparency`
   - `padding`

## HTML Reconstruction Rules

- Use `index.html` as the final page.
- Use only `assets/*.png` derived from `green_source.png` for image regions.
- Do not reference the original screenshot or generated source sheet directly in the page.
- Set display sizes with CSS based on the original screenshot, not the raw asset pixel size.
- Preserve aspect ratios; do not stretch or distort images.
- Rebuild normal text and controls as HTML/CSS.
- Keep image-internal text inside the image asset and do not duplicate it in HTML.
- Preserve page module count, layout, gaps, colors, radius, visual hierarchy, fixed/floating areas, and scrolling behavior.
- Support the screenshot's main viewport ratio and avoid overlap/overflow across nearby widths.

## Verification

- Open the page in an independent browser session.
- Capture a screenshot at the target viewport.
- Confirm no direct original screenshot references exist in HTML/CSS.
- Inspect assets for:
  - green remnants;
  - broken alpha;
  - clipped edges;
  - insufficient padding;
  - jagged small icons;
  - wrong aspect ratio;
  - low-resolution display.

If an extracted asset is too low resolution, regenerate a better `green_source.png`; do not upscale the extracted asset as a substitute.
