---
title: "Minimum-Energy Guidance Law for Spacecraft Rendezvous in the Hill Frame"
date: 2026-04-22 12:00:00 -0400
subtitle: Spacecraft Rendezvous • Optimal Control • Guidance
description: "A minimum-energy spacecraft rendezvous project using Hill-frame relative motion, Clohessy-Wiltshire dynamics, and continuous-time optimal control to guide proximity operations."
image: '/images/sv-rpo/rendezvous-hero.png'
featured: true
---
<style>
  .sv-rpo-intro {
    margin: 0 0 2rem;
    padding: 1.75rem;
    border: 1px solid var(--border-color);
    border-radius: 24px;
    background: linear-gradient(135deg, rgba(20, 79, 138, 0.12), rgba(195, 37, 45, 0.08));
  }

  .sv-rpo-intro p:last-child,
  .sv-rpo-stat p,
  .sv-rpo-resource p,
  .sv-rpo-case p:last-child {
    margin-bottom: 0;
  }

  .sv-rpo-stats,
  .sv-rpo-resources,
  .sv-rpo-model-grid,
  .sv-rpo-media-grid,
  .sv-rpo-detail-grid {
    display: grid;
    gap: 1rem;
  }

  .sv-rpo-stats {
    grid-template-columns: repeat(4, minmax(0, 1fr));
    margin-top: 1.5rem;
  }

  .sv-rpo-resources {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    margin: 1.5rem 0 0;
  }

  .sv-rpo-model-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    margin: 1.5rem 0;
  }

  .sv-rpo-media-grid {
    grid-template-columns: minmax(0, 1fr) minmax(260px, 0.75fr);
    align-items: start;
    margin: 1rem 0 1.25rem;
  }

  .sv-rpo-detail-grid {
    grid-template-columns: repeat(3, minmax(0, 1fr));
  }

  .sv-rpo-equation-flow {
    counter-reset: equation-step;
    display: grid;
    gap: 1rem;
    margin: 1.5rem 0 2rem;
  }

  .sv-rpo-stat,
  .sv-rpo-resource,
  .sv-rpo-model-card,
  .sv-rpo-equation-card,
  .sv-rpo-case,
  .sv-rpo-details {
    border: 1px solid var(--border-color);
    border-radius: 20px;
    background: var(--background-alt-color);
  }

  .sv-rpo-stat {
    padding: 1rem;
  }

  .sv-rpo-stat strong {
    display: block;
    margin-bottom: 0.3rem;
    color: var(--heading-font-color);
    font-size: clamp(1.35rem, 2.8vw, 2rem);
    line-height: 1;
  }

  .sv-rpo-stat span,
  .sv-rpo-resource p,
  .sv-rpo-case-meta,
  .sv-rpo-caption {
    color: var(--text-alt-color);
  }

  .sv-rpo-resource {
    display: flex;
    flex-direction: column;
    gap: 0.55rem;
    padding: 1rem 1.1rem;
  }

  .sv-rpo-resource strong {
    color: var(--heading-font-color);
  }

  .sv-rpo-resource a {
    display: inline-flex;
    align-items: center;
    gap: 0.45rem;
    width: fit-content;
    text-decoration: none;
  }

  .sv-rpo-model-card {
    padding: 1.15rem 1.2rem;
  }

  .sv-rpo-equation-card {
    padding: 1.2rem;
  }

  .sv-rpo-equation-card h3 {
    display: flex;
    align-items: center;
    gap: 0.6rem;
    margin-bottom: 0.65rem;
  }

  .sv-rpo-equation-card h3::before {
    counter-increment: equation-step;
    content: counter(equation-step);
    display: inline-flex;
    align-items: center;
    justify-content: center;
    min-width: 1.9rem;
    height: 1.9rem;
    border-radius: 50%;
    color: var(--white);
    background: var(--brand-color);
    font-family: "Inter Tight", Helvetica Neue, Helvetica, Arial, sans-serif;
    font-size: 0.9rem;
    line-height: 1;
  }

  .sv-rpo-equation-card p:last-child {
    margin-bottom: 0;
  }

  .sv-rpo-equation {
    overflow-x: auto;
    margin: 0.95rem 0 0.65rem;
    padding: 0.95rem 1rem;
    border: 1px solid var(--border-color);
    border-radius: 16px;
    background: var(--background-color);
  }

  .sv-rpo-formula {
    min-width: max-content;
    color: var(--heading-font-color);
    font-family: "Old Standard TT", "Times New Roman", Georgia, serif;
    font-size: clamp(1.05rem, 2.4vw, 1.35rem);
    line-height: 1.75;
    white-space: nowrap;
  }

  .sv-rpo-formula sub,
  .sv-rpo-formula sup {
    line-height: 0;
  }

  .sv-rpo-eq-stack {
    display: grid;
    gap: 0.35rem;
  }

  .sv-rpo-vector,
  .sv-rpo-matrix {
    display: inline-grid;
    align-items: center;
    vertical-align: middle;
    margin: 0 0.15rem;
    padding: 0.1rem 0.55rem;
    border-right: 2px solid currentColor;
    border-left: 2px solid currentColor;
    line-height: 1.35;
  }

  .sv-rpo-vector {
    grid-auto-flow: column;
    grid-auto-columns: max-content;
    gap: 0.45rem;
  }

  .sv-rpo-vector--column {
    grid-auto-flow: row;
    grid-auto-rows: max-content;
    gap: 0.18rem;
  }

  .sv-rpo-matrix {
    gap: 0.18rem 0.62rem;
  }

  .sv-rpo-matrix--six {
    grid-template-columns: repeat(6, minmax(1.25rem, max-content));
  }

  .sv-rpo-matrix--three {
    grid-template-columns: repeat(3, minmax(1.25rem, max-content));
  }

  .sv-rpo-matrix--two {
    grid-template-columns: repeat(2, minmax(1.25rem, max-content));
  }

  .sv-rpo-equation-note {
    margin: 0;
    color: var(--text-alt-color);
    font-size: 0.95rem;
  }

  .sv-rpo-scroll {
    overflow-x: auto;
    margin: 1.5rem 0;
  }

  .sv-rpo-table {
    width: 100%;
    min-width: 760px;
    border-collapse: collapse;
    font-size: 0.94rem;
  }

  .sv-rpo-table th,
  .sv-rpo-table td {
    padding: 0.85rem 0.95rem;
    border-bottom: 1px solid var(--border-color);
    text-align: left;
    vertical-align: top;
  }

  .sv-rpo-table th {
    color: var(--heading-font-color);
    background: rgba(20, 79, 138, 0.06);
  }

  .sv-rpo-case {
    margin: 1.75rem 0;
    padding: 1.15rem;
  }

  .sv-rpo-case h3 {
    margin-bottom: 0.25rem;
  }

  .sv-rpo-case-meta {
    margin-bottom: 0.85rem;
    font-size: 0.95rem;
  }

  .sv-rpo-video {
    width: 100%;
    border-radius: 20px;
    background: var(--background-alt-color-2);
  }

  .sv-rpo-caption {
    margin: 0.55rem 0 0;
    font-size: 0.92rem;
  }

  .sv-rpo-details {
    margin-top: 1rem;
    overflow: hidden;
  }

  .sv-rpo-details summary {
    padding: 0.95rem 1.05rem;
    color: var(--heading-font-color);
    cursor: pointer;
    font-weight: 600;
  }

  .sv-rpo-detail-grid {
    padding: 0 1rem 1rem;
  }

  .sv-rpo-detail-grid img {
    width: 100%;
  }

  @media (max-width: 980px) {
    .sv-rpo-stats {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }

    .sv-rpo-media-grid,
    .sv-rpo-detail-grid {
      grid-template-columns: minmax(0, 1fr);
    }
  }

  @media (max-width: 680px) {
    .sv-rpo-stats,
    .sv-rpo-resources,
    .sv-rpo-model-grid {
      grid-template-columns: minmax(0, 1fr);
    }

    .sv-rpo-intro,
    .sv-rpo-stat,
    .sv-rpo-resource,
    .sv-rpo-model-card,
    .sv-rpo-equation-card,
    .sv-rpo-equation,
    .sv-rpo-case,
    .sv-rpo-details,
    .sv-rpo-video {
      border-radius: 16px;
    }
  }
