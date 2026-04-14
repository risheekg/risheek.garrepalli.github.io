---
layout: single
title: "Hardware & Physical Systems"
permalink: /hardware/
author_profile: true
---

Before working on learned models, I spent several years doing hardware-first systems work. This background shapes how I think about reliability, failure modes, and the gap between simulation and deployment — problems that remain unresolved in modern physical AI.

---

## Nino: Consumer Humanoid Robot — Sirena Technologies (2014–2016)

Founding engineer on [Nino](https://sirenatech.com/nino/), a consumer humanoid robot developed at Sirena Technologies in Bangalore. Led the complete mechanical and control architecture from concept to walking prototype.

<div style="max-width: 640px; margin: 1.5em 0;">
  <iframe width="100%" height="360" src="https://www.youtube.com/embed/pRwlEzVTfzs" frameborder="0" allowfullscreen></iframe>
</div>

**Mechanical design.** Defined the full kinematic structure: degree-of-freedom allocation, joint placement, and actuator sizing via static load analysis to optimize stability margins. The tradeoffs here — between stability, weight distribution, actuator torque budget, and gait flexibility — are not separable from the control problem. Getting DoF selection wrong means no control algorithm can compensate.

**State estimation.** EKF-based sensor fusion across IMU, force/torque sensors, and encoders. The practical challenge: sensor noise characteristics change with gait phase, and the filter needs to remain reliable across contact events that introduce impulsive dynamics.

**Gait control.** 3D Linear Inverted Pendulum (3D-LIP) model for gait generation and real-time balance control. Hierarchical control stack connecting high-level gait planning to low-level joint impedance control.

**Systems integration.** Owned the hardware-software interface across embedded motor drivers, sensor buses, and high-level planning. The lesson from this work: reliability in physical systems is a design property, not something you can retrofit. It requires reasoning about failure modes before they happen.

---

## Neuro-Adaptive Optimal Control — Quadcopters (IASC)

Research on adaptive control for quadrotors at IASC, where the goal was maintaining stable flight under model uncertainty and external disturbances. Neuro-adaptive methods combine a nominal optimal controller with a neural network that learns and compensates for unmodeled dynamics online.

The interesting tension: the neural component improves performance in the presence of disturbances, but introduces questions about stability guarantees that classical adaptive control theory handles cleanly. This is a microcosm of the broader sim-to-real and distribution-shift problems in learned control.

---

## Satellite Systems — ISRO Collaboration

First introduction to systems engineering and safety-critical hardware design as part of a satellite team collaborating with ISRO. The focus was fault-tree analysis: systematically enumerating failure modes, estimating their probabilities, and designing redundancy structures to meet reliability targets.

**What this taught me that ML research often misses.** In safety-critical systems, reliability is specified *before* you build the system, not measured after deployment. The design process starts with a failure budget and works backwards to component-level requirements. This is almost the opposite of how most ML systems are deployed — trained, shipped, and then evaluated on failures as they occur.

The satellite work established an intuition I've returned to repeatedly: **the relevant question is not just "does this system work?" but "under what conditions does it fail, and how detectably?"** That question is now central to my research on uncertainty and representation.
