---
title: Effect of Quantization on Data-Driven Model Predictive Control of Quadcopters
date: 2024-06-05 08:01:35 +0300
subtitle: Control Systems
image: '/images/modalai-inc-drone-drone-only-px4-autonomy-developer-kit-43777520206128_2048x2048.jpg'
---
The following project was my undergraduate research thesis for graduation with honors research distinction. You can find the published paper at: <a href="https://kb.osu.edu/handle/1811/105739">Knowledge Bank, Published 05/25</a>. The project has since evolved to be my graduate research project. Updates to the material are currently still being incorporated.

This research project explores how quantized communication effects data-driven control of unmanned aerial vehicles (UAVs), focusing specifically on quadcopters. By integrating dither quantization into a Koopman operator-based Model Predictive Control (MPC) framework, the project investigates how reduced word length use conserves bandwidth and impacts system identification and flight performance. Using a multi-stage validation process including MATLAB simulations, Gazebo-based digital twin testing, and real-world experiments with the PX4 Starling Developer Kit, this work aims to bridge the gap between theoretical control models and their deployment on resource-constrained aerial platforms. The findings offer practical insights for designing reliable, low-bandwidth control systems in next-generation UAVs.

## Background
Modern quadcopters are becoming increasingly reliant on data-driven control strategies to maintain stability and performance in complex environments. However, these systems are often deployed on embedded platforms with limited computational power, memory, and bandwidth, which poses challenges for real-time communication and control. One common method to address these constraints is quantization, the process of converting high-resolution continuous data into lower-bit discrete representations. While this technique reduces data transmission costs, it introduces quantization error, which can distort system identification and degrade control performance.

To mitigate this, dither quantization introduces random noise before quantizing and removes it afterward, helping to decorrelate the error from the signal and reduce bias. In this project, dither quantization is applied to both state and control data before they are used in training and deploying a Koopman-based MPC. The Koopman operator provides a linear surrogate model of the inherently nonlinear quadrotor dynamics by lifting the system into a higher-dimensional space using Extended Dynamic Mode Decomposition (EDMD). This allows for efficient real-time control even when the input data is coarsely quantized.

By systematically analyzing how quantization resolution impacts model accuracy and tracking performance, this work contributes to a growing body of research focused on deploying advanced control algorithms in low-resource environments.

## Overview
Below are the key technical foundations that this research builds upon.

### What is Quantization?
Quantization is the process of mapping a large set of input values to a smaller set of output values. This is critical in embedded systems where memory, processing power, or bandwidth are limited.

A <strong>uniform quantizer</strong> with resolution <math><mi>&#x03B5;</mi></math> maps a real number <math><mi>x</mi></math> to:

<math display="block">
  <mrow>
    <mi>q</mi><mo>(</mo><mi>x</mi><mo>)</mo><mo>=</mo>
    <mo>&#x230A;</mo>
    <mfrac>
      <mrow><mi>x</mi><mo>&#x2212;</mo><msub><mi>x</mi><mi>min</mi></msub></mrow>
      <mi>&#x03B5;</mi>
    </mfrac>
    <mo>&#x230B;</mo>
  </mrow>
</math>

The resolution <math><mi>&#x03B5;</mi></math> is based on bit-depth <math><mi>b</mi></math> and the signal range:

<math display="block">
  <mrow>
    <mi>&#x03B5;</mi><mo>=</mo>
    <mfrac>
      <mrow>
        <msub><mi>x</mi><mi>max</mi></msub>
        <mo>&#x2212;</mo>
        <msub><mi>x</mi><mi>min</mi></msub>
      </mrow>
      <msup><mn>2</mn><mi>b</mi></msup>
    </mfrac>
  </mrow>
</math>

In <strong>dither quantization</strong>, noise <math><mi>w</mi></math> is added before quantizing and subtracted after:

<math display="block">
  <mrow>
    <mover>
      <mi>x</mi>
      <mo>&#x007E;</mo>
    </mover>
    <mo>=</mo>
    <mi>Q</mi><mo>(</mo><mi>x</mi><mo>+</mo><mi>w</mi><mo>)</mo><mo>&#x2212;</mo><mi>w</mi>
  </mrow>
</math>