</style>

<div class="sv-rpo-intro">
  <p>
    This project studies how to guide a deputy spacecraft into rendezvous with a target while minimizing control effort. The work is built around relative motion in the Hill frame, where the target spacecraft defines the local orbital reference frame and the deputy is guided through proximity operations using the linearized Clohessy-Wiltshire equations.
  </p>
  <p>
    Built as a graduate optimal-control project, the core idea is broader than the class setting: derive a continuous-time minimum-energy guidance law, implement it in MATLAB, and validate it across multiple rendezvous geometries that include direct docking, offset terminal states, and a compact nominal docking case.
  </p>

  <div class="sv-rpo-stats">
    <div class="sv-rpo-stat">
      <strong>5</strong>
      <span>completed rendezvous scenarios</span>
    </div>
    <div class="sv-rpo-stat">
      <strong>600-1500 s</strong>
      <span>terminal guidance horizons</span>
    </div>
    <div class="sv-rpo-stat">
      <strong>&le; 2.3e-13 m</strong>
      <span>final position error across all cases</span>
    </div>
    <div class="sv-rpo-stat">
      <strong>0</strong>
      <span>pseudo-inverse fallbacks required</span>
    </div>
  </div>

  <div class="sv-rpo-resources">
    <div class="sv-rpo-resource">
      <strong>Paper</strong>
      <p>[Coming soon]</p>
    </div>
    <div class="sv-rpo-resource">
      <strong>Presentation</strong>
      <a href="/files/sv-rpo/ME8220_Project_Presentation.pdf" target="_blank" rel="noopener">
        <i class="fa fa-file-pdf-o"></i> View Final Presentation (PDF)
      </a>
    </div>
  </div>
