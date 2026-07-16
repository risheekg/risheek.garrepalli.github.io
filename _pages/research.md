---
layout: single
title: "Research"
permalink: /research/
author_profile: true
---

The question running through my research is: **how should models learn from their own generated computation (rollouts, search traces, latent futures) while preserving distributional coverage and knowing when their internal representation is sufficient for the decision at hand?** Underneath it is a formulation question. The choice of training formulation shapes what a model can represent, where it fails, and how detectable those failures are. I think of this as representational/model competence under distribution shift.

The common object across these settings is a single loop: acquire feedback through search and exploration (deciding *where* to query), assess its quality by critique and filtering, then amortize it back into the base model.

1. **Search creates supervision.** Generated computation (rollouts, verifiers, self-consistency, process supervision, latent futures) produces structure worth internalizing.
2. **Distribution-aware correction, or OPD, internalizes it without collapse.** The model learns on its own induced states while preserving the diversity and coverage of the pretrained or reference distribution.

These questions show up in diffusion distillation, reasoning post-training, and embodied world models. I treat them as different instantiations of the same loop rather than separate topics, connecting my work on diffusion distillation, open-set recognition, efficient deployment, multimodal representation, and embodied control.

---

## Generative AI & Efficient Inference

**World's first on-device generative AI.** I was ML lead in the cross-functional Qualcomm team that shipped the world's first and fastest on-device text-to-image generation: sub-0.6s Stable Diffusion on Snapdragon hardware. I built the training pipeline, managed 50TB data pipelines via MosaicML MDS, worked through progressive distillation and W4A8 quantization constraints, identified the manifold-thresholding technique that resolved diversity collapse, and modified diffusion sampling for on-target quality. Featured at MWC'23 and Snapdragon Summit and covered by [The Verge](https://www.theverge.com/2023/2/23/23611668/ai-image-stable-diffusion-mobile-android-qualcomm-fastest) and [Engadget](https://www.engadget.com/qualcomm-brings-on-device-ai-to-mobile-and-pc-190030938.html).

<div style="max-width: 340px; margin: 1.5em 0;">
  <iframe width="340" height="604" src="https://www.youtube.com/embed/64VsQHhImQI" frameborder="0" allowfullscreen></iframe>
</div>

**Diffusion distillation as learning from rollouts.** Standard diffusion distillation trains a student on teacher outputs at fixed steps, but at inference the student conditions on its *own* previous outputs. That creates a trajectory mismatch: errors compound across denoising steps, intermediate marginals drift, and diversity collapses. I treated this as a sequential decision-making problem, where the student is a policy, its denoising path is a rollout, and teacher/student mismatch is the standard covariate-shift failure mode from imitation learning.

