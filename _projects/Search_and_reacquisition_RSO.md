---
title: Search & Reacquisition of Resident Space Object
date: 2024-08-28 08:01:35 +0300
subtitle: Space Domain Awareness
image: '/images/SRA_Cover_Image_Horizontal_V2.png'
---
As space becomes increasingly congested, maintaining custody of objects in orbit—such as satellites, rocket bodies, and debris—is critical for national security and safe space operations. Our research focuses on the search and reacquisition problem, which aims to accurately predict and relocate space objects after they’ve been initially observed.

By combining artificial intelligence, probabilistic orbit determination, and advanced filtering methods, we’ve developed a multi-stage framework capable of identifying rocket launches, estimating initial orbits from ground-based radar, and tracking those objects over time despite sensor noise and uncertainty. This system leverages Adaptive Monte Carlo (AMC) simulations and Kalman Filters to propagate orbital uncertainty and validate reacquisition on future passes. The result: a scalable approach to Space Domain Awareness (SDA) that improves the reliability of space tracking and decision-making.

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/AMC_prop.jpg" loading="lazy" alt="Project">
  </div>
  <em>AMC Propagation</em>
</div>

See the presentation we gave at the AIAA Region III Student Conference in Cincinnati, OH on April 5, 2025:
<p>
  <a href="/files/AIAA Presentation V2.pptx" target="_blank">
    <i class="fa fa-file-pdf-o"></i> AIAA Presentation (.ppt)
  </a>
</p>

## Methods

This page provides an in‐depth overview of the framework for the search and reacquisition of resident space objects. It highlights the growing challenges in Low Earth Orbit—such as sensor noise, limited detection ranges, and the inherent uncertainties in orbit tracking—that complicate the task of reacquiring satellites or rocket bodies after their initial detection.

Here are a few more specifcs about the framework:

• An AI-driven approach for launch detection and rocket classification, which uses machine learning models to automatically identify imminent launches and categorize rocket bodies.

• Initial Orbit Determination (IOD) using Gauss’s angles-only method to convert limited radar observations into a first estimate of the orbital elements.

• Refinement of the initial orbit through Gaussian Least Squares Differential Correction (GLSDC) and Adaptive Monte Carlo (AMC) simulations, which work together to minimize errors and propagate the orbit forward while accounting for uncertainties.

• The application of a discrete-time Kalman Filter in a second observation pass to further refine the orbital state and confirm the reacquisition of the object.

## Results

To validate this kill-chain system, we use the ISS in orbit around Earth as a test case. This is a well known orbit, thus it is easy to compare true results with our experimental results. Each stage of the kill-chain is tested individually and we conclude with a binary yes/no if the object was reacquired on a second pass.

# Phase 1 - <em>IOD<em>:
In this phase


<div class="gallery-box">
  <div class="gallery">
    <img src="/images/project-example-5.jpg" loading="lazy" alt="Project">
  </div>
  <em>Photo by <a href="https://unsplash.com/@rpnickson">Roberto Nickson</a> on <a href="https://unsplash.com/">Unsplash</a></em>
</div>

## Benefits of creating a mobile app

The evolution of design and development is a continuous journey, marked by exploration, experimentation, and growth. Emerging technologies, tools, and trends open new possibilities, empowering designers and developers to push the boundaries of what's possible. Embracing change and embracing a mindset of lifelong learning is essential, ensuring that professionals stay ahead of the curve in a rapidly evolving industry.

Ultimately, the fusion of design and development is more than just a collaboration; it’s a symbiotic relationship that fuels progress and innovation. By working in harmony, designers and developers can overcome challenges, seize opportunities, and create solutions that truly make a difference. As we look to the future, the potential for what can be achieved through this partnership is boundless, promising a world where technology and creativity coexist to improve the human experience.
