+++
title = "Flexible Distributed Particle Filtering for the Internet of Things via Aggregate Computing"
description = "DCOSS-IoT 2026 presentation"
outputs = ["Reveal"]
+++

# Flexible Distributed Particle Filtering for the Internet of Things via Aggregate Computing

{{% spacer %}}

[**<span class="deck-title-accent">Angela Cortecchia</span>**](mailto:angela.cortecchia@unibo.it),
[Davide Domini](mailto:davide.domini@unibo.it),
[Giovanni Ciatto](mailto:giovanni.ciatto@unibo.it),
[Roberto Casadei](mailto:roby.casadei@unibo.it),
[Danilo Pianini](mailto:danilo.pianini@unibo.it),
and
[Mirko Viroli](mailto:mirko.viroli@unibo.it)

{{% spacer %}}

<span class="deck-affiliation">*Department of Computer Science and Engineering (DISI)<br>
Alma Mater Studiorum -- University of Bologna - Cesena, Italy*</span>

<div class="hero-logo">
  <img src="./images/DIP INFORMATICA-SCIENZA E INGEGNERIA_DISI_EN.svg">
</div>

---

# Motivation

<div class="motivation-layout">
  <div class="motivation-copy">
    <p>Many cyber-physical systems must estimate a <strong>hidden dynamical state</strong> from distributed, noisy observations <small>[1]</small>.</p>
    <div class="pattern-line is-left">
      <span>target tracking</span>
      <span>environmental monitoring</span>
      <span>mobility and traffic</span>
      <span>smart-city sensing</span>
    </div>
    <div class="constraint-line">
      <span>Constraint</span>
      <p>No single device directly observes the full state.</p>
    </div>
  </div>
  <div class="motivation-visual">
    <img src="./images/Gemini_Generated_Image_y0tp7fy0tp7fy0tp.jpg">
    <p><strong>Tracking intuition:</strong> sensors observe fragments of evidence; the system reconstructs the target trajectory.</p>
  </div>
</div>

{{< footer >}}
[1] O. Hlinka, F. Hlawatsch, and P. M. Djuric, "Distributed particle filtering in agent networks: A survey, classification, and comparison," IEEE Signal Process. Mag., 2013.
{{< /footer >}}

---

# From Observations to a Posterior Estimate

<div class="posterior-layout" style="align-items: flex-start;">
  <div class="posterior-copy" style="margin-top: 0; padding-top: 0;">
    <p>Reconstruct a latent state over time from partial and noisy observations.</p>
    <div class="equation-line">
      <span>hidden state</span>
      <strong>$$x_t$$</strong>
    </div>
    <div class="equation-line">
      <span>observations</span>
      <strong>$$y_{1:t}$$</strong>
    </div>
    <div class="equation-line is-focus">
      <span>filtering objective</span>
      <strong>$$p(x_t \mid y_{1:t})$$</strong>
    </div>
  </div>
  <div class="posterior-side" style="margin-top: 0; padding-top: 0;">
    <div class="posterior-flow">
      <div class="posterior-flow-step">
      <strong>Noisy local sensing</strong>
      partial measurements
      </div>
      <div class="posterior-flow-arrow">→</div>
      <div class="posterior-flow-step">
      <strong>Posterior state estimate</strong>
      uncertainty remains explicit
      </div>
    </div>
    <p class="side-statement" style="margin-top: 1.6rem; margin-bottom: 1.8rem;">Classical linear estimators are not enough when dynamics are <strong>non-linear</strong> and uncertainty is <strong>non-Gaussian</strong>.</p>
    <img src="images/prediction.svg" class="posterior-diagram" style="margin-top: 0;">
  </div>
</div>

---

# Particle Filters

<div class="pf-layout">
  <div class="pf-copy">
    <p>A particle filter represents belief with a <strong>cloud of weighted hypotheses</strong> <small>[2]</small>.</p>
    <div class="takeaway-line">
      <span>01</span>
      <p>Each particle is one possible state.</p>
    </div>
    <div class="takeaway-line">
      <span>02</span>
      <p>Each weight says how plausible it is.</p>
    </div>
    <div class="takeaway-line">
      <span>03</span>
      <p>Likely particles survive; unlikely particles fade out.</p>
    </div>
    <div class="process-line">
      <span>predict</span>
      <span>weight</span>
      <span>resample</span>
      <span>estimate</span>
    </div>
  </div>
  <div class="pf-visual">
    <img src="./images/particles-distribution/step_0.png">
    <p><strong>The estimate is a distribution, not only a point.</strong></p>
  </div>
</div>

{{< meta-note >}}
In IoT, particle filtering is not only an estimation problem: it is a **coordination problem**.
{{< /meta-note >}}