<!-- The fix is DAgger-style correction ([DDIL](https://arxiv.org/abs/2410.11971)), which trains on the mixed distribution of student-induced and teacher-induced latent states. I first introduced DDIL to progressive distillation (behavior cloning), resolving co-variate shift/exposure bias and enabling the world's first sub-0.6s Stable Diffusion mobile deployment. 

Subsequently, I extended DDIL to __on-policy distillation__. While concurrent methods like DMD2 address on-policy training, they still struggle to preserve diversity. By leveraging a DAgger framework to train on the teacher-induced distribution too, DDIL preserves intermediate marginal distributions. This approach provides an effective mechanism for amortizing test-time search in LLM post-training.

The same correction generalizes beyond samplers. On-policy distillation, RL fine-tuning (including verifiable-reward / RLVR setups), and verifier routing all train a model on its *own* rollout distribution — so the covariate-shift framing applies directly to **multi-step reasoning post-training**. On a strong *native* diffusion substrate the open problems are concrete: RL **stability** (gradient/score variance under sparse or partial reward) and **credit assignment** across refinement steps. I treat a feature-space training signal as the lever for both — lower-variance, better-localized credit than token-likelihood reweighting — pre-registered against a matched autoregressive control and a task-reward non-inferiority guard. -->

The fix is DAgger-style mixed-distribution correction ([DDIL](https://arxiv.org/abs/2410.11971)): train on student-induced states to correct the distribution the student actually visits, while mixing in teacher and reference states to preserve diversity and coverage. I first applied this to progressive distillation, which helped enable the on-device deployment above, and later extended the framing to on-policy distillation, where preserving intermediate marginal distributions remains the central challenge and concurrent methods (e.g., DMD2) tend to lose diversity.

**From efficient sampling to search amortization.** The transferable idea in DDIL is not "make diffusion faster." It is **amortizing a slow teacher, search process, or scaffold into a faster model without collapsing the distribution that made the scaffold useful**. That is the same structural problem that appears in reasoning post-training, where verifier-filtered samples, process supervision, self-consistency, tree and search traces, and on-policy rollouts are all generated computation that can become supervision.

**Efficiency as retention, beyond throughput.** Deployment makes efficiency concrete, but the research metric I care about is *retention*: how much useful trajectory behavior (refinement, self-correction, diversity, recoverability) can survive at a fixed compute budget? Compressing an iterative model changes the states it visits, which is itself a new distribution shift.
<!-- , so the correction is again trajectory-aware. -->

**RL for continuous diffusion.** I'm also interested in how the DDIL and DAgger machinery transfers to reward-aligned continuous generation (diffusion-RL over the denoising trajectory), diffusion policies for continuous-action manipulation, and continuous-diffusion and video world models as substrate for downstream policy and value learning. This is a natural extension of the diffusion work toward the reasoning- and control-side directions below.

---

## LLMs, World Models & Embodied AI

<!-- **Search creates supervision; distribution-aware correction turns it into model capability without collapse; competence estimates when that capability applies; control uses that estimate to reason, verify, replan, or act reliably under shift.** -->

In agentic AI, across both language and embodied systems, the same question becomes operational: when search produces better behavior, how do we turn it into model capability while preserving coverage and knowing when that capability can be trusted?

**Post-training.** A model samples candidate solutions, a verifier or process signal selects or scores them, and post-training pushes the model toward the induced distribution of successful traces. This creates the same risks as diffusion distillation: exposure bias (covariate shift), verifier overfitting, reward hacking, and coverage collapse.

This is where much of my current attention sits: **search amortization and on-policy correction for reasoning**, training on self-generated rollouts while preserving the diversity and robustness of the base model, and studying whether the model has *internalized useful search* rather than merely imitating scaffold artifacts. I'm interested not only in whether a recipe improves reward, but in the **mechanism** behind it: how distribution-aware correction shifts training-signal variance, teacher/student overlap, feature alignment, credit localization, verifier dependence, and coverage, evaluated against matched controls across the **design space of OPD, RL, and the post-training toolkit**.



**Embodied world models.** 
<!-- Vision-language-action systems are strong at language-conditioned task planning but are often close to open-loop systems; video, diffusion (WAMs) and latent world models (JEPA) provide richer future-state and continuous-control priors 
but need mechanisms for correction, monitoring, and replanning. I'm interested in how the same loop applies over action and latent-future rollouts: search over possible futures, correct the model on its own induced states, preserve coverage, estimate competence, and use that estimate to replan, query a simulator, switch controllers, or defer within integrated learning and search framework.
**The key open question is the interface i.e., is the representation, rollout-correction, and competence-monitoring layer between high-level reasoning and closed-loop control** Reasoning post-training is a natural place to develop the same loop with faster feedback and clearer measurement. -->
<!-- The TAMP framework illuminates a current divide: natively multimodal token/KV-cache systems (VLAs) excel as model-free, feedforward task planners with strong LLM-native reasoning but remain open-loop; video-prediction and diffusion world models act as model-based feedback controllers, learning physics-aware priors that feedforward reasoning structurally lacks. Within the model-based world, JEPA-style latent prediction offers rich semantic structure efficiently, while video/diffusion world models capture richer physical dynamics with a clearer path to extreme-scale pretraining (Veo-class). The question is what controllable intermediate bridges these paradigms at scale towards a unified substrate across memory, reasoning, and action. -->
In embodied AI, the same loop moves from reasoning traces to action and latent-future rollouts. Vision-language-action systems are strong at language-conditioned task planning but are largely open-loop; Where as video-diffusion(WAMs), and latent world models offer richer future-state and continuous-control generative priors, but need mechanisms for correction, monitoring, and replanning. The TAMP framework names the divide: VLAs are model-free, feedforward planners with strong LLM-native **agentic** reasoning; video and diffusion world models are model-based feedback controllers with physics-aware priors that feedforward reasoning structurally lacks.
<!-- JEPA-style latent prediction offers efficient semantic structure; video/diffusion world models capture richer dynamics with a clearer path to extreme-scale pretraining (Veo-class). -->



Two questions follow. **(1) Search:** what controllable intermediate bridges these paradigms at scale into a unified substrate across memory, reasoning, and action? **(2) Competence:** what is the interface, the representation, rollout-correction, and competence-monitoring layer between high-level reasoning and closed-loop control, that applies the same search, correct, estimate-competence, and control/feedback loop of the LLM post-training toolkit to action and latent-future rollouts, deciding when to replan, query a simulator, switch controllers, or defer? Both matter for the same reason: search finds the behavior, but without a competence-aware interface to gate it, neither language nor embodied systems can tell when that behavior is trustworthy. Reasoning post-training remains the faster-feedback place to develop both questions before they have to survive contact with a physical system.

**Substrates and controllable abstraction.**

Autoregressive, diffusion, recursive, and world models impose different commitment structures on intermediate computation. <!-- I do not treat this as a blanket claim that one substrate is superior.  -->The useful question is: **which representation are we training and correcting over, and does that representation support intervention, refinement, and competence estimation?** Work such as [Skip-to-Good-Part](https://arxiv.org/abs/2603.07475) gives evidence that diffusion and autoregressive models organize intermediate representations differently; I view that as a *lever* for studying controllable abstraction.

<!-- **Multimodal abstraction interfaces.** A clean isolable instance of the interface question I'm interested in. Current VLMs often treat alignment as a single-stage projection into language-embedding space, with the LM residual stream implicitly carrying integration, reasoning, and gradient flow. The substrate issue is that vision is spatial, dense, and hierarchical while text is compressed, sequential, and semantic — and a one-shot projection can discard structure needed for later reasoning or control. I am interested in **multi-stage, bidirectional alignment** with matched-parameter and single-pass controls (so the experiment isolates structured aggregation from raw capacity), competence-instrumented (intrinsic dimension, CKA, cross-modal information loss) so the question is about *when* the cross-modal abstraction is sufficient, not only whether benchmarks improve. The aggregated representation that comes out of this is the perception latent any downstream world model runs on. -->

<!-- **Distributed training at scale.** The post-training and distillation work above runs on multi-node infrastructure I have architected and used for billion-parameter diffusion and foundation models — FSDP/ZeRO-3, model sharding, streaming dataloaders, large-scale data pipelines, deployment-aware debugging. This is not the research identity, but it matters because the loop has to be executable end-to-end. -->

---

## Competence Estimation, Self-Knowledge & Failure Detectability

**What a representation preserves bounds what any monitor can know.** My graduate work at Oregon State (advisors: Tom Dietterich, Alan Fern) studied open-set recognition and failure detectability under distribution shift. The core lesson: a downstream classifier, confidence head, verifier, or selective-prediction rule **cannot recover competence-relevant information the representation has already discarded.** The leverage is upstream, in the training objective and representation, not only in a score attached afterward.

**Open-set detection and reliable representations.** I studied which representation structures (contrastive, ensemble, flow-based, VAE-based) make unknown-class inputs detectable and where they break down. Pairing discriminative objectives with generative priors raised the ceiling on detectability because it preserved more of the data manifold. That principle now carries into reasoning and planning: a process reward model, verifier, or monitor is only as good as the intermediate state it reads from.

**Risk-sensitive RL and constrained MDPs.** A concrete instance of competence as a gate: incorporating uncertainty into policy learning through constraint-based methods that penalize acting where the world model is unreliable. This is the policy-learning face of knowing when *not* to trust a rollout.

**Online sufficiency.** A question I'm interested in is whether competence can be assessed **online**, from intermediate computation (hidden states, partial reasoning traces, rollout latents, feature trajectories, cross-modal abstractions) early enough to defer, replan, resample, or abstain before the answer or action fails. The same idea instantiates differently across domains: an unsupervised divergence monitor over reasoning-trace hidden states (test-time-compute control); competence-routed depth and compute in cross-modal alignment; supervised multi-source mismatch gating search, safety, and simulator queries in embodied rollouts. The connecting thread is one: **competence is upstream of the head, and online sufficiency is the runtime instrument that closes the loop.**

<!-- ---

## How the Pieces Fit

The directions above are grounded in work I have already done: -->

<!-- - **DDIL** showed that generative trajectories can be treated as rollouts and corrected with imitation-learning tools, without collapsing diversity.
- **The sub-0.6s Stable Diffusion deployment** showed the formulation survives real systems constraints.
- **Open-set recognition** showed that competence is bounded by what the representation preserves.
- **Hardware and robotics** grounded the same ideas in state estimation, reliability, and closed-loop control.

I'm interested in carrying this loop across reasoning post-training, multimodal interfaces, and embodied world models — not as separate pivots, but as different instantiations of the same question:

> **Search creates supervision; distribution-aware correction turns it into model capability without collapse; competence estimates when that capability applies; control uses that estimate to reason, verify, replan, or act reliably under shift.**

--- -->

*Last updated: June 2026*