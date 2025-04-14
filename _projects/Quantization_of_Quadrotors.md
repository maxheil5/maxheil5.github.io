---
title: Effect of Quantization on Data-Driven Model Predictive Control of Quadcopters
date: 2024-06-05 08:01:35 +0300
subtitle: Control Systems
image: '/images/modalai-inc-drone-drone-only-px4-autonomy-developer-kit-43777520206128_2048x2048.jpg'
---
The following project was my undergraduate research thesis for graduation with honors research distinction. You can find the published paper at: <a href="https://kb.osu.edu/handle/1811/105739">Knowledge Bank, Published 05/25</a>.

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
The methodology is divided into three phases: numerical simulations, Gazebo-based digital twin testing, and hardware-in-the-loop (HITL) flight testing. Each phase builds upon the previous one to validate both theoretical and experimental performance under quantized data conditions.

### Numerical Simulations
The first stage implements the full control and learning pipeline in MATLAB, allowing for controlled experimentation and detailed analysis.

<ul>
  <li>Trajectory Generation: Random quadrotor trajectories are generated using a nonlinear PID-based simulator. These trajectories include position, velocity,           orientation, and angular velocity states, as well as corresponding control inputs.</li>
  <li>System Identification: </li>
</ul>

## Results

<div class="gallery-box">
  <div class="gallery">
    <img src="/images/project-example-5.jpg" loading="lazy" alt="Project">
  </div>
  <em>Photo by <a href="https://unsplash.com/@rpnickson">Roberto Nickson</a> on <a href="https://unsplash.com/">Unsplash</a></em>
</div>
