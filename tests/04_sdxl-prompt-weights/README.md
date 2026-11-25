# Test 04 · SDXL prompt weights for film texture

**Problem:** adding "film grain, halation, chromatic aberration" to a long SDXL prompt changed nothing I could see.

**Fixed:** Juggernaut XI · seed 584502784340658 (never changed) · CFG 7 · 25 steps · dpmpp_2m / karras · 1216 × 832 · same base prompt (a pocket watch taken apart on a workbench).
**Changed:** only the texture part of the prompt, plus the extras in rows F and G. July 2026.

![Seven versions of the same still life](prompt-weights.jpg)

| | Change | Result |
|---|---|---|
| A | Control: no texture terms | Reference |
| B | Texture terms at normal weight | No visible difference from A, even at 200–300% zoom |
| C | Weights 1.3–1.4 | First real, controlled difference |
| D | Weights 1.6–1.7 | Too much: it reads as a filter, not as film |
| E | C + `BREAK` before the texture block | Slightly more integrated to my eye. See the note below |
| F | E + negative prompt | Small push: cleaner edges, nothing structural |
| G | F + second pass at denoise 0.3 | Polishes surfaces, doesn't fix geometry |

Texture block used from row C on:

```
(fine 35mm film grain:1.4), (subtle chromatic aberration on edges:1.3), (soft halation on highlights:1.3), organic analog texture
```

## Findings

- **Weights of 1.3–1.4 are the sweet spot.** Below that, the terms drown in a long prompt. Above ~1.6 it looks like a plugin.
- **Fine geometry is a model limit.** The numbers on the dial never became legible in any version, just like the fingers in [test 03](../03_sdxl-cfg). That's work for ControlNet and inpainting, not for prompting.
- **A fixed seed doesn't fix the composition.** The seed only sets the starting noise; changing the text changes the conditioning, and the layout drifts with it. For a strict A/B, generate the base once and run the variants as img2img at low denoise.

## Note on BREAK

When I wrote this up I checked how `BREAK` behaves: stock ComfyUI doesn't parse it the way A1111 does. It reads it as an ordinary word. So the difference between C and E most likely comes from the extra word changing the conditioning, not from a real separation of the prompt. For true separation, encode the texture block in its own CLIP Text Encode node and join it with **Conditioning (Concat)**, or use a custom node that implements BREAK. That's the next version of this test.

Workflow: [03 · SDXL film texture](../../workflows/03_sdxl-film-texture) is row G.