</div>

## Project Motivation

Rendezvous and proximity operations are central to docking, inspection, servicing, and formation-flying missions. The hard part is not only reaching the target, but reaching it with the right terminal position and velocity while using limited propellant. This project frames that challenge as a minimum-energy optimal-control problem in the Hill frame.

The resulting guidance law gives a direct way to compute the acceleration command needed to satisfy a prescribed terminal state. Instead of tuning a feedback controller first, the project starts from the continuous-time optimal-control formulation and then uses simulation to check whether the derived law behaves correctly across different relative-motion cases.

<div class="sv-rpo-model-grid">
  <div class="sv-rpo-model-card">
    <h3>Relative-Motion Model</h3>
    <p>
      The deputy spacecraft is modeled relative to a chief spacecraft on a circular orbit. The Hill-frame state contains relative position and velocity, and the Clohessy-Wiltshire equations provide the linear time-invariant dynamics used by the guidance law.
    </p>
  </div>
  <div class="sv-rpo-model-card">
    <h3>Minimum-Energy Guidance</h3>
    <p>
      The controller minimizes integrated acceleration effort while enforcing the desired terminal state. Each scenario uses the same guidance structure, which makes the cases useful for comparing geometry, time horizon, and offset terminal requirements.
    </p>
  </div>
</div>

## Mathematical Foundation

The math behind the project is compact enough to tell as a sequence. Start with a relative-motion state, write the Hill-frame dynamics, pose the minimum-energy problem, apply the optimality conditions, and then solve the two-point boundary value problem that enforces the desired final state.

