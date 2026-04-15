---
permalink: /
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

I'm a Staff Research Scientist at **Qualcomm AI Research**, working on generative modeling, reinforcement learning, and representation learning. My focus is **representational competence under distribution shift**: how training objectives, score matching, next-token prediction and post-training procedures, external scaffolds shape: what a model actually captures, where it fails silently, and how detectable those failures are at inference time.

I approach these problems through probabilistic modeling and sequential decision-making, grounded in numerical optimization with theory as a diagnostic tool. This lens runs across my work on diffusion models for vision and language, on-policy distillation, open-set detection, policy learning, and the broader question of how to guide inference toward regions where a model can actually reason and reliably estimate its own competence.

**20+ publications at JMLR / ICML / NeurIPS / ICLR / CVPR / ECCV.**

---

## Highlights

**World's first sub-0.6s Stable Diffusion on mobile** — Led a cross-functional team at Qualcomm delivering the world's first and fastest on-device text-to-image generation. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html), featured at MWC'23 and Snapdragon Summit.

**Diffusion distillation from first principles** — Identified trajectory diversity collapse as a fundamental failure mode in distilled diffusion models. Proposed a DAgger-based fix (DDIL) that eliminates covariate shift between teacher and student sampling trajectories, preserving marginal distributions at intermediate denoising steps.

**Reliable representations under distribution shift** — Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern) on a question that is still central today: what does a learned representation actually retain, what information does it discard, and how does that determine whether failure is detectable? I studied how to quantify information loss and showed that augmenting discriminative objectives with generative priors yields richer representations that expose more reliable uncertainty and competence signals.

**Hardware-first grounding** — Before ML: fault-tree analysis on a satellite team (ISRO collaboration), then founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot — DoF allocation, static load analysis, EKF state estimation, 3D-LIP gait control, from concept to walking prototype.

---

## Current Research Vision

How do we realize latent representations with multiple implicit levels of hierarchy that are still controllable and carry reliable notions of competence—and when can the external scaffolding that supports them be amortized away?

*Current research directions spanning post-training, multimodal world models, and competence-aware reasoning:*

- __Are world models splitting into two camps?__ Token/KV-cache systems give clean planning interfaces; video-diffusion gives richer dynamics but weaker manipulable abstractions. The missing piece is a controllable intermediate: persistent summaries plus local detail, and a physics-aware planning interface that exploits video-scale dynamics or long-reasoning chains. The bottleneck is the interface between memory, reasoning, and control.

- __Are external harnesses a stage, rather than a crutch?__ Verifiers, judges, and planners reveal what control signal the base model missed. The question is which parts can be amortized into internal representations, and how to know when they are still necessary. Models that know *when* they need external help are the right target.

- __Representation, competence, and memory are one problem.__ "Latent reasoning," "KV compression," and "external verifiers" share a core question: *what state should persist, and in what form?* Capability gains will come from internal structures that jointly support competence estimation, memory compression, and controllable latent states.

- __Diffusion LLMs as a lever.__ Diffusion commits jointly over spans (finite horizon MPC) rather than sequentially token by token. This may yield smoother trajectories and more compressible internal states, giving a cleaner substrate for hierarchical latent structure than autoregressive decoding.

## Research Interests

[**Generative modeling & efficient inference →**](/research/#generative-ai--efficient-inference)
Diffusion models, distillation, on-device deployment. The covariate shift problem in few-step samplers. Score-based and flow-based models as probabilistic inference.

[**Foundation models, VLAs & embodied AI →**](/research/#foundation-models--embodied-ai)
Vision-language-action models, speculative decoding, diffusion policies for manipulation. How planning and uncertainty should interact in long-horizon embodied tasks.

[**Competence estimation & self-knowledge →**](/research/#competence-estimation-self-knowledge--failure-detectability)
Open-set recognition, reliable representations under distribution shift. What training objectives reveal about model competence — and when competence estimates are themselves unreliable.

[**Physical systems & robotics →**](/hardware/)
Humanoid design and control, neuro-adaptive optimal control for quadcopters, fault-tolerant system design for safety-critical hardware.

---

## Writing

[Research notes & bets →](/blog/)
