---
layout: single
title: "Research"
permalink: /research/
author_profile: true
---

The question running through my research: **how does the choice of training formulation shape what a model can represent, where it fails, and how detectable those failures are?**

This connects work on diffusion distillation, open-set recognition, policy learning, and on-device deployment — not as separate topics but as different angles on the same underlying problem of representational competence under distribution shift.

---

## Generative AI & Efficient Inference

**The covariate shift problem in distilled diffusion models.** Standard distillation trains a student on teacher outputs at fixed time steps. But at inference the student conditions on its *own* previous outputs, not the teacher's — a trajectory mismatch that compounds across denoising steps and collapses diversity. I treated this as a sequential decision-making problem: the student is a policy, its denoising trajectory is a rollout, and covariate shift is the standard imitation learning failure mode. The fix is DAgger-style on-policy correction (DDIL), which eliminates trajectory divergence and preserves marginal distributions at intermediate steps.

This framing unified several concurrent distillation methods (DMD2, LADD, Clockwork) and led directly to the deployment pipeline for sub-0.6s Stable Diffusion on mobile.

**World's first on-device generative AI.** Led the cross-functional team at Qualcomm that shipped the world's first and fastest text-to-image generation on a mobile device: sub-0.6s Stable Diffusion on Snapdragon hardware. End-to-end stack: progressive distillation, W4A8 quantization, on-device memory management. Featured at MWC'23, Snapdragon Summit, and Qualcomm earnings. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html).

<div style="max-width: 340px; margin: 1.5em 0;">
  <iframe width="340" height="604" src="https://www.youtube.com/embed/64VsQHhImQI" frameborder="0" allowfullscreen></iframe>
</div>

---

## Foundation Models & Embodied AI

**Foundational modeling: inference, generation, and unified objectives.** A thread connecting speculative decoding for autoregressive models, discrete diffusion for language, and unified generative frameworks (e.g., masking-augmented diffusion with inference-time scaling). The underlying question across these: what does the choice of training objective — next-token prediction, score matching, masked diffusion — commit the model to at inference time, and where do those commitments become bottlenecks? Speculative decoding is one answer to the efficiency side; discrete diffusion and hybrid objectives are a bet on the representation side.

**Vision-language-action models and diffusion policies.** Research on VLA training, and RL-based action generation for autonomous driving.

**Distributed training at scale.** Architected multi-node training infrastructure (FSDP/ZeRO-3, model sharding, streaming dataloaders) for billion-parameter diffusion and foundation models.

---

## Uncertainty, Representation & Safety

**Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern).** The core question: what does a learned representation actually capture about the data manifold, and when does it fail silently?

**Open-set detection.** Models trained on closed-world distributions encounter out-of-distribution inputs at deployment. I studied what representation structure (contrastive, ensemble, flow-based, VAE-based) makes unknown-class inputs detectable — and where that structure breaks down.

<!-- **Calibrated uncertainty.** Confidence scores from deep networks are often miscalibrated under distribution shift. Work on post-hoc calibration and representation-level approaches (normalizing flows, deep ensembles) to produce uncertainty estimates that are reliable as a safety signal. -->

**Risk-sensitive RL and constrained MDPs.** How to incorporate uncertainty estimates into policy learning: constraint-based approaches that penalize policies for acting in regions where the world model is unreliable.

The thread connecting this body of work: **competence estimation** — knowing not just what a model predicts, but whether to trust the prediction. This is now more relevant than ever given the scale of deployed foundation models.

---

<!-- ## Current Work & Open Questions

**The central question.** How do you realize latent representations with multiple implicit levels of hierarchy — yet controllable, and with reliable notions of competence? This question connects back to the open-set and hybrid representation work directly: what a representation preserves determines what's detectable, what's compressible, and what's trustworthy. The difference now is that the question is live for foundation models at a scale where probing it is hard, and where the failure modes are less legible.

**SLMs augmented with on-policy distillation as the testbed.** Frontier-scale models are the wrong place to study this — too large to ablate, too opaque to probe systematically. Small language models trained with on-policy distillation are the right setting: use a frontier model as teacher, verifier, or critic; distill the useful control signal; and study whether that training pressure produces representations that are hierarchically structured, competence-aware, and increasingly self-sufficient as the harness is amortized away. If the representation structure we want exists in small models, it likely generalizes.

**Diffusion LLMs as a lever.** Discrete diffusion naturally extends the atomic horizon of generation — where next-token prediction commits one token at a time, diffusion operates over spans in a finite-horizon, MPC-style manner. That may yield smoother or more compressible internal states, and a cleaner substrate for hierarchical latent structure than autoregressive decoding forces. Skip-to-Good-Part gives early evidence that diffusion and AR models encode measurably different representational geometry; the open question is whether those differences are exploitable for controllability and hierarchy — not just efficiency.

**Open questions.** 
- World models: what controllable abstraction bridges token/KV-cache systems with strong planning interfaces and video-diffusion systems with richer implicit dynamics? 
- Harness amortization: when can external judges, teachers, and verifiers be internalized rather than permanently scaffolded? 
- Small model distillation: what survives compression while preserving structured reasoning? -->