This technique helps ensure that the quantization error is independent from the signal, which improves learning and control.

An example of quantization in image signal processing can be seen below. The image on the left shows the original image of a cat with full 24-bit RGB color array. A quantized version, down to 16 colors, is shown in the middle. And finally, a dither quantized version of the same image can be seen on the right.

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/cat_example_original.png" loading="lazy" alt="Project">
    <img src="/images/cat_example_quantized.png" loading="lazy" alt="Project">
    <img src="/images/cat_example_unquantized.png" loading="lazy" alt="Project">
  </div>
  <em>Left: Original Image / Middle: 16 Color Quantized / Right: Dither Quantized Version / Image Credit: <a href="https://en.wikipedia.org/wiki/Color_quantization">Wikepedia</a></em>
</div>

While the quantized version of the image does degrade the RGB signals, you can still understand the image and background. When dithering, the error becomes independent of the true signal, and the colors are smoothed and banding is visibly reduced.

This example motivates the idea behind using this architecture in drone controls.

## Koopman-Operator Theory
The <strong>Koopman operator</strong> lets us represent nonlinear systems as linear ones in a lifted feature space. For a system

<math display="block">
  <mrow>
    <msub><mi>x</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub>
    <mo>=</mo>
    <mi>f</mi><mo>(</mo><msub><mi>x</mi><mi>t</mi></msub><mo>)</mo>
  </mrow>
</math>

We define observables <math><mi>&#x03D5;</mi><mo>(</mo><mi>x</mi><mo>)</mo></math>, and apply the Koopman operator <math><mi>&#x1D4AA;</mi></math> as:

<math display="block">
  <mrow>
    <mi>&#x1D4AA;</mi><mo>&#x2061;</mo><mi>&#x03D5;</mi><mo>(</mo><mi>x</mi><mo>)</mo>
    <mo>=</mo>
    <mi>&#x03D5;</mi><mo>(</mo><mi>f</mi><mo>(</mo><mi>x</mi><mo>)</mo><mo>)</mo>
  </mrow>
</math>

This transforms nonlinear dynamics into linear evolution in a higher-dimensional space.

## Model Predictive Control (MPC)
<strong>Model Predictive Control</strong> (MPC) computes optimal control actions by solving a constrained optimization problem over a future horizon. The controller solves:

<math display="block">
  <mrow>
    <mo>minimize</mo>
    <munder>
      <mrow>
        <mo>{</mo><msub><mi>u</mi><mi>t</mi></msub><mo>}</mo>
      </mrow>
      <mrow></mrow>
    </munder>
    <mo>&#x2211;</mo><msub><mi>t</mi><mi>=0</mi></msub><msup><mi>T</mi><mi>h</mi></msup>
    <mo>(</mo>
    <msup>
      <mrow>
        <mo>&#x2225;</mo><mi>x</mi><mo>&#x2212;</mo><msub><mi>x</mi><mi>ref</mi></msub><mo>&#x2225;</mo>
      </mrow>
      <mn>2</mn>
    </msup>
    <mo>+</mo>
    <msup>
      <mo>&#x2225;</mo><mi>u</mi><mo>&#x2225;</mo>
      <mn>2</mn>
    </msup>
    <mo>)</mo>
  </mrow>
</math>

subject to:

<math display="block">
  <mrow>
    <msub><mi>x</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub>
    <mo>=</mo>
    <mi>f</mi><mo>(</mo><msub><mi>x</mi><mi>t</mi></msub><mo>,</mo><msub><mi>u</mi><mi>t</mi></msub><mo>)</mo>
  </mrow>
</math>

This formulation allows real-time optimization of control inputs under constraints.

### Extended Dynamic Mode Decomposition (EDMD)
To build a Koopman model from data, we use <strong>Extended Dynamic Mode Decomposition with Control (EDMDc)</strong>. Given snapshots of data, we solve:

<math display="block">
  <mrow>
    <mi>&#x03D5;</mi><mo>(</mo><msub><mi>x</mi><mrow><mi>t</mi><mo>+</mo><mn>1</mn></mrow></msub><mo>)</mo>
    <mo>&#x2248;</mo>
    <mi>A</mi><mi>&#x03D5;</mi><mo>(</mo><msub><mi>x</mi><mi>t</mi></msub><mo>)</mo><mo>+</mo><mi>B</mi><msub><mi>u</mi><mi>t</mi></msub>
  </mrow>
