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

<!-- ---
## Current Work & Open Questions

**Information organization, routing, and feedback.** The organizing question across these directions: how should information be structured in representations to support reliable routing for reasoning, iterative Bayesian-style planning updates, and feedback-aware memory compression? Competence estimation — knowing when a representation's organization is trustworthy enough to act on — is the grounding check throughout. On-policy distillation, verifier routing, and diffusion LLMs give concrete handles on each of these dimensions that didn't exist before.

**Closing the Action Loop: Unifying Task Planners and Continuous World Models.** The classic Task and Motion Planning (TAMP) framework informs the current architectural divide in foundational simulators. On one side, natively multimodal token/KV-cache systems act as model-free, feed-forward task planners (VLAs) that excel at LLM-native agentic reasoning but remain open-loop. On the other side, explicit latent models (JEPA) and video-prediction systems (diffusion) act as model-based, feedback controllers for motion planning. **The objective is a controllable intermediate: a unified substrate across memory, reasoning, and action.** Scaling continuous, physics-aware world models isn't just about motion execution—it is the path to learning foundational physical priors that pure text-based reasoning lacks. Natively multimodal architectures with shared KV-cache (BAGEL, Transfusion, Gemma, Qwen3.5) are converging on this from one direction, while video generation approaches it from the other.