{{< footer >}}
[2] M. S. Arulampalam, S. Maskell, N. J. Gordon, and T. Clapp, "A tutorial on particle filters for online nonlinear/non-gaussian bayesian tracking," IEEE Trans. Signal Process., 2002.
{{< /footer >}}

---

# Particle Filter Over Time

<div class="image-grid">
  <div class="image-grid-item">
    <img src="./images/particles-distribution/step_0.png">
    <div class="image-grid-title">t = 0</div>
    <div class="image-grid-caption">many possible hypotheses</div>
  </div>
  <div class="image-grid-item">
    <img src="./images/particles-distribution/step_1000.png">
    <div class="image-grid-title">t = 1000</div>
    <div class="image-grid-caption">belief concentrates</div>
  </div>
  <div class="image-grid-item">
    <img src="./images/particles-distribution/step_2000.png">
    <div class="image-grid-title">t = 2000</div>
    <div class="image-grid-caption">the cloud follows the target</div>
  </div>
  <div class="image-grid-item">
    <img src="./images/particles-distribution/step_2900.png">
    <div class="image-grid-title">t = 2900</div>
    <div class="image-grid-caption">uncertainty remains explicit</div>
  </div>
</div>

{{< meta-note >}}
The particle cloud follows the target while keeping the estimation error visible as uncertainty.
{{< /meta-note >}}

---

# Distributed Particle Filters

<p class="lead-line">In IoT systems, observations are naturally collected by many spatially distributed devices.</p>

<div class="dpf-compare">
  <div class="compare-item">
    <h3>Centralized PF</h3>
    <p>All observations are assumed to be available to one estimator.</p>
    <div class="compact-equation">$$p(x_t \mid y_{1:t})$$</div>
    <p>Simple, but unrealistic for open IoT deployments.</p>
  </div>
  <div class="compare-item is-focus">
    <h3>Distributed PF</h3>
    <p>Each device observes only local information.</p>
    <div class="compact-equation">$$y_{t,k} = h_k(x_t, v_{t,k})$$</div>
    <p>The goal is to approximate the same global belief through local cooperation.</p>
  </div>
</div>

{{< meta-note >}}
**DPF keeps the filtering logic, but turns estimation into a coordination problem.**
{{< /meta-note >}}

---

# DPF: many coordination choices

<div class="choices-layout">
  <div class="choices-copy">
    <p>DPF algorithms differ mainly in how they move, combine, and exploit information across the network.</p>
    <div class="pattern-line is-left">
      <span>where fusion happens</span>
      <span>what is exchanged</span>
      <span>how far evidence propagates</span>
      <span>which nodes participate</span>
    </div>
  </div>
  <div class="choices-tree">
    {{< pf-taxonomy-tree >}}
  </div>
</div>

---

# What Makes DPF Hard?

{{% spacer %}}

<div class="challenge-strip">
  <div class="challenge-item">
    <div>
      <h3>Communication constraints</h3>
      <p>Bandwidth, latency, energy, and robustness make centralized collection unrealistic.</p>
    </div>
  </div>
  <div class="challenge-item">
    <div>
      <h3>Networks change</h3>
      <p>Delays, topology changes, asynchrony, and failures affect estimation.</p>
    </div>
  </div>
  <div class="challenge-item is-critical">
    <div>
      <h3>Trade-offs</h3>
      <p>Accuracy, overhead, complexity, and robustness must be balanced.</p>
    </div>
  </div>
</div>

---

# Aggregate Computing
### A macroprogramming approach to coordination

<div class="ac-overview">
  <div class="ac-visual">
    <img src="./images/acDevices.svg">
  </div>
  <div class="ac-copy">
      <p>Aggregate Computing lets developers describe the <strong>collective behavior</strong> of a system, rather than programming each device separately <small>[3]</small>.</p>
    <p>The program manipulates <strong>computational fields</strong>: distributed values evolving across the network <small>[4]</small>.</p>
  </div>
</div>

<div class="pattern-line">
  <span>spreading information</span>
  <span>aggregating evidence</span>
  <span>converging data</span>
  <span>electing leaders</span>
</div>

{{< meta-note >}}
**Key intuition:** devices execute local code, but the programmer reasons in terms of global coordination patterns.
{{< /meta-note >}}

{{< footer >}}
[3] J. Beal, D. Pianini, and M. Viroli, "Aggregate programming for the internet of things," Computer, 2015.<br>
[4] G. Audrito, M. Viroli, F. Damiani, D. Pianini, and J. Beal, "A higher-order calculus of computational fields," ACM Trans. Comput. Log., 2019.
{{< /footer >}}

---

# AC Computational Model

{{% multicol %}}
{{% col %}}

### From collective program to local execution