</math>

Matrices <math><mi>A</mi></math> and <math><mi>B</mi></math> are learned from data and represent a linear model in the lifted space defined by <math><mi>&#x03D5;</mi><mo>(</mo><mi>x</mi><mo>)</mo></math>.

## Methods
The workflow follows the SciTech 2026 manuscript setup: identify a Koopman linear predictor with EDMDc from quantized state/input data, then deploy that predictor inside linear MPC and evaluate prediction and tracking performance versus quantization word-length.

### Data-Driven Identification Setup
The quadrotor dynamics are modeled on <strong>SE(3)</strong> and lifted with the EDMDc dictionary used in the paper, with <math><mi>q</mi><mo>=</mo><mn>3</mn></math>, giving a lifted observable dimension of 51.

For system identification, the paper uses:
<ul>
  <li><strong>100 independent training trajectories</strong> (<math><mi>N</mi><msub><mi>traj</mi></msub><mo>=</mo><mn>100</mn></math>)</li>
  <li><strong>Trajectory duration</strong> <math><mi>T</mi><mo>=</mo><mn>0.1</mn><mi>s</mi></math></li>
  <li><strong>Sampling period</strong> <math><mo>&#x0394;</mo><mi>t</mi><mo>=</mo><msup><mn>10</mn><mrow><mo>&#x2212;</mo><mn>3</mn></mrow></msup><mi>s</mi></math></li>
  <li><strong>Initial state</strong> fixed at <math><mi>s</mi><msub><mi>0</mi></msub><mo>=</mo><mn>0</mn></math>, <math><mi>v</mi><msub><mi>0</mi></msub><mo>=</mo><mn>0</mn></math>, <math><mi>R</mi><msub><mi>0</mi></msub><mo>=</mo><msub><mi>I</mi><mn>3</mn></msub></math>, and <math><mi>&#x03C9;</mi><msub><mi>0</mi></msub><mo>=</mo><mo>[</mo><mn>0.1</mn><mo>,</mo><mn>0</mn><mo>,</mo><mn>0</mn><msup><mo>]</mo><mi>T</mi></msup></math></li>
  <li><strong>Control input excitation</strong> sampled as zero-mean Gaussian with diagonal covariance <math><mi>diag</mi><mo>(</mo><mn>10</mn><mo>,</mo><mn>10</mn><mo>,</mo><mn>10</mn><mo>,</mo><mn>10</mn><mo>)</mo></math></li>
</ul>

Unquantized stacked snapshots are used once to compute the baseline ("ground truth") Koopman predictor matrices.

### Dither Quantization Campaign
To evaluate bandwidth-constrained learning, dither quantization is applied <strong>entry-wise</strong> to both state and control measurements before identification. Quantizer bounds are built from min/max values in unquantized training data and slightly inflated to avoid saturation under dither.

For each tested word-length, the full identification/prediction/MPC pipeline is repeated across independent dither draws:
<ul>
  <li><strong>Word-lengths:</strong> 4, 8, 12, 14, and 16 bits</li>
  <li><strong>Dither realizations per word-length:</strong> 50</li>
</ul>

