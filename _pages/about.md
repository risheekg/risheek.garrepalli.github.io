---
permalink: /
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

I'm a Staff Research Scientist at **Qualcomm AI Research**, working on generative modeling, reinforcement learning, and representation learning. My focus is **representational competence under distribution shift**. Specifically, I study how training objectives (like score matching or next-token prediction) and post-training procedures shape what a model actually captures, where it fails silently, and how detectable those failures are at inference time.

I approach these problems through probabilistic modeling and sequential decision-making, grounded in numerical optimization. This lens connects my work across diffusion distillation, open-set recognition, policy learning, and the broader question of how to guide inference (and learning) toward regions where a model can reliably estimate its own competence.

[**15+ peer-reviewed publications** at JMLR, NeurIPS, ICML, CVPR, and ECCV →](/publications/)

---

## Highlights

**World's first sub-0.6s Stable Diffusion on mobile** — ML lead in a cross-functional team at Qualcomm delivering the world's first and fastest on-device text-to-image generation. Built the training pipeline from scratch, managed 50TB data pipelines via MosaicML MDS, identified the manifold thresholding technique that resolved diversity collapse, modified diffusion sampling for on-target quality, and led debugging across the deployment stack. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html), featured at MWC'23 and Snapdragon Summit.

**Diffusion distillation reframed as imitation learning** — Identified trajectory diversity collapse as a fundamental failure mode in distilled diffusion models. Proposed a DAgger-based on-policy correction (DDIL) that eliminates covariate shift between teacher and student sampling trajectories, preserving marginal distributions at intermediate denoising steps — the insight that drove the sub-0.6s deployment.

**Reliable representations under distribution shift** — Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern) focusing on open-set detection. I studied how to quantify information loss and showed that augmenting discriminative objectives with generative priors yields richer representations that expose more reliable uncertainty and competence signals.

**Hardware-first grounding** — Before ML: fault-tree analysis on a satellite team (ISRO collaboration), then founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot — DoF allocation, static load analysis, EKF state estimation, 3D-LIP gait control, from concept to walking prototype.

---

## Current Research Vision

How do we realize latent representations with multiple implicit levels of hierarchy that are still controllable and carry reliable notions of competence—and when can the external scaffolding that supports them be amortized away?

*Current research directions spanning post-training, multimodal world models, and competence-aware reasoning:*

- **Are world models splitting into two camps?** Token/KV-cache systems give clean planning interfaces; video-diffusion gives richer dynamics but weaker manipulable abstractions. The missing piece seems to be a controllable intermediate with an interface between memory, reasoning, and control.

- **Are external harnesses a stage, rather than a crutch?** Verifiers, judges, and planners reveal what control signal the base model missed; the question is which parts can be amortized into internal representations. SLMs trained with on-policy distillation are the tractable testbed.

- **Diffusion LLMs as a lever.** Diffusion commits jointly over spans (finite-horizon MPC) rather than token-by-token, which may yield smoother trajectories and a cleaner substrate for hierarchical latent structure. Skip-to-Good-Part gives early evidence; the open question is whether those differences are exploitable for controllability—not just efficiency.

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