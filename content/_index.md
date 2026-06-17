+++
title = "Flexible Distributed Particle Filtering for the Internet of Things via Aggregate Computing"
description = "DCOSS-IoT 2026 presentation"
outputs = ["Reveal"]
+++

# Flexible Distributed Particle Filtering for the Internet of Things via Aggregate Computing

[**<span style="color: #bb2e29;">Angela Cortecchia</span>**](mailto:angela.cortecchia@unibo.it), 
[Davide Domini](mailto:davide.domini@unibo.it),
[Giovanni Ciatto](mailto:giovanni.ciatto@unibo.it),
[Roberto Casadei](mailto:roby.casadei@unibo.it),
[Danilo Pianini](mailto:danilo.pianini@unibo.it),
and
[Mirko Viroli](mailto:mirko.viroli@unibo.it)


Department of Computer Science and Engineering (DISI)<br>
Alma Mater Studiorum -- University of Bologna - Cesena, Italy

<div style="text-align: center; width: 100%; margin-top: 1rem;">
  <img src="./images/DIP INFORMATICA-SCIENZA E INGEGNERIA_DISI_EN.svg" style="width: 38%;">
</div>

---

# Application

{{% multicol %}}
{{% col class="text-start col-md-4" %}}

Estimating hidden state of dynamic systems from partial, noisy observations

Many cyber-physical systems estimates **hidden dynamical states over time**.

Such as target tracking and environmental monitoring applications.

### Constraint

The state is **not directly observable**.

We only receive noisy, partial, and spatially distributed measurements.

{{% /col %}}
{{% col %}}

![Target tracking example](./images/Gemini_Generated_Image_y0tp7fy0tp7fy0tp.jpg)

{{% /col %}}
{{% /multicol %}}

---

# Problem Statement

Core problem: **reconstruct latent dynamics from uncertain observations**.

Classical linear estimation techniques are not suitable: the system of interest exhibits **non-linear dynamics** and **non-Gaussian uncertainty**.

Therefore, we need an estimator that does not force the posterior into a simple closed-form shape, but can maintain **multiple uncertain hypotheses over time**.

---

# The Filtering Problem

We want to estimate the hidden state of a dynamic system over time.

{{% multicol %}}
{{% col %}}

### State-space view

At each time step:

$$
x_t = f(x_{t-1}, u_t)
$$

$$
y_t = h(x_t, v_t)
$$

where:

- $x_t$ is the hidden system state
- $y_t$ is the observation
- $u_t$ is process noise
- $v_t$ is measurement noise

{{% /col %}}
{{% col %}}

### Filtering objective

Given all observations so far:

$$
y_{1:t} = y_1, \dots, y_t
$$

estimate the posterior belief:

$$
p(x_t \mid y_{1:t})
$$

This belief is updated recursively as new observations arrive.

{{% /col %}}
{{% /multicol %}}

---

# Particle Filters (PF)

A practical way to track hidden state under nonlinear and non-Gaussian uncertainty

{{% multicol %}}
{{% col %}}

### Key idea
Instead of representing belief with a single estimate, a particle filter represents it with many possible hypotheses:

- each **particle** is one possible state of the system
- each **weight** says how plausible that hypothesis is
- as new observations arrive, unlikely particles fade out
- likely particles are reinforced over time

{{% /col %}}
{{% col %}}

### Why useful here
- Works with nonlinear dynamics
- Handles non-Gaussian and multimodal uncertainty
- Supports sequential estimation over time
- Naturally fits tracking and monitoring problems

{{% /col %}}
{{% /multicol %}}

Transition: **we move from “what is the hidden state?” to “how do we maintain a cloud of possible states as observations arrive?”**

---

# (Centralized) Particle Filters

Monte Carlo state estimation for nonlinear and non-Gaussian systems

{{% multicol %}}
{{% col %}}

### Posterior approximation
We estimate a hidden state $x_t$ from noisy observations $y_{1:t}$ by representing the posterior with weighted samples:

$p(x_t \mid y_{1:t}) \approx \sum_i w_t^i \delta(x_t - x_t^i)$

{{% /col %}}
{{% col %}}

### One filtering iteration
- **Prediction:** Propagate each particle through the dynamical model.
- **Weighting:** Evaluate each particle against the new observation.
- **Resampling:** Keep plausible hypotheses and discard weak ones.
- **Estimate:** Compute the state estimate from the weighted particles.

{{% /col %}}
{{% /multicol %}}

