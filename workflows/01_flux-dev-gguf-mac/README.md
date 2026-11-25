# 01 · FLUX.1 dev on Apple Silicon

![A speed skater alone on an empty Olympic rink. FLUX.1 dev, 1280 × 544](preview.jpg)

Base text-to-image for FLUX.1 [dev], quantized to GGUF Q5_K_S so it fits in the 24 GB of unified memory of a MacBook Pro M4 Pro. The image above was generated with these exact settings.

**File:** [`flux-dev-gguf-mac.json`](flux-dev-gguf-mac.json)

| Setting | Value | Why |
|---|---|---|
| Model | `flux1-dev-Q5_K_S.gguf` | Small quality loss in exchange for a size that fits on a 24 GB Mac |
| Text encoders | `t5xxl_fp8_e4m3fn` + `clip_l` | The standard FLUX pair |
| Frame | 1280 × 544 | 2.35:1 scope |
| Sampler / scheduler | euler / simple | See tests [01](../../tests/01_flux-samplers) and [02](../../tests/02_flux-schedulers) |
| Steps | 28 | |
| CFG | 1.0 | FLUX dev is guidance-distilled: you steer it with FluxGuidance instead |
| FluxGuidance | 3.0 | My usual range is 3.0–3.5 |
| Seed | 998374099277766, fixed | |

## Notes from practice

- FLUX reads prompts as prose. I write them like a shot description: subject, space, light, lens, palette.
- At CFG 1.0 the negative prompt has no effect, so it stays empty.
- Starting from scratch, closed models (Midjourney, Gemini) often infer the world around a request better than FLUX. FLUX's literalness pays off later, once you control it with ControlNet and LoRAs.

Requires the [ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF) custom node. Model links are in the [main README](../../README.md#models).