<div class="ac-model-layout">
  <div class="takeaway-editorial">
    <p>One program, many local executions</p>
    <div class="takeaway-line">
      <span>01</span>
      <p><strong>Collective specification.</strong> The programmer writes the behavior of the whole device network.</p>
    </div>
    <div class="takeaway-line">
      <span>02</span>
      <p><strong>Local rounds.</strong> Each device evaluates the same program using local sensors, memory, and neighbor messages.</p>
    </div>
    <div class="takeaway-line is-critical">
      <span>03</span>
      <p><strong>Emergent field.</strong> The collection of local values forms the global computational field.</p>
    </div>
  </div>
</div>

<div class="pattern-line">
  <span>local sensing</span>
  <span>neighbor exchange</span>
  <span>state update</span>
  <span>field evolution</span>
</div>

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
    <p class="fragment local-round-async" data-fragment-index="4">The model does not require global lock-step.</p>
  </div>

{{< local-round-loop >}}

{{% /col %}}
{{% /multicol %}}

---

# Key Idea: DPF as a Field Computation

<p class="lead-line">The goal is <strong>not</strong> to introduce yet another DPF algorithm.</p>

<div class="idea-layout">
  <div class="idea-side">
    <h3>Filtering logic</h3>
    <p>This remains standard.</p>
    <div class="process-line">
      <span>prediction</span>
      <span>weighting</span>
      <span>resampling</span>
      <span>estimation</span>
    </div>
  </div>
  <div class="idea-side is-focus">
    <h3>Coordination choices</h3>
    <p>These become programmable.</p>
    <div class="process-line">
      <span>where fusion happens</span>
      <span>what to exchange</span>
      <span>how information propagates</span>
      <span>who is active</span>
    </div>
  </div>
</div>

{{< meta-note >}}
**Architectural assumptions become design parameters.**
{{< /meta-note >}}

---

### Contribution 1: 
# Aggregate measurement function

<div class="contribution-layout">
  <div class="contribution-copy">
    <p>Each node keeps its own local particle filter.</p>
    <p>Instead of exchanging particle sets, neighbors share raw observations; local likelihoods are combined during the weighting step.</p>
    <p>Nearby sensors collectively behave like a <strong>distributed sensor</strong>.</p>
  </div>
  <div class="equation-spotlight">
    \[
    \hat{y}_t =
    H_{\mathcal{N}(k)}
    \bigl(
    \{ h_j(x_t, v_{j,t}) \}_{j \in \mathcal{N}(k)}
    \bigr)
    \]
  </div>
</div>

{{< meta-note >}}
<strong>Intuition:</strong> more neighbors provide richer local observations, so the aggregated measurement becomes more informative; with few neighbors, the estimate remains weaker and less stable.
{{< /meta-note >}}

---

### Contribution 2: 
# Self-Healing Fusion Center

<div class="healing-layout">
  <div class="takeaway-editorial">
    <h2>Leader-based fusion as a field-level behavior <small>[5] [6]</small></h2>
    <div class="takeaway-line">
      <span>01</span>
      <p><strong>Election.</strong> A leader is selected dynamically.</p>
    </div>
    <div class="takeaway-line">
      <span>02</span>
      <p><strong>Fusion.</strong> The leader behaves as the current fusion center.</p>
    </div>
    <div class="takeaway-line is-critical">
      <span>03</span>
      <p><strong>Failure.</strong> If the leader disappears, the role is reassigned.</p>
    </div>
    <div class="takeaway-line">
      <span>04</span>
      <p><strong>Self-healing.</strong> A new leader resumes the behavior.</p>
    </div>
  </div>
  <div class="healing-statement">
    <p>Configuration lets us move along the spectrum between centralized simplicity and decentralized robustness.</p>
  </div>
</div>

{{< footer >}}
[5] M. Viroli, G. Audrito, J. Beal, F. Damiani, and D. Pianini, "Engineering resilient collective adaptive systems by self-stabilisation," ACM Trans. Model. Comput. Simul., 2018.<br>
[6] D. Pianini, R. Casadei, and M. Viroli, "Self-stabilising priority-based multi-leader election and network partitioning," ACSOS, 2022.
{{< /footer >}}

---

# Experimental Evaluation

<div class="eval-layout">
  <div class="eval-main">
    <h3>Scenario</h3>
    <div class="metric-band">
      <span class="metric-pill">2D target tracking</span>
      <span class="metric-pill">25 sensors</span>
      <span class="metric-pill">1 Hz, unsynchronized</span>
      <span class="metric-pill">3000 simulated seconds</span>
      <span class="metric-pill">100 seeds</span>
    </div>
    <p>Each sensor observes the target through a radio-like signal: closer targets produce stronger, cleaner evidence; farther targets produce weaker and noisier evidence <small>[7] [8]</small>.</p>
    <p>Simulations are implemented in Alchemist<small>[7]</small>, with aggregate program written using Collektive <small>[8]</small>.</p>
  </div>
  <div class="eval-configs">
    <h3>Evaluated configurations</h3>
    <div class="config-row">
      <span class="config-index">1</span>
      <div><strong>Local PF + aggregated measurements</strong><br>each sensor has its own PF; only measurements are shared.</div>
    </div>
    <div class="config-row is-critical">
      <span class="config-index">2</span>
      <div><strong>Elected leader as fusion center</strong><br>measurements converge to a leader; failure is injected at time step 1500.</div>
    </div>
  </div>
