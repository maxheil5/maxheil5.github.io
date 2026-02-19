---
title: Search & Reacquisition of Resident Space Object
date: 2024-08-28 08:01:35 +0300
subtitle: Space Domain Awareness
image: '/images/SRA_Cover_Image_Horizontal_V2.png'
featured: true
---
This project presents a complete multi-stage kill chain for search and reacquisition of resident space objects (RSOs), built and presented at the AIAA Region III Student Conference.

The framework combines machine learning, orbit determination, probabilistic propagation, and filtering to reacquire an object on a second pass after initial ground-based sensing.

See the conference presentation here:
<p>
  <a href="/files/AIAA Presentation V2.pptx" target="_blank">
    <i class="fa fa-file-pdf-o"></i> AIAA Presentation (.pptx)
  </a>
</p>

## Space Domain Awareness Context
Low Earth Orbit is increasingly congested. The presentation and manuscript frame the problem around custody of active payloads, debris, and other anthropogenic objects under sensor uncertainty, limited coverage, and noisy measurements.

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_space_tracking_example.jpg" loading="lazy" alt="Example of tracking resident space objects in orbit.">
    <img src="/images/SRA_space_debris_example.png" loading="lazy" alt="Example of the growing space debris challenge in Earth orbit.">
  </div>
  <em>SDA challenge context: increasing orbital congestion and tracking complexity.</em>
</div>

## Objective and Kill Chain Layout
Goal: reacquire an initially tracked object on a second pass with high confidence.

Pipeline used (slide-ordered):
1. AI launch detection and rocket classification
2. Initial Orbit Determination (IOD) from first-pass observations
3. Orbit refinement using Gaussian Least Squares Differential Correction (GLSDC)
4. Adaptive Monte Carlo (AMC) propagation of uncertainty cloud
5. Discrete-time linear Kalman filtering and final orbit comparison

<div class="gallery-box">
  <div class="gallery gallery-columns-1">
    <img src="/images/SRA_kill_chain.png" loading="lazy" alt="Multi-stage kill chain for search and reacquisition of resident space objects.">
  </div>
  <em>End-to-end reacquisition framework used in the project.</em>
</div>

## Phase 1 - Launch Detection Using AI
Two CNN-based models were developed:
1. Launch pad occupancy detection (rocket present vs empty)
2. Rocket type classification

Training/validation split from manuscript:
1. 80% training
2. 20% validation

Reported model performance:
1. Launch pad detection accuracy: 73.33%
2. Rocket body classification accuracy: 30.56%

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/SRA_AI_Workflow.png" loading="lazy" alt="AI workflow for launch detection and rocket classification.">
    <img src="/images/SRA_AI_Launch_Detection.png" loading="lazy" alt="Launch pad occupancy detection output from AI model.">
    <img src="/images/SRA_AI_Rocket_Type_Detection.png" loading="lazy" alt="Rocket type classification output from AI model.">
  </div>
  <em>AI phase outputs used to trigger and condition downstream orbit estimation.</em>
</div>

## Phase 2 - Initial Orbit Determination (IOD)
IOD uses Gauss angles-only logic from first-pass radar geometry to estimate state and orbital elements.

ISS test-case assumptions from manuscript:
1. Observation site: AMOS (Maui)
2. 100 synthetic observations generated
3. Measurement noise injected at approximately +/-5 km range and +/-0.05 deg azimuth/elevation

True test-case orbital elements:
1. Semi-major axis: 12000 km
2. Eccentricity: 0.12
3. Inclination: 11 deg
4. RAAN: 220 deg
5. Argument of periapsis: 42 deg
6. True anomaly: 280 deg

Example IOD estimate reported:
1. Semi-major axis: 11953 km
2. Eccentricity: 0.19334
3. Inclination: 14 deg
4. RAAN: 257 deg
5. Argument of periapsis: 41 deg
6. True anomaly: 299 deg

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/SRA_IOD_ISS_Test_Case_First_Pass.png" loading="lazy" alt="IOD ISS test case first-pass orbit view.">
    <img src="/images/SRA_IOD_ISS_Test_Case_First_Pass_Obsv_points.png" loading="lazy" alt="IOD first-pass observation points from radar geometry.">
    <img src="/images/SRA_IOD_ISS_Test_Case_First_Pass_comparison.png" loading="lazy" alt="Comparison plot for first-pass IOD estimate versus reference orbit.">
  </div>
  <em>First-pass IOD outputs and comparisons.</em>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/SRA_IOD_ISS_Test_Case_Second_Pass.png" loading="lazy" alt="IOD ISS test case second-pass orbit view.">
    <img src="/images/SRA_IOD_ISS_Test_Case_Second_Pass_Obsv_points.png" loading="lazy" alt="IOD second-pass observation points from radar geometry.">
    <img src="/images/SRA_IOD_ISS_Test_Case_Second_Pass_Comparison.png" loading="lazy" alt="Comparison plot for second-pass IOD estimate versus reference orbit.">
  </div>
  <em>Second-pass IOD outputs and comparisons.</em>
</div>

## Phase 3 - Orbit Refinement (GLSDC)
GLSDC iteratively refines the initial state by minimizing residual error between predicted and observed measurements using a least-squares correction process and measurement sensitivity (Jacobian) information.

