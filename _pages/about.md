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

**Diffusion Distillation as Imitation Learning** Identified trajectory diversity collapse as a fundamental failure mode in distilled diffusion models. I proposed [DDIL](https://arxiv.org/abs/2410.11971): a DAgger-based framework that trains on the mixed distribution of student-induced states (correcting covariate shift) and teacher-induced states (preserving diversity). Initially applied to progressive distillation (behavior cloning) to drive the world's first sub-0.6s on-device deployment, I subsequently extended DDIL to improve concurrent on-policy methods (like DMD2) for larger models.

**Reliable representations under distribution shift** — Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern) focusing on open-set detection. I studied how to quantify information loss and showed that augmenting discriminative objectives with generative priors yields richer representations that expose more reliable uncertainty and competence signals.

**Hardware-first grounding** — Before ML: fault-tree analysis on a satellite team (ISRO collaboration), then founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot — DoF allocation, static load analysis, EKF state estimation, 3D-LIP gait control, from concept to walking prototype.

---

## Current Research Vision

**The question running through my current research: what internal structure enables a model to reason iteratively, estimate its own competence, and plan controllably:** and how much of today's external scaffolding can be built in? [DDIL](https://arxiv.org/abs/2410.11971) gives one concrete answer for distilled samplers; the forward-looking question is how far that recipe extends across post-training, test-time compute, and embodied control.

Three directions currently organize this work:

- **Controllable abstractions for multimodal world models.** Token/KV-cache systems plan abstractly but remain open-loop (e.g., VLAs); video and latent world models are grounded (e.g., DreamZero) and closed-loop but harder to control. The question is what interface between them supports both reasoning and closed-loop physical action — across embodied, visual, and multimodal settings.

- **Iterative and diffusion substrates for reasoning.** Autoregressive models commit token-by-token; diffusion and looped architectures enable span-level refinement(finite-horizon MPC), and depth-wise parameter sharing. The question is whether this structural difference produces a genuinely better substrate for hierarchical reasoning and controllable computation.

- **Competence estimation as a control primitive.** A useful model should not only act or predict, but also estimate when its own rollout is likely to fail — and use that estimate to decide when to defer, replan, or call external verification, especially under distribution shift.

<!-- - **Amortizing test-time compute and external scaffolding.** Verifiers, reward models, and search expose control signals base models don't fully internalize during pretraining. The interesting question is which components can be amortized into the base policy without collapsing diversity — and which cannot, because they provide oversight the model structurally should not internalize. The replan trigger (entropy, value drop, disagreement, observation mismatch) should be learned and calibrated, not hand-scheduled -->

<!-- This is the throughline connecting my past work on uncertainty and representation learning to current questions in post-training, agentic reasoning, and embodied foundation models. -->

---

## Research Interests

[**Generative modeling & efficient inference →**](/research/#generative-ai--efficient-inference)
Diffusion models, distillation, on-device deployment. The covariate shift problem in few-step samplers. Score-based and flow-based models as probabilistic inference.

[**LLMs, world models & embodied AI →**](/research/#llms-world-models--embodied-ai)
Vision-language-action models, speculative decoding, diffusion policies for manipulation. How planning and uncertainty should interact in long-horizon embodied tasks.

[**Competence estimation & self-knowledge →**](/research/#competence-estimation-self-knowledge--failure-detectability)
Open-set recognition, reliable representations under distribution shift. What training objectives reveal about model competence — and when competence estimates are themselves unreliable.

[**Physical systems & robotics →**](/hardware/)
Humanoid design and control, neuro-adaptive optimal control for quadcopters, fault-tolerant system design for safety-critical hardware.

<!-- --- -->
<!-- 
## Writing

[Research notes & bets →](/blog/) -->