<section class="sv-rpo-equation-flow" aria-label="Minimum-energy rendezvous equation flow">
  <div class="sv-rpo-equation-card">
    <h3>Relative State and Mean Motion</h3>
    <p>
      The deputy is described in the chief spacecraft's local Hill frame. The state collects relative position and velocity, while the chief's mean motion sets the natural orbital frequency.
    </p>
    <div class="sv-rpo-equation">
      <div class="sv-rpo-eq-stack">
        <div class="sv-rpo-formula">
          x =
          <span class="sv-rpo-vector">
            <span>&rho;<sub>x</sub></span>
            <span>&rho;<sub>y</sub></span>
            <span>&rho;<sub>z</sub></span>
            <span>v<sub>x</sub></span>
            <span>v<sub>y</sub></span>
            <span>v<sub>z</sub></span>
          </span><sup>T</sup>,
          &nbsp;
          u =
          <span class="sv-rpo-vector">
            <span>u<sub>x</sub></span>
            <span>u<sub>y</sub></span>
            <span>u<sub>z</sub></span>
          </span><sup>T</sup>
        </div>
        <div class="sv-rpo-formula">
          n = &radic;(&mu; / r<sub>c</sub><sup>3</sup>)
        </div>
      </div>
    </div>
    <p class="sv-rpo-equation-note">
      Here, &rho; is relative position, v is relative velocity, u is commanded acceleration, &mu; is the gravitational parameter, and r<sub>c</sub> is the chief orbit radius.
    </p>
  </div>

  <div class="sv-rpo-equation-card">
    <h3>Clohessy-Wiltshire Dynamics</h3>
    <p>
      For a circular chief orbit and small relative separation, the Hill-frame equations become a linear time-invariant model. This captures the coupling between radial and along-track motion while leaving cross-track motion as a harmonic mode.
    </p>
    <div class="sv-rpo-equation">
      <div class="sv-rpo-eq-stack">
        <div class="sv-rpo-formula">&rho;&#776;<sub>x</sub> - 2n&rho;&#775;<sub>y</sub> - 3n<sup>2</sup>&rho;<sub>x</sub> = u<sub>x</sub></div>
        <div class="sv-rpo-formula">&rho;&#776;<sub>y</sub> + 2n&rho;&#775;<sub>x</sub> = u<sub>y</sub></div>
        <div class="sv-rpo-formula">&rho;&#776;<sub>z</sub> + n<sup>2</sup>&rho;<sub>z</sub> = u<sub>z</sub></div>
      </div>
    </div>
    <p class="sv-rpo-equation-note">
      These equations are the Clohessy-Wiltshire relative orbit equations with acceleration control in each Hill-frame axis.
    </p>
  </div>

  <div class="sv-rpo-equation-card">
    <h3>State-Space Form</h3>
    <p>
      The scalar equations are assembled into the state-space form used by the simulations and guidance solve.
    </p>
    <div class="sv-rpo-equation">
      <div class="sv-rpo-eq-stack">
        <div class="sv-rpo-formula">x&#775; = A x + B u</div>
        <div class="sv-rpo-formula">
          A =
          <span class="sv-rpo-matrix sv-rpo-matrix--six">
            <span>0</span><span>0</span><span>0</span><span>1</span><span>0</span><span>0</span>
            <span>0</span><span>0</span><span>0</span><span>0</span><span>1</span><span>0</span>
            <span>0</span><span>0</span><span>0</span><span>0</span><span>0</span><span>1</span>
            <span>3n<sup>2</sup></span><span>0</span><span>0</span><span>0</span><span>2n</span><span>0</span>
            <span>0</span><span>0</span><span>0</span><span>-2n</span><span>0</span><span>0</span>
            <span>0</span><span>0</span><span>-n<sup>2</sup></span><span>0</span><span>0</span><span>0</span>
          </span>,
          &nbsp;
          B =
          <span class="sv-rpo-matrix sv-rpo-matrix--three">
            <span>0</span><span>0</span><span>0</span>
            <span>0</span><span>0</span><span>0</span>
            <span>0</span><span>0</span><span>0</span>
            <span>1</span><span>0</span><span>0</span>
            <span>0</span><span>1</span><span>0</span>
            <span>0</span><span>0</span><span>1</span>
          </span>
        </div>
      </div>
    </div>
    <p class="sv-rpo-equation-note">
      The first three rows map velocity into position; the lower rows encode the CW accelerations and direct acceleration control.
    </p>
  </div>

  <div class="sv-rpo-equation-card">
    <h3>Minimum-Energy Objective</h3>
    <p>
      The rendezvous problem is posed as a fixed-time, fixed-endpoint optimal-control problem. The terminal state is specified by the scenario, and the cost penalizes total acceleration effort.
    </p>
    <div class="sv-rpo-equation">
      <div class="sv-rpo-eq-stack">
        <div class="sv-rpo-formula">minimize &nbsp; J = &frac12; &int;<sub>t0</sub><sup>tf</sup> u(t)<sup>T</sup> R u(t) dt</div>
        <div class="sv-rpo-formula">subject to &nbsp; x(t<sub>0</sub>) = x<sub>0</sub>, &nbsp; x(t<sub>f</sub>) = x<sub>f</sub></div>
      </div>
    </div>
    <p class="sv-rpo-equation-note">
      The reported runs use R = I, so the cost corresponds directly to integrated squared control acceleration.
    </p>
  </div>

  <div class="sv-rpo-equation-card">
    <h3>Guidance Law from Optimality Conditions</h3>
    <p>
      Applying the Hamiltonian conditions produces a coupled state-costate system. Solving for the initial costate gives the control history that reaches the desired terminal state with minimum effort.
    </p>
    <div class="sv-rpo-equation">
      <div class="sv-rpo-eq-stack">
        <div class="sv-rpo-formula">H = &frac12;u<sup>T</sup>Ru + &lambda;<sup>T</sup>(Ax + Bu)</div>
        <div class="sv-rpo-formula">u<sup>*</sup>(t) = -R<sup>-1</sup>B<sup>T</sup>&lambda;(t), &nbsp; &lambda;&#775; = -A<sup>T</sup>&lambda;</div>
        <div class="sv-rpo-formula">
          d/dt
          <span class="sv-rpo-vector sv-rpo-vector--column">
            <span>x</span>
            <span>&lambda;</span>
          </span>
          =
          <span class="sv-rpo-matrix sv-rpo-matrix--two">
            <span>A</span><span>-BR<sup>-1</sup>B<sup>T</sup></span>
            <span>0</span><span>-A<sup>T</sup></span>
          </span>
          <span class="sv-rpo-vector sv-rpo-vector--column">
            <span>x</span>
            <span>&lambda;</span>
          </span>
        </div>
        <div class="sv-rpo-formula">x<sub>f</sub> = &Phi;<sub>xx</sub>x<sub>0</sub> + &Phi;<sub>x&lambda;</sub>&lambda;<sub>0</sub></div>
        <div class="sv-rpo-formula">&lambda;<sub>0</sub> = &Phi;<sub>x&lambda;</sub><sup>-1</sup>(x<sub>f</sub> - &Phi;<sub>xx</sub>x<sub>0</sub>)</div>
      </div>
    </div>
    <p class="sv-rpo-equation-note">
      The final simulation summary tracks the conditioning of &Phi;<sub>x&lambda;</sub>; none of the reported cases required the pseudo-inverse fallback.
    </p>
  </div>
