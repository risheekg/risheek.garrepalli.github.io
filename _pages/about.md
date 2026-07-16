---
permalink: /
author_profile: true
title: "About"
redirect_from:
  - /about.html
---

I'm a Researcher at **Qualcomm AI Research**, working on generative modeling, reinforcement learning, and representation learning, including post-training for reasoning models (on-policy distillation, RL fine-tuning), also in context of embodied world models.

One line of work I keep returning to is [DDIL](https://arxiv.org/abs/2410.11971), where I reframed diffusion distillation as imitation learning: the diffusion student is a policy, its denoising trajectory is a rollout, and teacher–student mismatch is a covariate-shift/exposure bias problem. The correction is DAgger-style mixed-distribution training i.e., learn on the student's own induced states while preserving teacher/reference states that maintain diversity. This work was instrumental in **world's first sub-0.6s on-device Stable Diffusion** deployment. The transferable idea is not "faster sampling": it is **amortizing a slow teacher or search process into a faster model without collapsing the diversity of the pretrained prior**, a structure that reappears in reasoning post-training, verifier-guided learning, and embodied world models.

A thread I find especially interesting is **competence as a runtime signal**: a verifier or confidence head cannot recover competence-relevant structure the representation has already discarded. So the leverage tends to be upstream, at the representation, rather than at a better score on top of a fixed one.

[**15+ peer-reviewed publications** at JMLR, NeurIPS, ICML, CVPR, and ECCV →](/publications/)

---

## Highlights

**World's first sub-0.6s Stable Diffusion on mobile** — ML lead in a cross-functional team at Qualcomm delivering the world's first and fastest on-device text-to-image generation. Built the training pipeline from scratch, managed 50TB data pipelines via MosaicML MDS, identified the manifold-thresholding and modified diffusion sampling for  on-target quality, and led debugging across the deployment stack. Covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html), featured at MWC'23 and Snapdragon Summit.

**Diffusion distillation as imitation learning ([DDIL](https://arxiv.org/abs/2410.11971))** — Identified trajectory diversity collapse as a fundamental failure mode in distilled diffusion models and proposed a DAgger-style **on-policy** correction that trains on a mixture of student-induced states (correcting covariate shift) and teacher/reference data (preserving diversity), recovering intermediate marginal distributions. First applied to progressive distillation (behavior cloning) to enable the sub-0.6s on-device deployment, then extended to distribution matching for larger models. 
<!-- The same structure appears in on-policy distillation, verifier-guided reasoning, and self-improvement loops. -->

**Reliable representations under distribution shift** — Graduate research at Oregon State (advisors: Tom Dietterich, Alan Fern) on open-set recognition and failure detectability. I studied what a representation preserves about the data manifold, where competence-relevant information is lost, and why a downstream classifier, confidence head, or verifier cannot recover structure the representation has already discarded. Pairing discriminative objectives with generative priors raised the ceiling on detectable failures.

**Hardware-first grounding** Before ML: satellite fault-tree analysis (ISRO collaboration), then founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot: DoF allocation, static load analysis, EKF state estimation, 3D-LIP gait control, from concept to walking prototype.

---

## Current Research Direction

A loop I'm interested in studying across settings:

1. **Search creates supervision.** A model improves by exploring its own generated computation: rollouts, verifier-filtered samples, self-consistency, process supervision, latent futures — not only by imitating fixed targets.
2. **Distribution-aware correction / on-policy distillation internalizes it without collapse.** Once a model learns from its own behavior, the training distribution becomes endogenous; the model must learn on its induced states while preserving enough reference support to keep diversity, coverage, and recoverability. 
<!-- This is the DDIL lesson, carried into reasoning post-training. -->
<!-- 3. **Competence estimates whether the current representation is sufficient.** Read online, from intermediate states and not just only from final outputs. -->
<!-- 3. **Control uses that estimate to act** — allocate test-time compute, refine, resample, rollback, call a verifier, replan, or abstain. -->

Much of my current attention is on **reasoning**: search amortization, on-policy distillation, verifier-guided learning, and competence-gated test-time compute as because it gives fast, measurable feedback: verifiers, critics, search traces, on-policy rollouts, and clear failure modes such as critic/verifier overfitting and **coverage collapse**. I'm interested not only in whether a recipe improves reward, but in *why and how* i.e., whether it preserves coverage and shifts measurable quantities such as rollout diversity, teacher–student overlap, feature alignment, and credit localization.

<!-- I'm also interested in **multimodal alignment** as a clean instance of the same interface question — visual, linguistic, and action representations live at different granularities, and a one-shot projection into a language model can discard structure needed for later reasoning or control — and in **RL for diffusion** (reward-aligned generation, diffusion policies, and continuous-diffusion / video world models) as a natural extension of the diffusion work. -->

I'm interested in how the same loop carries to **embodied world models**, where it appears over future states via internal world model and action rollouts under physical constraints. Here one key aspect would be **representation, interface and competence-monitoring layer between high-level reasoning and low level control**. DDIL and the reasoning work are the concrete starting points; the multimodal and embodied directions are where I'd like to see the same ideas carry.

[**Research →**](/research/) · [**Publications →**](/publications/)