**Diffusion and looped transformers as substrates for System 2 planning.** Autoregressive models dominate via scaling laws, but their fundamental constraint is token-by-token commitment, requiring external test-time compute (CoT) to plan or backtrack. Discrete diffusion commits over spans (finite-horizon MPC), natively enabling multi-token refinement — yielding smoother internal states and a cleaner substrate for hierarchical latent structure, and the mechanism motivating [*Skip-to-Good-Part*](https://arxiv.org/abs/2603.07475). Looped transformers and mixture-of-recursion architectures add a complementary axis: iterative refinement via depth-wise parameter sharing, where passes adapt to difficulty — concentrating compute across reasoning depth, not just position. Together, span-level commitment and adaptive depth form a unified design space for controllable internal computation. The open question is whether looped-diffusion hybrid architectures deepen the representational geometry gap further, and whether these structural advantages in controllability and efficiency compound with scale.

**Amortizing Harness, Search and Test-Time Compute.** Verifiers, reward models, and MCTS-style planners reveal the control signals that base models miss during next-token prediction. The question is internalization: what part of this explicit test-time search and scaffolding should be amortized back into the base policy. I view on-policy distillation and RL-driven reasoning not just as alignment, but as the mechanism to force representations to become inherently hierarchically structured and self-sufficient as the external harness is amortized away.

## Current Work & Open Questions

**Organizing thesis.** Covariate shift over latent rollouts is a single failure mode that recurs under different names across diffusion distillation (diversity collapse), VLA post-training (physical unreliability), and on-policy LLM fine-tuning (reasoning mode collapse). [DDIL](https://arxiv.org/abs/2410.11971) corrected it in one setting by training on the mixed distribution of student-induced and teacher-induced latent states. The research program here asks whether the same correction — combined with a supervision schedule gated by distance from the pretrained manifold, and a learned competence signal as the replan trigger — generalizes to a shared planning template for LLMs and world models.

**Positioning.** The pipeline (extract a latent interface → learn latent dynamics → goal-condition → short-horizon rollout → competence-triggered replan → decode only at the output boundary) shares structural elements with Dreamer, JEPA/H-JEPA, MuZero, and the Coconut / Quiet-STaR latent-reasoning line. What I claim as distinct are three specific technical commitments that, together, are not in those programs:

1. **DAgger-style mixed-distribution training *over latent rollouts*** (not over tokens or pixels), directly inheriting DDIL's correction of imitation-learning failure at the rollout level.
2. **A supervision-density schedule inversely tied to distance from the pretrained manifold**: weakest near pretraining-proximal states, strongest near terminal outcomes.
3. **A learned, calibrated competence signal as the replan trigger**, treating the replan condition itself as a trainable object rather than a threshold hyperparameter.

If any of these three is wrong, the program has to restructure. That is intentional.

---

**1. Closing the Action Loop: a shared planning template for LLMs and world models.**
Token/KV-cache systems plan abstractly but are open-loop. Video-diffusion and latent world models propagate rich physical dynamics but offer weaker manipulable abstractions. The claim is not that one latent geometry unifies them, but that the *same interface properties* — predictive sufficiency, intervention-stability, distance-from-pretraining calibration — determine whether the planning template works in either regime.

The critical distinction is between representations that are useful for *prediction* and representations that remain valid under *intervention*. Most current "latent reasoning" work conflates these. I take the distinction from causal representation learning (Schölkopf, Locatello, von Kügelgen) and want to make it operational for pretrained models.

Falsifiable predictions I am committed to:
- On VLA benchmarks with non-stationary disturbances, rollouts in a multi-layer "interface" extracted from the pretrained encoder will outperform rollouts over final-layer embeddings at matched compute. If they do not, the "multi-layer sufficient features" thesis is weaker than I think.
- Latents selected for intervention-stability (via a counterfactual-rollout diagnostic) will replan more reliably than latents selected by predictive accuracy alone, even when the latter have lower open-loop rollout error.

Open questions:
- What is the right operational diagnostic for intervention-stability of a pretrained latent — counterfactual consistency, equivariance under controlled perturbations, or a learned stability probe?
- Can language goals, scalar rewards, and subgoal latents condition the *same* short-horizon dynamics model without collapsing the pretrained prior, or does each goal modality require its own adapter?
- How far can latent rollout extend before drift off the pretrained manifold makes the plan unreliable, and can that horizon be predicted from model internals rather than measured post-hoc?


**2. Iterative vs. autoregressive commitment as a controllability question.**
AR decoding commits token by token; diffusion and looped transformers refine over spans or repeated latent passes. I treat this as a claim about representational geometry and controllable computation, not a blanket efficiency claim. [*Skip-to-Good-Part*](https://arxiv.org/abs/2603.07475), [*SPIFFY*](https://arxiv.org/abs/2509.18085), and [*ConFu*](https://arxiv.org/abs/2603.08899) are my entry points on the empirical side.

Open questions with stake:
- Do iterative models produce latent states that are more intervention-stable than AR models at comparable scale, or is the apparent geometry difference primarily a training-objective artifact?
- Are weight-tied / equivariant latent dynamics better suited to short-horizon rollout than untied step-specific predictors? The hypothesis is yes, by analogy to denoising diffusion's shared score network, but this has not been tested cleanly in post-training.
- When does internal iterative computation beat explicit output-space search (best-of-N, beam, verifier rerank) at matched compute? This is the quantitative version of the "decode only at the output boundary" principle.
- Does the representational geometry gap between iterative and AR families widen or attenuate with scale?

---

**3. Amortizing external harnesses.**
Verifiers, reward models, search, and tool use expose control signals that pretraining does not reliably internalize. The research question is which components *should* be amortized into the base policy or latent dynamics model, and which should remain external because they provide robustness, modularity, or oversight that the model structurally should not internalize.

Open questions with stake:
- What should be amortized — selection policies, value functions, latent search dynamics — and in what order?
- Under what conditions does on-policy distillation improve the base representation versus shallowly imitate the harness on the benchmark distribution? A clean diagnostic is held-out transfer to tasks the harness was not applied to.
- Can a model learn a calibrated competence signal — not a confidence threshold, but a *trained predictor* of when its own rollout will fail — well enough to decide when to defer to external search?
- What empirical evidence would show that a scaffold has been genuinely internalized rather than behaviorally mimicked? This is the harder question and the one I think the field is currently evading.

---

These threads share one concern: building models whose internal structure is rich enough to support planning and self-monitoring, while being honest about which external scaffolding is load-bearing. -->