</section>

## Simulation Results

The final simulation campaign includes four reference rendezvous scenarios plus a smaller nominal docking case. All cases use the Clohessy-Wiltshire truth model and converge to the requested terminal state.

<div class="sv-rpo-scroll">
  <table class="sv-rpo-table">
    <thead>
      <tr>
        <th>Case</th>
        <th>Final Time</th>
        <th>Final Position Error</th>
        <th>Final Velocity Error</th>
        <th>Peak Control Norm</th>
        <th>Integrated Control Effort</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Case 1</td>
        <td>1000 s</td>
        <td>6.05e-18 m</td>
        <td>1.82e-15 m/s</td>
        <td>2.58e-2 m/s<sup>2</sup></td>
        <td>8.53e-2</td>
      </tr>
      <tr>
        <td>Case 2</td>
        <td>1000 s</td>
        <td>2.27e-13 m</td>
        <td>9.59e-11 m/s</td>
        <td>4.51e-2 m/s<sup>2</sup></td>
        <td>2.55e-1</td>
      </tr>
      <tr>
        <td>Case 3</td>
        <td>1000 s</td>
        <td>8.72e-18 m</td>
        <td>2.62e-15 m/s</td>
        <td>2.59e-2 m/s<sup>2</sup></td>
        <td>8.41e-2</td>
      </tr>
      <tr>
        <td>Case 4</td>
        <td>1500 s</td>
        <td>1.31e-17 m</td>
        <td>6.24e-10 m/s</td>
        <td>4.15e-2 m/s<sup>2</sup></td>
        <td>3.77e-1</td>
      </tr>
      <tr>
        <td>Nominal Docking</td>
        <td>600 s</td>
        <td>1.33e-18 m</td>
        <td>4.00e-16 m/s</td>
        <td>1.74e-3 m/s<sup>2</sup></td>
        <td>3.16e-4</td>
      </tr>
    </tbody>
  </table>