</div>

<img alt="experiment setup" src="./images/experiment.gif" width="30%"/>

{{< footer >}}
[7] D. Pianini, S. Montagna, and M. Viroli, "Chemical-oriented simulation of computational systems with alchemist," Journal of Simulation, 2013.
[8] A. Cortecchia, "Multiplatform self-organizing systems through a KotlinMP implementation of aggregate computing," ACSOS Companion, 2024.<br>
{{< /footer >}}

---

## Experiment 1: AGGREGATE MEASUREMENT FUNCTION

<div class="experiment-brief">
  <div>
    <p>Each sensor keeps its <strong>own local particle filter</strong>. No particles are exchanged.</p>
  </div>
  <div class="experiment-equation">$$|\mathcal{N}| \in \{0, 1, 4, 7\}$$</div>
  <div>
    <p><strong>Small neighborhood</strong> → weak evidence → high error</p>
    <p><strong>Larger neighborhood</strong> → aggregated evidence → better tracking</p>
  </div>
</div>

<div class="plot-card">
  <img src="./images/trajectories.png" style="width: 76%;">
</div>

---

## Experiment 1 results: RMSE

{{% multicol %}}
{{% col %}}

<div class="plot-card">
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

<div class="exp2-layout">
  <div class="exp2-copy">
    <p>An elected leader plays the fusion-center role. At time step <strong>1500</strong>, the leader fails.</p>
    <div class="takeaway-line">
      <span>01</span>
      <p>Transient during initial leader election</p>
    </div>
    <div class="takeaway-line">
      <span>02</span>
      <p>Convergence to a valid estimation</p>
    </div>
    <div class="takeaway-line is-critical">
      <span>03</span>
      <p>Transient after leader failure</p>
    </div>
    <div class="takeaway-line">
      <span>04</span>
      <p>Resumed tracking after re-election</p>
    </div>
    <p>Fusion-center behavior can be retained without a permanently fixed center.</p>
  </div>
  <div class="plot-card">
    <img src="./images/trajectories-fc-based.png" style="width: 82%;">
    <div class="plot-caption">The <span class="accent-red">red line</span> marks the failure; the following deviation is the temporary tracking error caused by reconfiguration.</div>
  </div>
</div>

--- 

# Takeaways and Future Work

<div class="closing-layout">
  <div class="takeaway-editorial">
    <h2>Takeaways</h2>
    <div class="takeaway-line">
      <span>-</span>
      <p><strong>DPF as field computation.</strong> Filtering stays standard; coordination becomes programmable.</p>
    </div>
    <div class="takeaway-line">
      <span>-</span>
      <p><strong>Flexible architectures.</strong> Fusion, dissemination, and active regions become configurable choices.</p>
    </div>
    <div class="takeaway-line">
      <span>-</span>
      <p><strong>Local likelihood aggregation works.</strong> Sharing observations improves tracking without exchanging particle sets.</p>
    </div>
    <div class="takeaway-line is-critical">
      <span>-</span>
      <p><strong>Fusion can self-heal.</strong> Leader election preserves fusion-center behavior after failures.</p>
    </div>
  </div>
  <div class="future-panel">
    <h2>Future work</h2>
    <div class="future-group">
      <h3>Explore more AC design dimensions</h3>
      <p>Broader coordination strategies for propagation and fusion; activation only in relevant regions.</p>
    </div>
    <div class="future-group">
      <h3>Extend the scenarios</h3>
      <p>Multiple moving targets, flocking-inspired coordination, heterogeneous sensing, and richer deployment conditions.</p>
    </div>
  </div>
</div>

--- 

# Thank you for your attention!

{{% spacer %}}

### Reproducible experiments here:

<div class="insight-card" style="max-width: 58%; margin: 0 auto;">
  {{< qrcode data="https://github.com/domm99/experiments-ac-based-distributed-particle-filtering" width=240 height=240 dotsColor="theme" backgroundColor="transparent" >}}
  <p><i class="fab fa-github mr-3" style="color: #095aa6;"></i> <a href="https://github.com/domm99/experiments-ac-based-distributed-particle-filtering">domm99/experiments-ac-based-distributed-particle-filtering</a></p>
</div>
