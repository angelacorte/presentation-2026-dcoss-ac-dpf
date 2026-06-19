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

# Motivation

> In IoT, particle filtering is not only an estimation problem: it is a **coordination problem**.

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

Many cyber-physical systems must estimate a **hidden dynamical state** from distributed, noisy observations.

Examples:
- target tracking
- environmental monitoring
- mobility and traffic
- smart-city sensing

### Constraint
No single device directly observes the full state.

{{% /col %}}
{{% col %}}

<div style="text-align: center;">
  <img src="./images/Gemini_Generated_Image_y0tp7fy0tp7fy0tp.jpg" style="width: 92%; border-radius: 0.4rem;">
</div>

**Tracking intuition:** sensors observe fragments of evidence; the system reconstructs the target trajectory.

{{% /col %}}
{{% /multicol %}}

---

# From Observations to a Posterior Estimate

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

We want to reconstruct a latent state over time:

$$
\text{hidden state} \quad x_t
$$

from partial and noisy observations:

$$
\text{observations} \quad y_{1:t}
$$

### Filtering objective
Estimate the posterior distribution:

$$
p(x_t \mid y_{1:t})
$$

This distribution represents uncertainty over the current state.

{{% /col %}}
{{% col %}}

<div style="border: 2px solid #ddd; border-radius: 0.6rem; padding: 1rem; margin-top: 0.5rem;">
  <div style="display: grid; grid-template-columns: 1fr 0.35fr 1fr; align-items: center; gap: 0.8rem; text-align: center;">
    <div>
      <div style="font-size: 3rem;">📡</div>
      <strong>Noisy local sensing</strong><br>
      partial measurements
    </div>
    <div style="font-size: 2.3rem;">→</div>
    <div>
      <div style="font-size: 3rem;">🎯</div>
      <strong>Posterior state estimate</strong><br>
      uncertainty remains explicit
    </div>
  </div>
</div>

### Why particle filters?
Classical linear estimators are not enough when dynamics are **non-linear** and uncertainty is **non-Gaussian**.

{{% /col %}}
{{% /multicol %}}

---

# Particle Filters

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

A particle filter represents belief with a **cloud of weighted hypotheses**.

- each particle is one possible state
- each weight says how plausible it is
- likely particles survive
- unlikely particles fade out

One iteration:

$$
\text{predict} \rightarrow \text{weight} \rightarrow \text{resample} \rightarrow \text{estimate}
$$

{{% /col %}}
{{% col %}}

<div style="text-align: center;">
  <img src="./images/particles-distribution/step_0.png" style="width: 82%;">
</div>

**The estimate is a distribution, not only a point.**

{{% /col %}}
{{% /multicol %}}

---

# Particle Filter Over Time

<div style="display: grid; grid-template-columns: repeat(4, 1fr); gap: 0.6rem; align-items: start;">
  <div style="text-align: center;">
    <img src="./images/particles-distribution/step_0.png" style="width: 100%;">
    <div><strong>t = 0</strong><br>many possible hypotheses</div>
  </div>
  <div style="text-align: center;">
    <img src="./images/particles-distribution/step_1000.png" style="width: 100%;">
    <div><strong>t = 1000</strong><br>belief concentrates</div>
  </div>
  <div style="text-align: center;">
    <img src="./images/particles-distribution/step_2000.png" style="width: 100%;">
    <div><strong>t = 2000</strong><br>the cloud follows the target</div>
  </div>
  <div style="text-align: center;">
    <img src="./images/particles-distribution/step_2900.png" style="width: 100%;">
    <div><strong>t = 2900</strong><br>uncertainty remains explicit</div>
  </div>
</div>

The particle cloud follows the target while keeping the estimation error visible as uncertainty.

---

# Distributed Particle Filters

In IoT systems, observations are naturally collected by many spatially distributed devices.

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

### Centralized PF
All observations are assumed to be available to one estimator:

$$
p(x_t \mid y_{1:t})
$$

