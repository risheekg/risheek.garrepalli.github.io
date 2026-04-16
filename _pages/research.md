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

## Current Work & Open Questions

**Information organization, routing, and feedback.** The organizing question across these directions: how should information be structured in representations to support reliable routing for reasoning, iterative Bayesian-style planning updates, and feedback-aware memory compression? Competence estimation — knowing when a representation's organization is trustworthy enough to act on — is the grounding check throughout. On-policy distillation, verifier routing, and diffusion LLMs give concrete handles on each of these dimensions that didn't exist before.

**Closing the Action Loop: Unifying Task Planners and Continuous World Models.** The classic Task and Motion Planning (TAMP) framework informs the current architectural divide in foundational simulators. On one side, natively multimodal token/KV-cache systems act as model-free, feed-forward task planners (VLAs) that excel at LLM-native agentic reasoning but remain open-loop. On the other side, explicit latent models (JEPA) and video-prediction systems (diffusion) act as model-based, feedback controllers for motion planning. **The objective is a controllable intermediate: a unified substrate across memory, reasoning, and action.** Scaling continuous, physics-aware world models isn't just about motion execution—it is the path to learning foundational physical priors that pure text-based reasoning lacks. Natively multimodal architectures with shared KV-cache (BAGEL, Transfusion, Gemma, Qwen3.5) are converging on this from one direction, while video generation approaches it from the other.

**Diffusion and looped transformers as substrates for System 2 planning.** Autoregressive models dominate via scaling laws, but their fundamental constraint is token-by-token commitment, requiring external test-time compute (CoT) to plan or backtrack. Discrete diffusion commits over spans (finite-horizon MPC), natively enabling multi-token refinement — yielding smoother internal states and a cleaner substrate for hierarchical latent structure, and the mechanism motivating [*Skip-to-Good-Part*](https://arxiv.org/abs/2603.07475). Looped transformers and mixture-of-recursion architectures add a complementary axis: iterative refinement via depth-wise parameter sharing, where passes adapt to difficulty — concentrating compute across reasoning depth, not just position. Together, span-level commitment and adaptive depth form a unified design space for controllable internal computation. The open question is whether looped-diffusion hybrid architectures deepen the representational geometry gap further, and whether these structural advantages in controllability and efficiency compound with scale.

**Amortizing Harness, Search and Test-Time Compute.** Verifiers, reward models, and MCTS-style planners reveal the control signals that base models miss during next-token prediction. The question is internalization: what part of this explicit test-time search and scaffolding should be amortized back into the base policy. I view on-policy distillation and RL-driven reasoning not just as alignment, but as the mechanism to force representations to become inherently hierarchically structured and self-sufficient as the external harness is amortized away.
