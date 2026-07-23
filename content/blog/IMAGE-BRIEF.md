# Image Brief — Wooden Masala Box blog post

> **Status:** Prompts + SEO wiring ready. Pixel generation is pending — the
> banana/Gemini MCP backend (`gemini_generate_image`) is not connected in this
> session, and the `scripts/generate.py` fallback needs a `GEMINI_API_KEY`.
> See "How to actually generate" at the bottom.

Two assets: a **blog hero** (in-article) and an **OG/social preview** (link cards).

---

## Asset 1 — Blog hero (16:9, 2K)

- **Use case:** Blog Hero → aspect `16:9`, resolution `2K`, domain mode: Editorial/Product
- **Filename:** `wooden-masala-box-hero-1600x900.webp`
- **Alt text:** `Handcrafted wooden masala box with seven spice compartments and a wooden spoon on a bright Indian kitchen counter`

**Crafted prompt (Creative Director brief):**
```
Editorial product photograph of a handcrafted wooden masala box on a clean,
sunlit kitchen counter. A single-block acacia-wood box with seven round
compartments, each holding a different vivid Indian ground spice — turmeric
yellow, red chilli, brown coriander, cumin, garam masala, white salt — with a
small matching wooden serving spoon resting on the rim. Warm natural side light
from a window, soft shadows, shallow depth of field, subtle steam of morning
tea in the far background. Muted terracotta-and-cream palette, premium
lifestyle look, no text, no logos, no hands. Shot on 50mm, f/2.8, high detail
on the wood grain.
```
- **Settings:** `gemini-3.1-flash-image-preview` @ 2K, aspect `16:9`
- **Est. cost:** ~$0.08

---

## Asset 2 — OG / social preview (16:9, 1K)

- **Use case:** OG/Social Preview → aspect `16:9`, resolution `1K`, domain mode: Product
- **Filename:** `wooden-masala-box-og-1200x630.webp`
- **Alt text:** `Wooden masala box buying and care guide — Brick Brown`

**Crafted prompt:**
```
Clean, bright product-hero shot of a single handcrafted wooden masala box with
seven colourful spice compartments and a wooden spoon, centred on a soft cream
background with generous negative space on the left third for a title overlay.
Warm studio lighting, crisp focus on the wood grain and spice colours,
minimalist premium e-commerce style, no text, no hands.
```
- **Settings:** `gemini-3.1-flash-image-preview` @ 1K, aspect `16:9`
- **Est. cost:** ~$0.04
- Add your title text ("Wooden Masala Box — Buying & Care Guide") in Canva/theme
  over the left negative space; leave the generated image text-free.

---

## Post-generation SEO checklist

1. **Convert to WebP:** `magick hero.png -quality 85 wooden-masala-box-hero-1600x900.webp`
2. **File size targets:** hero < 200 KB, OG < 100 KB
3. **Alt text:** use the strings above (keyword-forward, descriptive)
4. **Wire the hero into `wooden-masala-box.schema.json`** — replace the placeholder
   `image.url` with the live CDN path
5. **OG meta tags** (add to the article `<head>`):
```html
<meta property="og:image" content="https://brickbrown.com/cdn/shop/articles/wooden-masala-box-og-1200x630.webp" />
<meta property="og:image:width" content="1200" />
<meta property="og:image:height" content="630" />
<meta property="og:image:alt" content="Wooden masala box buying and care guide — Brick Brown" />
```

## How to actually generate (pick one)

- **Enable the banana extension** in a session that allows it: `./extensions/banana/install.sh`,
  then re-run — I'll call `gemini_generate_image` with the prompts above.
- **Local fallback:** set `GEMINI_API_KEY` (from https://aistudio.google.com/apikey), then
  `python3 scripts/generate.py --prompt "<hero prompt>" --aspect-ratio "16:9"`.
- **Any other generator** (Firefly, Midjourney, etc.): paste the crafted prompts as-is.