Simple, but unrealistic for open IoT deployments.

{{% /col %}}
{{% col class="text-start col-md-5" %}}

### Distributed PF
Each device observes only local information:

$$
y_{t,k} = h_k(x_t, v_{t,k})
$$

The goal is to approximate a global belief through local cooperation.

{{% /col %}}
{{% /multicol %}}

**DPF keeps the filtering logic, but turns estimation into a coordination problem.**

---

# DPF: many coordination choices

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

DPF algorithms differ mainly in how they move, combine, and exploit information across the network.

Design choices include:

- where information is fused
- what information is exchanged
- how far information propagates
- which nodes participate

{{% /col %}}
{{% col %}}

<div style="text-align: center;">
  <img src="./images/pfs.png" style="width: 95%;">
</div>

{{%/ col %}}
{{%/ multicol %}}

---

# What Makes DPF Hard?

{{% multicol %}}
{{% col %}}

### Communication constraints 📡
Bandwidth, latency, energy, and robustness make centralized collection unrealistic.

{{% /col %}}
{{% col %}}
  
### Networks change 🌐
Delays, topology changes, asynchrony, and failures affect estimation.

{{% /col %}}
{{% col %}}

### Trade-offs ⚖️

Accuracy, overhead, complexity, and robustness must be balanced.

{{% /col %}}
{{% /multicol %}}

---

# Aggregate Computing
### A macroprogramming approach to coordination

<div style="text-align: center;">
  <img src="./images/acDevices.svg" style="width: 52%; border-radius: 0.4rem;">
</div>

Aggregate Computing lets developers describe the behaviour of a **collective system**, rather than programming each device separately.

The program manipulates **computational fields**: distributed values evolving across the network.

### Why it helps coordination

Field-level patterns make common distributed behaviours reusable:
- spreading information
- aggregating local evidence
- converging data to selected nodes
- electing leaders dynamically

**Key intuition:** devices execute local code, but the programmer reasons in terms of global coordination patterns.

---

# AC Computational Model

{{% multicol %}}
{{% col %}}

### From collective program to local execution

AC programs are specified at the **collective level**, but executed independently by each device.

At every local round, a device updates its state from:

- local sensor readings
- previous local state
- messages received from neighbours

This makes field-level coordination executable on asynchronous IoT networks.

{{% /col %}}
{{% col %}}

### Round-based model

<div class="local-round-slide">
  <div class="local-round-points">
    <p><strong>Each round consists of three repeated phases:</strong></p>
    <ol>
      <li class="fragment" data-fragment-index="1">
        <strong class="local-round-sense-text">Sense</strong> collect sensor inputs and incoming neighbor messages.
      </li>
      <li class="fragment" data-fragment-index="2">
        <strong class="local-round-compute-text">Compute</strong> run local state evaluation logic to produce <span class="local-round-mark-teal">new state</span> and <span class="local-round-mark-green">outbound messages</span>.
      </li>
      <li class="fragment" data-fragment-index="3">
        <strong class="local-round-interact-text">Interact / Act</strong> share results and affect the local environment.
      </li>
    </ol>
    <p class="fragment local-round-async" data-fragment-index="4">Rounds are <strong>asynchronous</strong>: there is no global lock-step.</p>
  </div>

{{< local-round-loop >}}
</div>

{{% /col %}}
{{% /multicol %}}

**Why it matters for DPF:** each node can run the filtering step locally, while AC controls how evidence is exchanged and propagated.

---

# Key Idea: DPF as a Field Computation

The goal is **not** to introduce yet another DPF algorithm.

DPF becomes a configurable field computation: filtering stays standard, coordination becomes programmable.

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

### Filtering logic
This remains standard:

- prediction
- weighting
- resampling
- estimation

{{% /col %}}
{{% col class="text-start col-md-5" %}}

### Coordination choices
These become programmable:

- where fusion happens
- what is exchanged
- how far information propagates
- who is active

