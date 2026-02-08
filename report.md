# Report: StyleGAN2 + CLIP Finetuning (StyleGAN-NADA)

## Summary of Work Done

Implemented text-driven adaptation of a pretrained StyleGAN2 generator based on the paper [*StyleGAN-NADA: CLIP-Guided Domain Adaptation of Image Generators*](https://arxiv.org/abs/2108.00946).

**Core idea:** The generator is finetuned so that the *direction* in CLIP image space (from frozen reference image to trained output) aligns with the *direction* in CLIP text space (from source to target prompt). No target-domain images are required.

**Deliverables:**
- **v1** — training notebook with straightforward approach: synthesis network trained as a whole, mapping frozen, directional CLIP loss
- **v2** — training notebook with adaptive layer-freezing: latent optimization selects k layers, only those layers are trained per iteration
- **Inference notebook** — load checkpoints from both versions and generate samples
- Support for Google Drive and local storage for checkpoints and outputs

---

## Experimental Results (Model Example)

On the tested domain shift (**photo → anime/cartoon**):

| Version | Approach | Result |
|---------|----------|--------|
| **v1** | Simple: full synthesis training, frozen mapping | **Better / comparable quality** |
| **v2** | Adaptive layer-freezing, latent optimization | Did not outperform v1 |

**Conclusion:** The more complex v2 approach (adaptive layer-freezing, selective layer training) did not yield stronger results than the simpler v1 on this model example. Possible reasons: dataset/prompts specifics, hyperparameters, or that for moderate domain shifts the simpler full-network training is sufficient; adaptive freezing may help more on very strong domain shifts or limited memory.
