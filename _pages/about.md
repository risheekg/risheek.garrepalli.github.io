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


<!-- ## Current Research Vision

What internal structure enables a model to reason hierarchically, estimate its own competence, and plan controllably, and how much of today's external scaffolding can be built in?

*Current research directions spanning post-training, multimodal world models, and competence-aware reasoning (see [Research](/research/#current-work--open-questions) for a deeper dive):*

- **Closing the Action Loop via Multimodal World Models.** Standard LLMs and VLAs excel at abstract, feedforward task planning but remain open-loop. Conversely, video-prediction models act as feedback controllers that learn continuous, physics-aware priors. The  goal is a unified architectural substrate—leveraging natively multimodal LLMs and video pretraining—to bridge these paradigms, creating a single representation capable of both advanced reasoning and grounded, closed-loop action.
- **Amortizing Test-Time Compute, Harness.** Verifiers, reward models, and MCTS-style planners reveal the control signals that base models miss during next-token prediction. The  question is internalization: what and how to amortize this explicit test-time search, external scaffolding back into the base policy via on-policy distillation and RL.
- **Diffusion as a Substrate for System 2 Planning.** Autoregressive models are constrained by token-by-token commitment. Because discrete diffusion commits over spans (finite-horizon MPC), it natively enables multi-token refinement. The open question is whether this provides a fundamentally better substrate for internal backtracking and hierarchical reasoning than AR models. -->

<!-- ## Current Research Vision

The load-bearing commitment in my work: **covariate shift over latent rollouts is the unifying failure mode across diffusion distillation, VLA post-training, and test-time compute amortization — and it admits a shared correction recipe.**

Concretely: a pretrained model (diffusion, LLM, or world model) induces a latent trajectory at inference. Once training and deployment distributions over those trajectories diverge, the failure is identical across regimes — diversity collapse in distilled samplers, physical unreliability in VLAs, reasoning collapse under on-policy RL. [DDIL](https://arxiv.org/abs/2410.11971) solved one instance of this at scale. The research program is to test whether the same DAgger-style mixed-distribution correction, combined with a supervision schedule gated by *distance from the pretrained manifold*, generalizes to latent rollouts in post-training and embodied control.

*See [Research → Current Work & Open Questions](/research/#current-work--open-questions) for the falsifiable predictions and open questions that follow.*

- **A unified planning template, not a universal representation.** LLMs and world models need not share one latent geometry; they need to share a *planning template*: extract a latent interface from the pretrained model, run a short-horizon rollout under learned latent dynamics, replan on a calibrated uncertainty signal, decode only at the output boundary. The claim worth making — and the one that could be wrong — is that **the same interface properties (predictive sufficiency, intervention-stability, distance-from-pretraining calibration) determine whether this template works in any modality**. I want to know when pretrained hidden states are merely useful for prediction versus *valid under intervention*, a distinction most of the current "latent reasoning" work elides.

- **Competence as a learned object, not a threshold.** External verifiers, reward models, and search scaffolds are a temporary stage. The interesting question is which components can be amortized into the base policy without collapsing diversity — and which cannot, because they provide oversight the model structurally should not internalize. The replan trigger (entropy, value drop, disagreement, observation mismatch) should be learned and calibrated, not hand-scheduled. My grad school work on competence estimation under distribution shift is the prior I bring to this. -->

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