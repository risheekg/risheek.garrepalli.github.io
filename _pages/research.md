---
layout: single
title: "Research"
permalink: /research/
author_profile: true
---

The question running through my research: **how does the choice of training formulation shape what a model can represent, where it fails, and how detectable those failures are?** Today that question shows up in post-training, agent scaffolding, multimodal world models, and small-model distillation: what internal competence is actually learned, what still has to remain external, and how do we know the difference?

This connects work on diffusion distillation, open-set recognition, policy learning, and on-device deployment — not as separate topics but as different angles on the same underlying problem of representational competence under distribution shift.

---

## Generative AI & Efficient Inference

**The covariate shift problem in distilled diffusion models.** Standard distillation trains a student on teacher outputs at fixed time steps. But at inference the student conditions on its *own* previous outputs, not the teacher's: a trajectory mismatch that compounds across denoising steps and collapses diversity. I treated this as a sequential decision-making problem: the student is a policy, its denoising trajectory is a rollout, and covariate shift is the standard imitation learning failure mode. 

The fix is DAgger-style on-policy correction (DDIL), which trains on the mixed distribution of student-induced and teacher-induced latent states: student-induced trajectories correct the covariate shift (the student learns to recover from its own denoising errors), while teacher-induced trajectories preserve diversity and recover marginal distributions at intermediate steps. In hindsight, this is an early instance of the same core idea now central in LLM post-training: the model must be trained on the states induced by its own behavior, not just teacher-conditioned trajectories.

This framing helps connect several concurrent distillation methods (DMD2, LADD) and led directly to the deployment pipeline for sub-0.6s Stable Diffusion on mobile.

**World's first on-device generative AI.** ML lead in the cross-functional team at Qualcomm that shipped the world's first and fastest on-device text-to-image generation: sub-0.6s Stable Diffusion on Snapdragon hardware. Owned the distillation problem end-to-end — built the training pipeline, managed 50TB data pipelines via MosaicML MDS, progressive distillation, and W4A8 quantization — and modified diffusion sampling for on-target quality. Featured at MWC'23, Snapdragon Summit, and Qualcomm earnings. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html).

<div style="max-width: 340px; margin: 1.5em 0;">
  <iframe width="340" height="604" src="https://www.youtube.com/embed/64VsQHhImQI" frameborder="0" allowfullscreen></iframe>
</div>

---

## Foundation Models & Embodied AI

**Controllable abstractions for foundation models.** An active research direction connecting speculative decoding, discrete diffusion for language, and unified generative objectives is the question of what internal state a model should actually plan in. Next-token prediction, score matching, and masked diffusion impose different priors on representation and commitments on inference, memory, and controllability. My interest is in which objectives yield smoother reasoning state, better compression, and more manipulable abstractions for post-training and agentic control.

**Vision-language-action models and diffusion policies.** Research on VLA training, and RL-based action generation for autonomous driving.

**Distributed training at scale.** Architected multi-node training infrastructure (FSDP/ZeRO-3, model sharding, streaming dataloaders) for billion-parameter diffusion and foundation models.

---

## Competence Estimation, Self-Knowledge & Failure Detectability

**Competence estimation under distribution shift.** My graduate work asked what a representation actually preserves about the data manifold, and whether that preserved structure is enough to expose failure before deployment. This connects directly to current questions around verifier routing, scaffold necessity, and when a model can estimate its own competence rather than relying on external harnesses.

**Open-set detection.** Models trained on closed-world distributions encounter out-of-distribution inputs at deployment. I studied what representation structure (contrastive, ensemble, flow-based, VAE-based) makes unknown-class inputs detectable — and where that structure breaks down.

**Risk-sensitive RL and constrained MDPs.** How to incorporate uncertainty estimates into policy learning: constraint-based approaches that penalize policies for acting in regions where the world model is unreliable.

The thread connecting this body of work: **competence estimation** — knowing not just what a model predicts, but whether to trust the prediction. This is now more relevant than ever given the scale of deployed foundation models.

---

## Current Work & Open Questions

**The central question.** The open-set work showed that what a representation preserves determines what's detectable and what's trustworthy. The same question now runs through foundation models at scale: what internal competence is actually learned, what still has to remain external, and how do we know the difference? The tractability has changed — on-policy distillation, verifier routing, and diffusion LLMs give concrete handles that didn't exist before.

**World models splitting into two camps.** Token/KV-cache systems give clean planning interfaces but limited expressive dynamics; video-diffusion gives richer implicit dynamics but weaker manipulable abstractions. The missing piece is a controllable intermediate: persistent summaries plus local detail, and a physics-aware planning interface that exploits video-scale dynamics or long-horizon reasoning chains. The bottleneck is the interface between memory, reasoning, and control.

**External harnesses as a stage, not a permanent scaffold.** Verifiers, judges, and planners reveal what control signal the base model missed. The tractable version of this question: SLMs trained with on-policy distillation, using a frontier model as teacher, verifier, or critic. Distill the useful control signal and study whether that training pressure produces representations that are hierarchically structured, competence-aware, and increasingly self-sufficient as the harness is amortized away.

**Diffusion LLMs as a lever.** Discrete diffusion naturally extends the atomic horizon of generation — where next-token prediction commits one token at a time, diffusion operates over spans in a finite-horizon, MPC-style manner. That may yield smoother or more compressible internal states, and a cleaner substrate for hierarchical latent structure than autoregressive decoding forces. Skip-to-Good-Part gives early evidence that diffusion and AR models encode measurably different representational geometry; the open question I am pursuing is whether those differences are exploitable for controllability and hierarchy — not just efficiency.