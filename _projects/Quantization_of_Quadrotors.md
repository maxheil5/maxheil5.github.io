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



## Branding and recognition

At the heart of every successful design and development endeavor lies the user experience. Understanding the needs, desires, and behaviors of the end user is paramount, guiding every decision and iteration throughout the design and development process. User research and testing provide invaluable insights, illuminating the path forward and ensuring that products resonate with their intended audience.

<div class="gallery-box">
  <div class="gallery">
    <img src="/images/project-example-5.jpg" loading="lazy" alt="Project">
  </div>
  <em>Photo by <a href="https://unsplash.com/@rpnickson">Roberto Nickson</a> on <a href="https://unsplash.com/">Unsplash</a></em>
</div>

## Benefits of creating a mobile app

The evolution of design and development is a continuous journey, marked by exploration, experimentation, and growth. Emerging technologies, tools, and trends open new possibilities, empowering designers and developers to push the boundaries of what's possible. Embracing change and embracing a mindset of lifelong learning is essential, ensuring that professionals stay ahead of the curve in a rapidly evolving industry.

Ultimately, the fusion of design and development is more than just a collaboration; it’s a symbiotic relationship that fuels progress and innovation. By working in harmony, designers and developers can overcome challenges, seize opportunities, and create solutions that truly make a difference. As we look to the future, the potential for what can be achieved through this partnership is boundless, promising a world where technology and creativity coexist to improve the human experience.