In distributed settings, this machinery remains the same; the hard part becomes information coordination.

---

# Distributed Particle Filters

{{% multicol %}}
{{% col %}}

### Motivation

Filtering becomes a coordination problem

In many networked systems, observations are collected by **multiple spatially distributed agents**.

A centralized particle filter assumes all measurements are available in one place.

{{% /col %}}
{{% col %}}

### Why this is unrealistic
- Limited bandwidth
- Energy constraints
- Latency
- Topology changes
- Node and link failures

{{% /col %}}
{{% /multicol %}}

Main design question: **how do we distribute filtering without losing too much estimation quality?**

---

# Different DPF Architectures

{{% multicol %}}
{{% col %}}

### Fusion center
Local agents process measurements, then send local results to a central node for fusion.

### Leader-agent based
A selected path or subset of nodes accumulates information and performs global estimation.

### Consensus-based
All agents iteratively exchange information to obtain consistent local beliefs.

Main trade-off: **estimation quality vs communication burden vs robustness**.

{{% /col %}}
{{% col %}}

![DPF taxonomy](./images/pfs.png)

{{%/ col %}}
{{%/ multicol %}}

---

# Fusion Center-Based DPF

{{% multicol %}}
{{% col %}}

### Architecture
Each agent processes its own measurements locally. Local posteriors or local estimates are reported to a **fusion center**, which combines the received information and computes the global estimate.

{{% /col %}}
{{% col %}}

### Why attractive
- Conceptually simple
- Easy to organize
- Global decision available in one place

{{% /col %}}
{{% col %}}

### Typical use case
Applications where global knowledge is needed only at one node.

{{%/ col %}}
{{%/ multicol %}} 


{{% multicol %}}
{{% col %}}

### Advantages
- Simple architecture
- Easy fusion logic
- Often good estimation quality
- Natural with a central decision-maker

{{% /col %}}
{{% col %}}

### Drawbacks
- Single point of failure
- Poor robustness against fusion-center faults
- High communication burden near the center
- Strong dependence on network topology

{{% /col %}}
{{% /multicol %}}

---

# Leader-Agent Based DPF

{{% multicol %}}
{{% col %}}

### Key idea
No fixed fusion center is assumed. Instead, only a subset of agents is activated for global estimation, forming a **leader-agent path** along which information is accumulated and propagated.

{{% /col %}}
{{% col %}}

### Design intuition
Global estimation is not done by everyone: it is delegated to selected nodes, dynamically chosen in the network.

{{% /col %}}
{{% /multicol %}}


{{% multicol %}}
{{% col %}}

### Advantages
- Avoids a permanently fixed central node
- Can reduce communication compared with fully decentralized schemes
- May exploit informative nodes more efficiently

{{% /col %}}
{{% col %}}

### Drawbacks
- Performance depends on leader selection
- Accuracy and energy consumption are coupled to the selection policy
- Coordination overhead remains
- Leadership can be fragile
  
{{% /col %}} 
{{% /multicol %}}

---

# Consensus-Based DPF

{{% multicol %}}
{{% col %}}

### Architecture
All agents participate in the filtering process. No fusion center and no privileged node are required.

{{% /col %}}
{{% col %}}

### High-level intuition
- **Local beliefs:** Each node starts from local information.
- **Exchange:** Neighbors communicate iteratively.
- **Propagation:** Information spreads through the network.
- **Consistency:** Estimates become more globally informed.

{{% /col %}}
{{% /multicol %}}

{{% multicol %}}
{{% col %}}

### Advantages
- High robustness to node failures
- Enhanced scalability
- Better adaptability to topology changes
- No single point of failure

{{% /col %}}
{{% col %}}

### Drawbacks
- Heavier communication demand
- Possible delay due to consensus iterations
- Synchronization and consistency can become nontrivial
- Agreement may not be the real objective

{{% /col %}}
{{% /multicol %}}

---

# What Makes DPF Hard in Practice?

### 01. Particles and communication are costly
Particle filters require many particles, and exchanging particles, weights, or likelihoods can dominate the method.

### 02. Networks are imperfect
Communication delays, topology changes, asynchrony, and failures affect estimation quality.

### 03. Trade-offs dominate
DPF is always a compromise among **accuracy**, **overhead**, **complexity**, and **robustness**.

---

# What Should We Be Careful About?

### Cost and load
- Communication cost
- Load imbalance
- Particle, weight, or likelihood exchange may dominate the whole method