{{% /col %}}
{{% /multicol %}}

**Architectural assumptions become design parameters.**

---

### Contribution 1: 
# Aggregate measurement function

Each node keeps its own local particle filter. Instead of exchanging particle sets, neighbors share measurements that are combined during the weighting step.

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

### Contribution 2: 
# Self-Healing Fusion Center

### Leader-based fusion as a field-level behavior
- **Election:** A leader is selected dynamically.
- **Fusion:** The leader behaves as the current fusion center.
- **Failure:** If the leader disappears, the role is released.
- **Self-healing:** A new leader resumes the behavior.

We can move along the spectrum between centralized simplicity and decentralized robustness through configuration.

---

# Experimental Evaluation

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

### Scenario
- 2D target tracking
- 25 static sensors on a perturbed grid
- signal quality degrades with distance
- sensors execute at 1 Hz
- 3000 simulated seconds
- 100 random seeds per configuration

{{% /col %}}
{{% col class="text-start col-md-5" %}}

### Evaluated configurations
1. **Local PF + aggregated measurements**
   - each sensor has its own PF
   - only measurements are shared

2. **Elected leader as fusion center**
   - measurements converge to a leader
   - leader failure injected at time step 1500

{{% /col %}}
{{% /multicol %}}

---

## Experiment 1

Each sensor keeps its **own local particle filter**.

No particles are exchanged.

Only the measurement used for weighting changes:

$$
|\mathcal{N}| \in \{0, 1, 4, 7\}
$$

<strong>Small neighborhood</strong> → weak local evidence → high error<br>
<strong>Larger neighborhood</strong> → aggregated evidence → better tracking

<div style="text-align: center;">
  <img src="./images/trajectories.png" style="width: 96%;">
</div> 

---

## Experiment 1 results: RMSE

{{% multicol %}}
{{% col %}}

<div style="text-align: center;">
  <img src="./images/rmse.png" style="width: 100%;">
</div>

{{% /col %}}
{{% col %}}

- RMSE measures the distance between the estimated target position and the real one.
- With few neighbors, the error remains high.
- Increasing $|\mathcal{N}|$ reduces RMSE and improves long-term stability.
- The benefit comes from sharing **measurements**, not particle sets.

{{% /col %}}
{{% /multicol %}}

---

## Experiment 2: Self-Healing Fusion

{{% multicol %}}
{{% col class="text-start col-md-5" %}}

An elected leader plays the fusion-center role.

At time step **1500**, the leader fails.

The system shows:

1. Transient during initial leader election
2. Convergence to a valid estimation
3. Transient after leader failure
4. Resumed tracking after re-election

Fusion-center behavior can be retained without a permanently fixed center.

{{% /col %}}
{{% col %}}

<div style="text-align: center;">
  <img src="./images/trajectories-fc-based.png" style="width: 82%;">
</div>

The red line marks the failure; the following deviation is the temporary tracking error caused by reconfiguration.

{{% /col %}}
{{% /multicol %}}

--- 

# Takeaways

### 01. DPF is coordination-heavy
The particle-filter machinery is standard; the difficult part is deciding how information moves through the network.

### 02. Aggregate computing abstracts the network architecture
Fusion, propagation, exchanged information, and active regions become configurable parameters.

### 03. Local cooperation can pay off
Aggregated measurements improve tracking accuracy while avoiding particle exchange.

### 04. Fusion can be resilient
Leader election preserves fusion-center behaviour without relying on a permanently fixed center.

--- 

## Reproducible experiments here!

<div style="text-align: center;">
  {{< qrcode data="https://github.com/domm99/experiments-ac-based-distributed-particle-filtering" width=240 height=240 dotsColor="theme" backgroundColor="transparent" >}}
<p><i class="fab fa-github mr-3" style="color: #095aa6;"></i> <a href="https://github.com/domm99/experiments-ac-based-distributed-particle-filtering">domm99/experiments-ac-based-distributed-particle-filtering</a></p>
</div>