</div>

## Case Studies

Each case below shows the Hill-frame trajectory and the corresponding animation. The state histories, control histories, and terminal error plots are included in expandable result panels so the full data stays available without turning the page into a wall of figures.

<section class="sv-rpo-case">
  <h3>Case 1: Baseline 3D Docking Rendezvous</h3>
  <div class="sv-rpo-case-meta">1000 second horizon, zero terminal relative position and velocity.</div>
  <div class="sv-rpo-media-grid">
    <div>
      <img src="/images/sv-rpo/Paper1_trajectory.png" loading="lazy" alt="Case 1 Hill-frame trajectory.">
      <p class="sv-rpo-caption">The deputy follows a curved 3D approach from the initial state to the target docking condition.</p>
    </div>
    <div>
      <video class="sv-rpo-video" autoplay loop muted playsinline controls>
        <source src="/images/sv-rpo/Paper1_cw_visualization.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="sv-rpo-caption">Animated CW-frame rendezvous visualization.</p>
    </div>
  </div>
  <details class="sv-rpo-details">
    <summary>State, control, and error plots</summary>
    <div class="sv-rpo-detail-grid">
      <img src="/images/sv-rpo/Paper1_states.png" loading="lazy" alt="Case 1 state history.">
      <img src="/images/sv-rpo/Paper1_controls.png" loading="lazy" alt="Case 1 control history.">
      <img src="/images/sv-rpo/Paper1_errors.png" loading="lazy" alt="Case 1 terminal error history.">
    </div>
  </details>
</section>

<section class="sv-rpo-case">
  <h3>Case 2: Offset Terminal State</h3>
  <div class="sv-rpo-case-meta">1000 second horizon, nonzero terminal relative position and velocity.</div>
  <div class="sv-rpo-media-grid">
    <div>
      <img src="/images/sv-rpo/Paper2_trajectory.png" loading="lazy" alt="Case 2 Hill-frame trajectory.">
      <p class="sv-rpo-caption">The guidance law targets an offset final state instead of a pure docking point.</p>
    </div>
    <div>
      <video class="sv-rpo-video" autoplay loop muted playsinline controls>
        <source src="/images/sv-rpo/Paper2_cw_visualization.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="sv-rpo-caption">Animated offset rendezvous visualization.</p>
    </div>
  </div>
  <details class="sv-rpo-details">
    <summary>State, control, and error plots</summary>
    <div class="sv-rpo-detail-grid">
      <img src="/images/sv-rpo/Paper2_states.png" loading="lazy" alt="Case 2 state history.">
      <img src="/images/sv-rpo/Paper2_controls.png" loading="lazy" alt="Case 2 control history.">
      <img src="/images/sv-rpo/Paper2_errors.png" loading="lazy" alt="Case 2 terminal error history.">
    </div>
  </details>
</section>

