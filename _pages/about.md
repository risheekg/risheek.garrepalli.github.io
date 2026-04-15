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

**Diffusion distillation reframed as imitation learning** — Identified trajectory diversity collapse as a fundamental failure mode in distilled diffusion models. Proposed DDIL: a DAgger-based correction that trains on the mixed distribution of student-induced states (correcting covariate shift) and teacher-induced states (preserving diversity), recovering marginal distributions at intermediate denoising steps — the insight that drove the sub-0.6s deployment.

**Reliable representations under distribution shift** — Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern) focusing on open-set detection. I studied how to quantify information loss and showed that augmenting discriminative objectives with generative priors yields richer representations that expose more reliable uncertainty and competence signals.

**Hardware-first grounding** — Before ML: fault-tree analysis on a satellite team (ISRO collaboration), then founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot — DoF allocation, static load analysis, EKF state estimation, 3D-LIP gait control, from concept to walking prototype.

---

## Current Research Vision

How do we realize latent representations with multiple implicit levels of hierarchy that are still controllable and carry reliable notions of competence—and when can explicit test-time search, scaffolding be amortized away?

*Current research directions spanning post-training, multimodal world models, and competence-aware reasoning (see [Research](/research/) for a deeper dive):*

- **Closing the Action Loop via Multimodal World Models.** Standard LLMs and VLAs excel at abstract, feedforward task planning but remain open-loop. Conversely, video-prediction models act as feedback controllers that learn continuous, physics-aware priors. The  goal is a unified architectural substrate—leveraging natively multimodal LLMs and video pretraining—to bridge these paradigms, creating a single representation capable of both advanced reasoning and grounded, closed-loop action.
- **Amortizing Test-Time Compute, Harness.** Verifiers, reward models, and MCTS-style planners reveal the control signals that base models miss during next-token prediction. The frontier question is internalization: what and how to amortize this explicit test-time search back into the base policy via on-policy distillation and RL.
- **Diffusion as a Substrate for System 2 Planning.** Autoregressive models are constrained by token-by-token commitment. Because discrete diffusion commits over spans (finite-horizon MPC), it natively enables multi-token refinement. The open question is whether this provides a fundamentally better substrate for internal backtracking and hierarchical reasoning than AR models.

## Research Interests

[**Generative modeling & efficient inference →**](/research/#generative-ai--efficient-inference)
Diffusion models, distillation, on-device deployment. The covariate shift problem in few-step samplers. Score-based and flow-based models as probabilistic inference.

[**LLMs, world models & embodied AI →**](/research/#llms-world-models--embodied-ai)
Vision-language-action models, speculative decoding, diffusion policies for manipulation. How planning and uncertainty should interact in long-horizon embodied tasks.

[**Competence estimation & self-knowledge →**](/research/#competence-estimation-self-knowledge--failure-detectability)
Open-set recognition, reliable representations under distribution shift. What training objectives reveal about model competence — and when competence estimates are themselves unreliable.

[**Physical systems & robotics →**](/hardware/)
Humanoid design and control, neuro-adaptive optimal control for quadcopters, fault-tolerant system design for safety-critical hardware.

---

## Writing

[Research notes & bets →](/blog/)