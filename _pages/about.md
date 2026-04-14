---
permalink: /
author_profile: true
redirect_from:
  - /about/
  - /about.html
---

I'm a Staff Research Scientist at **Qualcomm AI Research**, working at the intersection of generative modeling, reinforcement learning, and representation learning. My focus is **representational competence under distribution shift**: what a model captures, where it fails silently, and how training formulations — score matching, next-token prediction, on-policy distillation — shape what's recoverable at inference and how detectable the semantic gaps are.

I approach problems through probabilistic modeling and RL, grounded in numerical optimization and occasionally dynamical systems and theory as a diagnostic tool. This lens runs across my work on diffusion models for vision and language, open-set detection, covariate shift in distilled samplers, policy learning, and how to guide sampling toward regions the model can actually reason about.

**20+ publications at NeurIPS / ICML / CVPR / ECCV / ICLR / JMLR.**

---

## Highlights

**World's first sub-0.6s Stable Diffusion on mobile** — Led a cross-functional team at Qualcomm delivering the world's first and fastest on-device text-to-image generation. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html), featured at MWC'23 and Snapdragon Summit.

**Diffusion distillation from first principles** — Identified trajectory diversity collapse as a fundamental failure mode in distilled diffusion models. Proposed a DAgger-based fix (DDIL) that eliminates covariate shift between teacher and student sampling trajectories, preserving marginal distributions at intermediate denoising steps. A sequential decision-making framing of a problem the field had been treating as pure regression.

**Reliable representations under distribution shift** — Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern). The inability to handle "unknown-unknowns" natively during learning is a fundamental flaw we currently patch with external harnesses. My work focused on what a learned representation fundamentally captures and how to quantify its information loss. I demonstrated that augmenting discriminative objectives with generative priors forces models to retain richer structural information, yielding representations that naturally bound their own competence and expose reliable uncertainty signals.

**Hardware-first grounding** — Before ML: fault-tree analysis on a satellite team (ISRO collaboration), then founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot — DoF allocation, static load analysis, EKF state estimation, 3D-LIP gait control, from concept to walking prototype.

---

## Current Research Vision

How do we realize latent representations with multiple implicit levels of hierarchy that are still controllable and carry reliable notions of competence—and when can the external scaffolding that supports them be amortized away?

*Shared modeling questions for LLMs, vision-grounded world models, and reasoning:*

- __Are world models splitting into two camps, and is the opportunity in the middle?__ Token/KV-cache systems give clean planning interfaces and persistent state; video-diffusion models (without KV-cache) give richer implicit dynamics but weaker manipulable abstractions. Neither wins alone. The missing piece is a controllable intermediate: persistent summaries plus local detail, and a physics-aware planning interface and **feedback policy (e.g., DreamZero compared to VLAs)** that still exploits video-scale dynamics. The bottleneck is the interface between memory, reasoning, control, and scalability.

- __Are external harnesses a stage, rather than a permanent crutch?__ Verifiers, judges, teachers, and planners reveal what control signal the base model was missing. The scientifically interesting question is which parts can be amortized into the model's internal representation over time, and how we know when they are still necessary. Models that know *when* they need external help, and learn from it, are the right target. Scaffolding helps manage the __unknown-unknowns__ of the model's own capabilities.

- __Representation, competence, and memory are one problem.__ "Latent reasoning," "KV compression," "visual memory, CoT," and "external verifiers" are currently treated as separate problems. In reality, they are competing answers to the same underlying question: *what state should persist, and in what form?* The next capability gains will come from shaping internal structures that jointly support competence estimation, memory compression, and multi-level controllable latent states with the selective use of scaffolds—not from treating these as independent engineering concerns.

- __Diffusion LLMs as a lever.__ Multi-token atomic generation shifts both the planning horizon and decision DoF. Diffusion commits jointly over spans rather than sequentially token by token, which may yield smoother trajectories and more compressible internal states.

## Research Interests

[**Generative modeling & efficient inference →**](/research/#generative-ai--efficient-inference)
Diffusion models, distillation, on-device deployment. The covariate shift problem in few-step samplers. Score-based and flow-based models as probabilistic inference.

[**Foundation models, VLAs & embodied AI →**](/research/#foundation-models--embodied-ai)
Vision-language-action models, speculative decoding, diffusion policies for manipulation. How planning and uncertainty should interact in long-horizon embodied tasks.

[**Uncertainty, representation & safety →**](/research/#uncertainty-representation--safety)
Open-set recognition, calibration, reliable representations under distribution shift. What training objectives reveal about model competence — and when competence estimates are themselves unreliable.

[**Physical systems & robotics →**](/hardware/)
Humanoid design and control, neuro-adaptive optimal control for quadcopters, fault-tolerant system design for safety-critical hardware.

---

## Writing

[Research notes & bets →](/blog/)
