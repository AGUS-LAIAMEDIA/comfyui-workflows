# 03 · SDXL film texture

![Pocket watch still life with weighted film texture and a second pass. Juggernaut XI](preview.jpg)

Analog texture terms (grain, halation, chromatic aberration) written at normal weight in a long SDXL prompt simply disappear. This workflow is what I arrived at in [test 04](../../tests/04_sdxl-prompt-weights). The image above was generated with these exact settings.

**File:** [`sdxl-film-texture.json`](sdxl-film-texture.json)

## How it works

1. The first part of the prompt describes the scene.
2. The texture terms go at the end, with explicit weights:
   ```
   (fine 35mm film grain:1.4), (subtle chromatic aberration on edges:1.3), (soft halation on highlights:1.3), organic analog texture
   ```
3. A short negative prompt handles small cleanups.
4. A second KSampler re-runs the result at **denoise 0.3** to polish surfaces.

| Setting | Value |
|---|---|
| Checkpoint | Juggernaut XI |
| Frame | 1216 × 832 |
| Pass 1 | dpmpp_2m / karras · 25 steps · CFG 7 · denoise 1.0 |
| Pass 2 | same settings · denoise 0.3 |
| Seed | 584502784340658, fixed |

## Limits

- Above ~1.6 the weights stop reading as texture and start reading as a filter.
- None of this fixes fine geometry: the numbers on the dial never become legible. That's a structural limit of SDXL without extra control, and the tools for it are ControlNet and inpainting, not prompting.
- The prompt contains `BREAK` before the texture block. Stock ComfyUI reads it as a plain word, not as a chunk separator, so it's not doing what the A1111 keyword does. I left it in because it was part of the run that made this image. For real separation, encode the texture block in its own CLIP Text Encode and join both with **Conditioning (Concat)**.

Model links are in the [main README](../../README.md#models).
