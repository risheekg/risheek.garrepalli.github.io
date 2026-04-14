---
layout: single
title: "Research"
permalink: /research/
author_profile: true
---

My research is organized around a single question that surfaces differently across domains: **how does the choice of training formulation shape what a model can represent, where it fails, and how detectable those failures are?**

This question connects work on diffusion distillation, open-set recognition, policy learning, and on-device deployment — not as separate topics but as different instantiations of the same underlying problem of representational competence under distribution shift.

---

## Generative AI & Efficient Inference

**The covariate shift problem in distilled diffusion models.** Standard distillation trains a student on teacher outputs at fixed time steps. But at inference the student conditions on its *own* previous outputs, not the teacher's — a trajectory mismatch that compounds across denoising steps and collapses diversity. I treated this as a sequential decision-making problem: the student is a policy, its denoising trajectory is a rollout, and covariate shift is the standard imitation learning failure mode. The fix is DAgger-style on-policy correction (DDIL), which eliminates trajectory divergence and preserves marginal distributions at intermediate steps.

This framing unified several concurrent distillation methods (DMD2, LADD, Clockwork) and led directly to the deployment pipeline for sub-0.6s Stable Diffusion on mobile.

**World's first on-device generative AI.** Led the cross-functional team at Qualcomm that shipped the world's first and fastest text-to-image generation on a mobile device: sub-0.6s Stable Diffusion on Snapdragon hardware. End-to-end stack: progressive distillation, W4A8 quantization, on-device memory management. Featured at MWC'23, Snapdragon Summit, and Qualcomm earnings. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html).

<div style="max-width: 340px; margin: 1.5em 0;">
  <iframe width="340" height="604" src="https://www.youtube.com/embed/64VsQHhImQI" frameborder="0" allowfullscreen></iframe>
</div>

**Efficient video perception with calibrated uncertainty.** Temporally-consistent depth estimation and optical flow under extreme latency constraints (<10ms), with calibrated uncertainty — useful for downstream tasks that need to know when the perception system is unreliable.

---

## Foundation Models & Embodied AI

**Speculative decoding for autoregressive models.** Led research on draft-then-verify acceleration for LLMs and VLAs at Qualcomm. The core challenge is not just speed but maintaining output distribution fidelity under on-device memory budgets.

**Vision-language-action models and diffusion policies.** Research on VLA training, on-policy distillation for action chunking, and RL-based action generation for robotic manipulation. The connection to the diffusion distillation work is direct: a diffusion policy is a conditional sampler, and the same trajectory covariate shift problem applies.

**Distributed training at scale.** Architected multi-node training infrastructure (FSDP/ZeRO-3, model sharding, streaming dataloaders) for billion-parameter diffusion and foundation models.

---

## Uncertainty, Representation & Safety

**Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern).** The core question: what does a learned representation actually capture about the data manifold, and when does it fail silently?

**Open-set detection.** Models trained on closed-world distributions encounter out-of-distribution inputs at deployment. I studied what representation structure (contrastive, ensemble, flow-based, VAE-based) makes unknown-class inputs detectable — and where that structure breaks down.

**Calibrated uncertainty.** Confidence scores from deep networks are often miscalibrated under distribution shift. Work on post-hoc calibration and representation-level approaches (normalizing flows, deep ensembles) to produce uncertainty estimates that are reliable as a safety signal.

**Risk-sensitive RL and constrained MDPs.** How to incorporate uncertainty estimates into policy learning: constraint-based approaches that penalize policies for acting in regions where the world model is unreliable.

The thread connecting this body of work: **competence estimation** — knowing not just what a model predicts, but whether to trust the prediction. This is now more relevant than ever given the scale of deployed foundation models.