Observed manuscript behavior in this implementation:
1. GLSDC ran for 54 iterations
2. The run diverged to NaN in the presented test case
3. Additional numerical/debug work was identified as future improvement

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/SRA_GLSDC_Workflow.png" loading="lazy" alt="GLSDC workflow for orbit refinement in the kill chain.">
    <img src="/images/SRA_GLSDC_First_Pass_Workflow.png" loading="lazy" alt="GLSDC first-pass workflow diagram.">
    <img src="/images/SRA_GLSDC_Second_Pass_Workflow.png" loading="lazy" alt="GLSDC second-pass workflow diagram.">
  </div>
  <em>GLSDC processing flow across first and second pass.</em>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_GLSDC_ISS_Test_Case_First_Pass_Orbit_Comparison.png" loading="lazy" alt="GLSDC first-pass orbit comparison for ISS test case.">
    <img src="/images/SRA_GLSDC_ISS_Test_Case_First_Pass_Parameters.png" loading="lazy" alt="GLSDC first-pass orbital parameter comparison.">
    <img src="/images/SRA_GLSDC_ISS_Test_Case_Second_Pass_Orbit_Comparison.png" loading="lazy" alt="GLSDC second-pass orbit comparison for ISS test case.">
    <img src="/images/SRA_GLSDC_ISS_Test_Case_Second_Pass_Parameters.png" loading="lazy" alt="GLSDC second-pass orbital parameter comparison.">
  </div>
  <em>GLSDC orbit and element comparisons for both passes.</em>
</div>

## Phase 4 - Adaptive Monte Carlo Propagation (AMC)
AMC propagates a probabilistic cloud of states forward in time under uncertainty. The implementation adaptively adjusts sample count based on variance behavior to balance accuracy and computational cost.

Key manuscript/presentation points:
1. Point cloud begins approximately Gaussian
2. Cloud shape evolves nonlinearly over time
3. Additional particles are generated when uncertainty thresholds are exceeded
4. Accuracy improves with larger sample counts

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_AMC_Workflow.png" loading="lazy" alt="Adaptive Monte Carlo workflow for uncertainty propagation.">
    <img src="/images/SRA_AMC_Accuracy_Plot.png" loading="lazy" alt="AMC accuracy trend with increasing number of particles.">
  </div>
  <em>AMC workflow and particle-count accuracy trend.</em>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_AMC_ISS_Test_Case_Original_Position.png" loading="lazy" alt="Original AMC point cloud in position state space.">
    <img src="/images/SRA_AMC_ISS_Test_Case_Propagated_Position.png" loading="lazy" alt="Propagated AMC point cloud in position state space.">
  </div>
  <em>Position cloud before and after AMC propagation.</em>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_AMC_ISS_Test_Case_Original_Velocity.png" loading="lazy" alt="Original AMC point cloud in velocity state space.">
    <img src="/images/SRA_AMC_ISS_Test_Case_Propagated_Velocity.png" loading="lazy" alt="Propagated AMC point cloud in velocity state space.">
  </div>
  <em>Velocity cloud before and after AMC propagation.</em>
</div>

## Phase 5 - Kalman Filtering and Reacquisition Test
A discrete-time linear Kalman Filter fuses:
1. AMC propagated mean/covariance
2. second-pass state estimate
3. sensor noise model

Expected output:
1. updated state vector
2. reduced covariance
3. improved confidence for same-object determination

Presentation notes:
1. position/orbit agreement looked strong
2. velocity showed signs of overcorrection (identified future work)

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_Kalman_Filtering_Workflow.png" loading="lazy" alt="Kalman filtering workflow used in second-pass reacquisition.">
    <img src="/images/SRA_KF_ISS_Test_Case_Position_Comparison.png" loading="lazy" alt="Kalman filter position comparison for ISS test case.">
    <img src="/images/SRA_KF_ISS_Test_Case_Velocity_Comparison.jpg" loading="lazy" alt="Kalman filter velocity comparison for ISS test case.">
    <img src="/images/SRA_KF_ISS_Test_Case_Orbit_Comparison.png" loading="lazy" alt="Kalman filter orbit comparison for ISS test case.">
  </div>
  <em>Kalman filter results for second-pass validation.</em>
</div>

## TLE Comparison Criteria and Decision Logic
The manuscript compares first-pass, second-pass, and posterior TLE values using thresholds derived from historical ISS TLE analysis.

Reported matching thresholds:
1. Semi-major axis: 0.0 km
2. Eccentricity: 0.014
3. Inclination: 0.007 deg
4. RAAN: 6.283 deg
5. Argument of periapsis: 6.291 deg

Using these criteria, the project reports successful same-object classification for the ISS two-pass simulation case.

## Final Outcome
The end-to-end framework demonstrated:
1. successful object reacquisition in the simulated ISS two-pass case
2. operational feasibility of a multi-stage autonomous reacquisition pipeline
3. clear next steps for improving robustness with real sensor data and strengthened GLSDC convergence

## Future Work
High-priority items from the presentation/manuscript:
1. incorporate real sensor feedback and SSN-like observation streams
2. improve AI launch and rocket-classification performance
3. stabilize and validate GLSDC convergence behavior
4. expand to maneuvering or low-thrust targets
5. evaluate EKF/UKF and full automated closed-loop kill chain execution

<div class="gallery-box">
  <div class="gallery gallery-columns-1">
    <img src="/images/SRA_Future_Work_Example.png" loading="lazy" alt="Future work map for next-stage SDA and orbit determination enhancements.">
  </div>
  <em>Future expansion paths for SDA-focused reacquisition capability.</em>
</div>

## Acknowledgements
This project was supported through Ohio State MAE and collaborators including LADDCS and ARC resources.

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/SRA_LADDCS_Laboratory.png" loading="lazy" alt="LADDCS laboratory acknowledgment graphic.">
    <img src="/images/SRA_OSU_ARC_Facility.png" loading="lazy" alt="OSU Aerospace Research Center acknowledgment graphic.">
  </div>
  <em>Contributing research facilities and lab support.</em>
</div>
