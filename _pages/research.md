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

The fix is DAgger-style correction ([DDIL](https://arxiv.org/abs/2410.11971)), which trains on the mixed distribution of student-induced and teacher-induced latent states. I first introduced DDIL to progressive distillation (behavior cloning), resolving co-variate shift/exposure bias and enabling the world's first sub-0.6s Stable Diffusion mobile deployment. 

Subsequently, I extended DDIL to __on-policy distillation__. While concurrent methods like DMD2 address on-policy training, they still struggle to preserve diversity. By leveraging a DAgger framework to train on the teacher-induced distribution too, DDIL preserves intermediate marginal distributions. This approach provides an effective mechanism for amortizing test-time search in LLM post-training.


**World's first on-device generative AI.** ML lead in the cross-functional team at Qualcomm that shipped the world's first and fastest on-device text-to-image generation: sub-0.6s Stable Diffusion on Snapdragon hardware. Owned the distillation problem end-to-end — built the training pipeline, managed 50TB data pipelines via MosaicML MDS, progressive distillation, and W4A8 quantization — and modified diffusion sampling for on-target quality. Featured at MWC'23, Snapdragon Summit, and Qualcomm earnings. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html).

<div style="max-width: 340px; margin: 1.5em 0;">
  <iframe width="340" height="604" src="https://www.youtube.com/embed/64VsQHhImQI" frameborder="0" allowfullscreen></iframe>
</div>

---

## LLMs, World Models & Embodied AI

**Controllable abstractions for foundation models.** The organizing question: does the training objective — next-token prediction, score matching, masked diffusion — determine whether a model's internal state is compressible, hierarchically structured, and manipulable for downstream control? AR decoding has a structural redundancy problem: token-by-token commitment forces sequential dependencies through confident and uncertain positions alike; speculative execution patches this locally but doesn't resolve the underlying geometry. Discrete diffusion operates over spans via multi-token *refinement* rather than regeneration, concentrating compute at genuinely uncertain positions — a different commitment structure with dual implications for efficiency and controllability. [*Skip-to-Good-Part*](https://arxiv.org/abs/2603.07475) gives direct empirical evidence that diffusion and AR models encode measurably different representational geometry, establishing that the gap is real and exploitable. [*SPIFFY*](https://arxiv.org/abs/2509.18085) and [*ConFu*](https://arxiv.org/abs/2603.08899) translate this into concrete inference gains, targeting the commitment-structure difference directly in agentic and reasoning settings where sequential overhead compounds. A model scaling study (in preparation) asks whether these representational and efficiency advantages compound or attenuate with scale.

**Embodied AI systems.** VLA training, diffusion policies for manipulation, and RL-based action generation; model-theoretic framing and open questions in current work below.

**Distributed training at scale.** Architected multi-node training infrastructure (FSDP/ZeRO-3, model sharding, streaming dataloaders) for billion-parameter diffusion and foundation models.

---

## Competence Estimation, Self-Knowledge & Failure Detectability

**Competence estimation under distribution shift.** My graduate work asked what a representation actually preserves about the data manifold, and whether that preserved structure is enough to expose failure before deployment. This connects directly to current questions around verifier routing, scaffold necessity, and when a model can estimate its own competence rather than relying on external harnesses.

**Open-set detection.** Models trained on closed-world distributions encounter out-of-distribution inputs at deployment. I studied what representation structure (contrastive, ensemble, flow-based, VAE-based) makes unknown-class inputs detectable — and where that structure breaks down.

**Risk-sensitive RL and constrained MDPs.** How to incorporate uncertainty estimates into policy learning: constraint-based approaches that penalize policies for acting in regions where the world model is unreliable.

The thread connecting this body of work: **competence estimation** — knowing not just what a model predicts, but whether to trust the prediction. This is now more relevant than ever given the scale of deployed foundation models.

---

## Current Research & Open Questions

My current work sits at the intersection of architecture, training methodology, and world modeling — asking what it takes for a model to reason iteratively, learn efficiently from its own rollouts, and acquire grounded physical representations. Three active themes approach this from different angles.



## Open Questions

- **Do iterative / diffusion-style models offer a better substrate for controllable reasoning than autoregressive models, or just a different compute tradeoff?**
- **Which latent states are valid under intervention, not just prediction?** A representation can support open-loop forecasting yet still fail under replanning or counterfactual control.
- **Can competence be learned as a robust, transferable object across modalities?**
- **What should be amortized from external scaffolds into the base model, and what must remain external?** Selection policies, value functions, search dynamics, or only parts of them?
<!-- - **What counts as genuine internalization?** How do we distinguish a model that has absorbed useful control structure from one that is only mimicking scaffolded behavior on-distribution?  -->


### Directions

**1. Diffusion LLMs and Looped Architectures.**
Autoregressive model have fundamental constraint with token-by-token decoding, requiring test-time compute (CoT) to plan or backtrack. Discrete diffusion commits over spans (finite-horizon MPC), natively enabling multi-token refinement — yielding potentially smoother internal states and a cleaner substrate for hierarchical latent structure. Looped transformers and mixture-of-recursion architectures add a complementary axis: iterative refinement via depth-wise parameter sharing, where passes adapt to difficulty — concentrating compute across reasoning depth, not just position. The question is whether this structural difference produces a genuinely better substrate for controllable, efficient reasoning, and whether the advantage compounds with scale.

**2. Multimodal world models and grounded control.**
The TAMP framework illuminates a current divide: natively multimodal token/KV-cache systems (VLAs) excel as model-free, feedforward task planners with strong LLM-native reasoning but remain open-loop; video-prediction and diffusion world models act as model-based feedback controllers, learning physics-aware priors that feedforward reasoning structurally lacks. Within the model-based world, JEPA-style latent prediction offers rich semantic structure efficiently, while video/diffusion world models capture richer physical dynamics with a clearer path to extreme-scale pretraining (Veo-class). The question is what controllable intermediate bridges these paradigms at scale towards a unified substrate across memory, reasoning, and action.

**3. On-policy distillation and amortization.**
[DDIL](https://arxiv.org/abs/2410.11971) established that mixed-distribution correction resolves covariate shift in distilled diffusion samplers. The broader question is how far this recipe generalizes — to other regimes where a model trains or deploys under its own induced distribution. A related question is when a model can estimate its own competence well enough to decide what to amortize from external scaffolding versus what must stay external.

---

These threads share one concern: building models whose internal structure is rich enough to support planning and self-monitoring, while being explicit about which external scaffolding is still load-bearing.