<section class="sv-rpo-case">
  <h3>Case 3: Alternate 3D Docking Geometry</h3>
  <div class="sv-rpo-case-meta">1000 second horizon, alternate initial geometry with zero terminal relative state.</div>
  <div class="sv-rpo-media-grid">
    <div>
      <img src="/images/sv-rpo/Paper3_trajectory.png" loading="lazy" alt="Case 3 Hill-frame trajectory.">
      <p class="sv-rpo-caption">A different approach geometry checks whether the same guidance law remains well behaved.</p>
    </div>
    <div>
      <video class="sv-rpo-video" autoplay loop muted playsinline controls>
        <source src="/images/sv-rpo/Paper3_cw_visualization.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="sv-rpo-caption">Animated alternate docking visualization.</p>
    </div>
  </div>
  <details class="sv-rpo-details">
    <summary>State, control, and error plots</summary>
    <div class="sv-rpo-detail-grid">
      <img src="/images/sv-rpo/Paper3_states.png" loading="lazy" alt="Case 3 state history.">
      <img src="/images/sv-rpo/Paper3_controls.png" loading="lazy" alt="Case 3 control history.">
      <img src="/images/sv-rpo/Paper3_errors.png" loading="lazy" alt="Case 3 terminal error history.">
    </div>
  </details>
</section>

<section class="sv-rpo-case">
  <h3>Case 4: Long-Horizon Offset Rendezvous</h3>
  <div class="sv-rpo-case-meta">1500 second horizon, offset terminal state over a longer transfer.</div>
  <div class="sv-rpo-media-grid">
    <div>
      <img src="/images/sv-rpo/Paper4_trajectory.png" loading="lazy" alt="Case 4 Hill-frame trajectory.">
      <p class="sv-rpo-caption">The longer-horizon case uses more time to reach a prescribed offset state.</p>
    </div>
    <div>
      <video class="sv-rpo-video" autoplay loop muted playsinline controls>
        <source src="/images/sv-rpo/Paper4_cw_visualization.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="sv-rpo-caption">Animated long-horizon rendezvous visualization.</p>
    </div>
  </div>
  <details class="sv-rpo-details">
    <summary>State, control, and error plots</summary>
    <div class="sv-rpo-detail-grid">
      <img src="/images/sv-rpo/Paper4_states.png" loading="lazy" alt="Case 4 state history.">
      <img src="/images/sv-rpo/Paper4_controls.png" loading="lazy" alt="Case 4 control history.">
      <img src="/images/sv-rpo/Paper4_errors.png" loading="lazy" alt="Case 4 terminal error history.">
    </div>
  </details>
</section>

<section class="sv-rpo-case">
  <h3>Nominal Docking Case</h3>
  <div class="sv-rpo-case-meta">600 second horizon, compact near-origin docking scenario.</div>
  <div class="sv-rpo-media-grid">
    <div>
      <img src="/images/sv-rpo/NominalDock_trajectory.png" loading="lazy" alt="Nominal docking Hill-frame trajectory.">
      <p class="sv-rpo-caption">The nominal case starts much closer to the target and requires the smallest control effort.</p>
    </div>
    <div>
      <video class="sv-rpo-video" autoplay loop muted playsinline controls>
        <source src="/images/sv-rpo/NominalDock_cw_visualization.mp4" type="video/mp4">
        Your browser does not support the video tag.
      </video>
      <p class="sv-rpo-caption">Animated nominal docking visualization.</p>
    </div>
  </div>
  <details class="sv-rpo-details">
    <summary>State, control, and error plots</summary>
    <div class="sv-rpo-detail-grid">
      <img src="/images/sv-rpo/NominalDock_states.png" loading="lazy" alt="Nominal docking state history.">
      <img src="/images/sv-rpo/NominalDock_controls.png" loading="lazy" alt="Nominal docking control history.">
      <img src="/images/sv-rpo/NominalDock_errors.png" loading="lazy" alt="Nominal docking terminal error history.">
    </div>
  </details>
</section>

## Takeaways

The simulations show that the minimum-energy guidance law can hit both docking and offset terminal conditions with extremely small numerical terminal errors. The nominal docking case needs very little acceleration, while the offset cases naturally require larger control effort because the final state is more constrained.

The main lesson is that a compact continuous-time optimal-control derivation can become a practical rendezvous simulation tool: define the target terminal state, select the transfer time, compute the guidance law, and verify the resulting trajectory, controls, and terminal residuals across a family of proximity-operation scenarios.