### Network behavior
- Topology sensitivity
- Delay and asynchrony
- Node failures

### Robustness
- Special nodes introduce fragility
- Failure handling must be part of the design

### Information granularity
Raw measurements, local posteriors, particles, and likelihood summaries have very different costs.

---

## How Can a Field-Based Perspective Help DPF?

### Main point
The goal is **not** to introduce yet another DPF algorithm.

The idea is to expose DPF as a **configurable coordination problem**.

{{% multicol %}}
{{% col %}}

### Filtering logic
- Prediction
- Weighting
- Resampling

{{% /col %}}
{{% col %}}

### Coordination choices
- Where fusion happens
- What is exchanged
- How far information propagates
- Who is active

{{% /col %}}
{{% /multicol %}}

The field-based view turns architectural assumptions into **design parameters**, rather than hard-coded algorithmic commitments.

---

## Aggregate Measurements

### A useful extra degree of freedom
Instead of exchanging particles, each node can run its own local particle filter while the weighting step exploits an **aggregated measurement function** built from neighboring observations.

<div>
\[
\hat{y}_t =
H_{\mathcal{N}(k)}
\bigl(
\{ h_j(x_t, v_{j,t}) \}_{j \in \mathcal{N}(k)}
\bigr)
\]
</div>


Nearby sensors collectively behave like a **distributed sensor**. This can be cheaper than exchanging particle sets.

---

## Resilient Fusion-Center Behavior Without a Fixed Center

### Leader-based fusion as a field-level behavior
- **Election:** A leader is selected dynamically.
- **Fusion:** The leader behaves as the current fusion center.
- **Failure:** If the leader disappears, the role is released.
- **Self-healing:** A new leader resumes the behavior.


We can move along the spectrum between centralized simplicity and decentralized robustness through configuration.

---

# Experimental Evaluation

Two target-tracking experiments in a distributed sensor network

---

## Experimental Setup

### Scenario
- Target tracking in a 2D space
- Static network of **25 sensors**
- Sensors deployed on a perturbed spatial grid
- Non-linear target trajectory

### Execution
- Signal quality degrades with distance
- Each sensor executes at **1 Hz**
- Experiments last **3000 simulated seconds**
- Each configuration uses **100 random seeds**

---

## Two Experiments

### 01. Neighborhood measurement aggregation
Each node runs a local PF and aggregates neighboring measurements.

### 02. Leader-based fusion center
One elected leader acts as fusion center, and leader failure is injected during execution.

### Common question
How much coordination is enough to improve tracking without paying the full cost of particle exchange?

---

## Experiment 1: Local PF + Aggregated Measurements

### Setup
Each sensor keeps its **own local particle filter**. No particle exchange occurs among neighbors. What changes is the **measurement used for weighting**.

{{% multicol %}}
{{% col %}}

### Main variable

$$
|\mathcal{N}| \in \{0, 1, 4, 7\}
$$

{{% /col %}}
{{% col %}}

- With very small neighborhoods, filters struggle to converge
- Increasing neighborhood size improves information quality
- Better measurements imply better trajectory reconstruction

{{% /col %}}
{{% /multicol %}}

---

## Experiment 1: Trajectories

<div style="text-align: center;">
  <img src="./images/trajectories.png" style="width: 98%;">
</div>


Aggregating raw local observations can substantially improve weighting quality **without exchanging particle sets**.

---

## Experiment 1: RMSE

<div style="text-align: center;">
  <img src="./images/rmse.png" style="width: 70%;">
</div>

---

## Experiment 2: Elected Leader as Fusion Center

### Setup
An elected leader plays the role of fusion center. Measurements are progressively aggregated toward the leader, and a **leader failure** is injected at time step **1500**.

### What happens
- Initial transient during first leader election
- Second transient when the current leader fails
- Tracking resumes after re-election

### Key message
Fusion-center behavior can be retained, but with self-healing and without a permanently fixed central node.

---

## Experiment 2: Leader-Based Fusion

<div style="text-align: center;">
  <img src="./images/trajectories-fc-based.png" style="width: 68%;">
</div>

---

# Takeaways

### 01. DPF is coordination-heavy
The particle-filter machinery is standard; the difficult part is deciding how information moves through the network.

### 02. Aggregate computing exposes the design space
Fusion, propagation, exchanged information, and active regions become configurable parameters.

### 03. Local cooperation can pay off
Aggregated measurements and leader election improve estimation behavior while avoiding fully centralized assumptions.