### MPC and Evaluation Metrics
The learned predictor is embedded in linear MPC with:
<ul>
  <li><strong>Prediction horizon:</strong> 10 steps (<math><mn>0.01</mn><mi>s</mi></math>)</li>
  <li><strong>Simulation window:</strong> <math><mn>1.2</mn><mi>s</mi></math></li>
  <li><strong>Input penalty:</strong> <math><mi>R</mi><mo>=</mo><mi>diag</mi><mo>(</mo><msup><mn>10</mn><mrow><mo>&#x2212;</mo><mn>6</mn></mrow></msup><mo>,</mo><mn>1</mn><mo>,</mo><mn>1</mn><mo>,</mo><mn>1</mn><mo>)</mo></math></li>
  <li><strong>Input constraints:</strong> each control channel bounded in <math><mo>[</mo><mo>&#x2212;</mo><mn>50</mn><mo>,</mo><mn>50</mn><mo>]</mo></math> (no saturation observed in reported runs)</li>
</ul>

The paper reports four primary metrics:
<ul>
  <li>Relative Koopman matrix error in <math><mi>A</mi></math>: <math><mfrac><mrow><mo>&#x2225;</mo><mi>A</mi><mo>&#x2212;</mo><mover><mi>A</mi><mo>&#x007E;</mo></mover><mo>&#x2225;</mo></mrow><mrow><mo>&#x2225;</mo><mi>A</mi><mo>&#x2225;</mo></mrow></mfrac></math></li>
  <li>Relative Koopman matrix error in <math><mi>B</mi></math>: <math><mfrac><mrow><mo>&#x2225;</mo><mi>B</mi><mo>&#x2212;</mo><mover><mi>B</mi><mo>&#x007E;</mo></mover><mo>&#x2225;</mo></mrow><mrow><mo>&#x2225;</mo><mi>B</mi><mo>&#x2225;</mo></mrow></mfrac></math></li>
  <li>One-step position prediction RMSE</li>
  <li>Closed-loop MPC position tracking RMSE</li>
</ul>

## Results
Across all experiments in the paper, lower word-length increased identification, prediction, and tracking error, while higher word-length consistently recovered performance.

<div class="gallery-box">
  <div class="gallery gallery-columns-4">
    <img src="/images/A_Matrix_Difference_vs_Word_Length.png" loading="lazy" alt="Relative A-matrix identification error versus quantization word-length.">
    <img src="/images/B_Matrix_Difference_vs_Word_Length.png" loading="lazy" alt="Relative B-matrix identification error versus quantization word-length.">
    <img src="/images/Prediction_Error_vs_Word_Length.png" loading="lazy" alt="One-step position prediction error versus quantization word-length.">
    <img src="/images/MPC_Position_Tracking_Error_vs_Word_Length.png" loading="lazy" alt="Closed-loop MPC position tracking error versus quantization word-length.">
  </div>
  <em>Quantitative summary across word-length: relative Koopman matrix errors (A and B), one-step prediction RMSE, and closed-loop MPC position tracking RMSE.</em>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-4">
    <img src="/images/4_bit_50_signals_legend_latex_units.png" loading="lazy" alt="4-bit MPC position trajectories over 50 dither realizations.">
    <img src="/images/8_bit_50_signals_legend_latex_units.png" loading="lazy" alt="8-bit MPC position trajectories over 50 dither realizations.">
    <img src="/images/12_bit_50_signals_legend_latex_units.png" loading="lazy" alt="12-bit MPC position trajectories over 50 dither realizations.">
    <img src="/images/16_bit_50_signals_legend_latex_units.png" loading="lazy" alt="16-bit MPC position trajectories over 50 dither realizations.">
  </div>
  <em>Position tracking trajectories under Koopman MPC with dither-quantized measurements for (a) 4-bit, (b) 8-bit, (c) 12-bit, and (d) 16-bit word-lengths. Each panel shows 50 independent dither realizations.</em>
</div>

These plots capture the same trend reported in the paper: identification error in both <math><mi>A</mi></math> and <math><mi>B</mi></math> decreases with increasing word-length, and that improvement carries through to both prediction and closed-loop tracking. The clearest transition occurs from 8 to 12 bits, after which additional gains are smaller.

Quantitative results:
<ul>
  <li>Relative errors in the identified lifted matrices decrease as word-length increases; at 4 bits, both median error and spread are substantially larger than higher-resolution cases.</li>
  <li>One-step prediction RMSE follows the same trend: 12-16 bit predictors remain low-error with weak variation, while coarse quantization shows visibly larger error and variability across dither realizations.</li>
  <li>Closed-loop tracking shows the strongest transition between 8 and 12 bits. The paper reports the median MPC position RMSE dropping by roughly a factor of 3-4 from 8 to 12 bits, with a much tighter spread across realizations.</li>
  <li>Beyond ~12 bits, gains from additional precision are modest; the reported practical trade regime is about <strong>12-14 bits</strong>, where performance is close to the unquantized baseline with much lower bandwidth per sample.</li>
</ul>
