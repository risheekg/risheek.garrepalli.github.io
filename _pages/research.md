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

## LLMs, World Models & Embodied AI

**Controllable abstractions for foundation models.** The organizing question: does the training objective — next-token prediction, score matching, masked diffusion — determine whether a model's internal state is compressible, hierarchically structured, and manipulable for downstream control? AR decoding has a structural redundancy problem: token-by-token commitment forces sequential dependencies through confident and uncertain positions alike; speculative execution patches this locally but doesn't resolve the underlying geometry. Discrete diffusion operates over spans via multi-token *refinement* rather than regeneration, concentrating compute at genuinely uncertain positions — a different commitment structure with dual implications for efficiency and controllability. *Skip-to-Good-Part* gives direct empirical evidence that diffusion and AR models encode measurably different representational geometry, establishing that the gap is real and exploitable. *SPIFFY* and *ConFu* translate this into concrete inference gains, targeting the commitment-structure difference directly in agentic and reasoning settings where sequential overhead compounds. A model scaling study (in preparation) asks whether these representational and efficiency advantages compound or attenuate with scale.

**Embodied AI systems.** VLA training, diffusion policies for manipulation, and RL-based action generation; model-theoretic framing and open questions in current work below.

**Distributed training at scale.** Architected multi-node training infrastructure (FSDP/ZeRO-3, model sharding, streaming dataloaders) for billion-parameter diffusion and foundation models.

---

## Competence Estimation, Self-Knowledge & Failure Detectability

**Competence estimation under distribution shift.** My graduate work asked what a representation actually preserves about the data manifold, and whether that preserved structure is enough to expose failure before deployment. This connects directly to current questions around verifier routing, scaffold necessity, and when a model can estimate its own competence rather than relying on external harnesses.

**Open-set detection.** Models trained on closed-world distributions encounter out-of-distribution inputs at deployment. I studied what representation structure (contrastive, ensemble, flow-based, VAE-based) makes unknown-class inputs detectable — and where that structure breaks down.

**Risk-sensitive RL and constrained MDPs.** How to incorporate uncertainty estimates into policy learning: constraint-based approaches that penalize policies for acting in regions where the world model is unreliable.

The thread connecting this body of work: **competence estimation** — knowing not just what a model predicts, but whether to trust the prediction. This is now more relevant than ever given the scale of deployed foundation models.

---

## Current Work & Open Questions

**Information organization, routing, and feedback.** The organizing question across these directions: how should information be structured in representations to support reliable routing for reasoning, iterative Bayesian-style planning updates, and feedback-aware memory compression? Competence estimation — knowing when a representation's organization is trustworthy enough to act on — is the grounding check throughout. On-policy distillation, verifier routing, and diffusion LLMs give concrete handles on each of these dimensions that didn't exist before.

**LLM+World Models: Unifying Task Planners and Feedback Controllers for Embodied AI.** The classic Task and Motion Planning (TAMP) framework informs us on the current architectural divide. On one side, natively multimodal token/KV-cache systems act as model-free, feed-forward task planners (VLAs) that excel at LLM-native agentic reasoning and high-level task planning, but remain open-loop. On the other side, explicit latent models (JEPA) and video-prediction systems (diffusion) act as model-based, feedback controllers for motion planning, providing the continuous, physics-aware, closed-loop policies necessary for robustness to disturbances with implicit memory. **The missing piece is a controllable intermediate: a unified substrate across memory, reasoning, and action.** Natively multimodal architectures with shared KV-cache (BAGEL, Transfusion, Gemma, Qwen3.5) are converging on this from one direction, while JEPA and diffusion approach it from the other, aiming to finally close the loop between agentic task planning and robust motion execution.

**Diffusion LLMs and looped transformers as levers.** Discrete diffusion commits over spans (finite-horizon MPC) rather than token-by-token, yielding smoother internal states and a cleaner substrate for hierarchical latent structure — the mechanism motivating *Skip-to-Good-Part*. Looped transformers and mixture-of-recursion architectures add an orthogonal dimension: depth-wise parameter sharing where the number of passes adapts to difficulty, concentrating compute across reasoning depth rather than just position. Training-time KV-compression closes the loop, forcing the model to learn what state to persist across iteration. The open question is whether the representational geometry gap deepens in looped-diffusion hybrid architectures, and whether these structural advantages in controllability and efficiency compound or attenuate with scale.

**External harnesses as a stage, not a permanent scaffold.** Verifiers, judges, and planners reveal what control signal the base model missed. The tractable version of this question: SLMs trained with on-policy distillation, using a frontier model as teacher, verifier, or critic. Distill the useful control signal and study whether that training pressure produces representations that are hierarchically structured, competence-aware, and increasingly self-sufficient as the harness is amortized away.