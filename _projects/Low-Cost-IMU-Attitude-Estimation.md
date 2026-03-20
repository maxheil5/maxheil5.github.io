---
title: "Low-Cost IMU Attitude Estimation"
date: 2024-12-02 12:00:00 -0500
subtitle: Extended Kalman Filtering • Sensor Fusion • Embedded Prototyping
description: "A low-cost IMU attitude-estimation project using an Arduino UNO, a BNO055 9-axis IMU, MATLAB, and a quaternion-based extended Kalman filter to estimate roll and pitch."
image: '/images/imu-attitude-estimation/hardware-photo.jpg'
featured: false
---
<style>
  .imu-project-intro {
    margin: 0 0 2rem;
    padding: 1.75rem;
    border: 1px solid var(--border-color);
    border-radius: 24px;
    background: linear-gradient(135deg, rgba(34, 151, 193, 0.12), rgba(40, 65, 111, 0.08));
  }

  .imu-project-intro p:last-child,
  .imu-project-card p:last-child,
  .imu-project-note p:last-child {
    margin-bottom: 0;
  }

  .imu-project-stats,
  .imu-project-grid {
    display: grid;
    gap: 1rem;
    margin: 1.5rem 0 0;
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .imu-project-stats {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }

  .imu-project-stat,
  .imu-project-card,
  .imu-project-note {
    padding: 1rem 1.1rem;
    border: 1px solid var(--border-color);
    border-radius: 20px;
    background: var(--background-alt-color);
  }

  .imu-project-stat strong {
    display: block;
    margin-bottom: 0.25rem;
    color: var(--heading-font-color);
    font-size: 1.85rem;
    line-height: 1;
  }

  .imu-project-stat span,
  .imu-project-note {
    color: var(--text-alt-color);
  }

  .imu-project-links {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
    margin-top: 1rem;
  }

  .imu-project-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.85rem 1rem;
    border: 1px solid var(--border-color);
    border-radius: 999px;
    background: var(--background-color);
    text-decoration: none;
  }

  .imu-project-scroll {
    overflow-x: auto;
    margin: 1.5rem 0;
  }

  .imu-project-table {
    width: 100%;
    min-width: 560px;
    border-collapse: collapse;
    font-size: 0.96rem;
  }

  .imu-project-table th,
  .imu-project-table td {
    padding: 0.85rem 0.95rem;
    border-bottom: 1px solid var(--border-color);
    text-align: left;
    vertical-align: top;
  }

  .imu-project-table th {
    color: var(--heading-font-color);
    background: rgba(34, 151, 193, 0.08);
  }

  .imu-project-equation {
    margin: 1.5rem 0;
    padding: 1.25rem 1.4rem;
    border: 1px solid var(--border-color);
    border-radius: 22px;
    background: linear-gradient(180deg, rgba(40, 65, 111, 0.05), rgba(34, 151, 193, 0.09));
    overflow-x: auto;
  }

  .imu-project-equation__line {
    min-width: max-content;
    font-family: "Old Standard TT", serif;
    font-size: clamp(1.05rem, 2.4vw, 1.45rem);
    line-height: 1.6;
    color: var(--heading-font-color);
  }

  .imu-project-equation__note {
    margin-top: 0.55rem;
    color: var(--text-alt-color);
    font-size: 0.94rem;
  }

  @media (max-width: 900px) {
    .imu-project-stats {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }
  }

  @media (max-width: 600px) {
    .imu-project-stats,
    .imu-project-grid {
      grid-template-columns: minmax(0, 1fr);
    }

    .imu-project-intro,
    .imu-project-stat,
    .imu-project-card,
    .imu-project-note,
    .imu-project-equation {
      border-radius: 16px;
    }
  }
</style>

<div class="imu-project-intro">
  <p>
    This project focused on improving orientation estimates from a low-cost IMU using an <strong>Extended Kalman Filter (EKF)</strong>. I worked with <strong>Kyle Frith</strong> to connect a <strong>BNO055 9-axis IMU</strong> to an <strong>Arduino UNO</strong>, bring the data into MATLAB, and use a quaternion-based filter to estimate roll and pitch more cleanly than raw sensor measurements alone.
  </p>
  <p>
    The hardware was intentionally simple and inexpensive, which made the real question more interesting: how much performance can you get out of modest hardware if the estimation logic is thoughtful? The result was a practical attitude-estimation setup that handled low-frequency motion well, while also making the limits of the sampling rate and sensor responsiveness very clear during faster maneuvers.
  </p>

  <div class="imu-project-stats">
    <div class="imu-project-stat">
      <strong>9-axis</strong>
      <span>BNO055 IMU with onboard sensing and fusion support</span>
    </div>
    <div class="imu-project-stat">
      <strong>4-state</strong>
      <span>quaternion state used in the EKF implementation</span>
    </div>
    <div class="imu-project-stat">
      <strong>2 tests</strong>
      <span>low-frequency 10 s runs and faster 2 s runs</span>
    </div>
    <div class="imu-project-stat">
      <strong>MATLAB</strong>
      <span>used for acquisition, covariance tuning, and plotting</span>
    </div>
  </div>

  <div class="imu-project-links">
    <a class="imu-project-link" href="/files/imu-attitude-estimation/Low_Cost_IMU_Attitude_Estimation_Report.pdf" target="_blank" rel="noopener">
      <i class="fa fa-file-pdf-o"></i> Final Report (PDF)
    </a>
    <a class="imu-project-link" href="/files/imu-attitude-estimation/Low_Cost_IMU_Attitude_Estimation_Report_Alternate.pdf" target="_blank" rel="noopener">
      <i class="fa fa-file-pdf-o"></i> Alternate PDF
    </a>
    <a class="imu-project-link" href="/files/imu-attitude-estimation/Low_Cost_IMU_Attitude_Estimation_Writeup.docx" target="_blank" rel="noopener">
      <i class="fa fa-file-word-o"></i> Editable Writeup
    </a>
    <a class="imu-project-link" href="/files/imu-attitude-estimation/Low_Cost_IMU_Attitude_Estimation_Circuit_Layout.docx" target="_blank" rel="noopener">
      <i class="fa fa-file-word-o"></i> Circuit Layout
    </a>
  </div>
</div>

## Hardware Setup

The physical setup was intentionally lightweight: an Arduino UNO Rev3, a BNO055 9-axis IMU, a mini breadboard, and four jumper wires. That small footprint made it easy to prototype, but it also meant the filter had to do real work if the final orientation estimates were going to look stable and useful.

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/imu-attitude-estimation/hardware-photo.jpg" loading="lazy" alt="Arduino UNO and BNO055 IMU hardware setup on a breadboard.">
    <img src="/images/imu-attitude-estimation/wiring-diagram.png" loading="lazy" alt="Wiring diagram for the Arduino and BNO055 IMU.">
  </div>
  <em>The completed hardware build and the matching wiring layout used for the project.</em>
</div>

The I2C wiring was simple and reliable:

<div class="imu-project-scroll">
  <table class="imu-project-table">
    <thead>
      <tr>
        <th>Arduino UNO</th>
        <th>BNO055 IMU</th>
        <th>Purpose</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>A5</td><td>SCL</td><td>I2C clock line</td></tr>
      <tr><td>A4</td><td>SDA</td><td>I2C data line</td></tr>
      <tr><td>GND</td><td>GND</td><td>Common electrical ground</td></tr>
      <tr><td>5V</td><td>Vin</td><td>Power input to the IMU board</td></tr>
    </tbody>
  </table>
</div>

<div class="imu-project-grid">
  <div class="imu-project-card">
    <h3>Why this platform worked well</h3>
    <p>
      The Arduino UNO gave the project an easy serial and power interface, while the BNO055 packaged a gyroscope, accelerometer, and magnetometer into one accessible sensor board. It was a practical testbed for attitude-estimation work without turning the build into a larger embedded systems project.
    </p>
  </div>
  <div class="imu-project-card">
    <h3>What the setup exposed</h3>
    <p>
      Because the hardware was low-cost, noise and responsiveness limits were visible right away. That was actually useful: it made it easier to see what the EKF improved, and where sensor bandwidth still became the limiting factor.
    </p>
  </div>
</div>

## Estimation Approach

The filter was built as a quaternion-based EKF rather than a direct Euler-angle filter. That choice mattered because quaternions avoid the singularities and awkward edge cases that can come with naive roll-pitch-yaw integration.

<div class="imu-project-equation">
  <div class="imu-project-equation__line">
    x&#770;<sub>k</sub><sup>-</sup> = f( x&#770;<sub>k-1</sub>, u<sub>k-1</sub>, 0 )
  </div>
  <div class="imu-project-equation__line">
    P<sub>k</sub><sup>-</sup> = A<sub>k</sub> P<sub>k-1</sub> A<sub>k</sub><sup>T</sup> + Q
  </div>
  <div class="imu-project-equation__note">
    Prediction used the prior state, gyro input, and a simplified process-noise model with a constant Q term.
  </div>
</div>

<div class="imu-project-equation">
  <div class="imu-project-equation__line">
    K<sub>k</sub> = P<sub>k</sub><sup>-</sup> ( P<sub>k</sub><sup>-</sup> + R<sub>k</sub> )<sup>-1</sup>
  </div>
  <div class="imu-project-equation__line">
    x&#770;<sub>k</sub> = x&#770;<sub>k</sub><sup>-</sup> + K<sub>k</sub> ( z<sub>k</sub> - h( x&#770;<sub>k</sub><sup>-</sup>, 0 ) )
  </div>
  <div class="imu-project-equation__note">
    After prediction, the filter blended the state estimate with measured attitude information derived from the accelerometers.
  </div>
</div>

The measurement side of the filter used accelerometer-based attitude estimates for roll and pitch, while yaw was held at zero because the magnetometer behavior was too sensitive to rely on cleanly in this setup:

<div class="imu-project-equation">
  <div class="imu-project-equation__line">
    &theta; = sin<sup>-1</sup>( a<sub>x</sub> / g ),&nbsp;
    &phi; = tan<sup>-1</sup>( a<sub>y</sub> / a<sub>z</sub> ),&nbsp;
    &psi; = 0
  </div>
  <div class="imu-project-equation__note">
    That tradeoff kept the project focused on stable roll and pitch estimation without over-claiming yaw accuracy from noisy magnetic data.
  </div>
</div>

The sensor covariance values used for the measurement model were gathered by leaving the IMU stationary for roughly three minutes and computing the covariance in MATLAB:

<div class="imu-project-scroll">
  <table class="imu-project-table">
    <thead>
      <tr>
        <th>Sensor channel</th>
        <th>Measured covariance</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>Accel x</td><td>1.732 &times; 10<sup>-7</sup></td></tr>
      <tr><td>Accel y</td><td>1.327 &times; 10<sup>-7</sup></td></tr>
      <tr><td>Accel z</td><td>4.473 &times; 10<sup>-7</sup></td></tr>
      <tr><td>Gyro x</td><td>4.125 &times; 10<sup>-7</sup></td></tr>
      <tr><td>Gyro y</td><td>6.884 &times; 10<sup>-7</sup></td></tr>
      <tr><td>Gyro z</td><td>4.011 &times; 10<sup>-7</sup></td></tr>
    </tbody>
  </table>
</div>

## Bring-Up Workflow

Before the filtering work could be tested, the MATLAB support package for Arduino had to be configured so the board and IMU could be read reliably from the desktop toolchain. That step was simple, but it mattered because the whole data path depended on a clean hardware-software handshake.

<div class="gallery-box">
  <div class="gallery gallery-columns-1">
    <img src="/images/imu-attitude-estimation/matlab-hardware-setup.jpg" loading="lazy" alt="MATLAB Arduino hardware support setup completion window.">
  </div>
  <em>MATLAB hardware support setup used to get the Arduino communication pipeline running.</em>
</div>

## Experimental Results

The project tested the EKF under two broad motion regimes:

1. Low-frequency roll, pitch, and combined maneuvers over 10 seconds.
2. Higher-frequency roll, pitch, and combined maneuvers over 2 seconds.

That split made the performance differences easy to see. The filter looked strong when the motion changed gradually, but the faster tests showed where the IMU and overall sampling chain started to struggle.

### Low-Frequency Maneuvers

The 10-second tests produced the cleanest results. Roll and pitch traces stayed smooth, and the combined maneuver behaved consistently with the single-axis cases.

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/imu-attitude-estimation/roll-10s.png" loading="lazy" alt="Roll estimate during the 10-second maneuver.">
    <img src="/images/imu-attitude-estimation/pitch-10s.png" loading="lazy" alt="Pitch estimate during the 10-second maneuver.">
    <img src="/images/imu-attitude-estimation/roll-pitch-10s.png" loading="lazy" alt="Combined roll and pitch estimate during the 10-second maneuver.">
  </div>
  <em>For slower maneuvers, the EKF produced smoother and more convincing attitude estimates.</em>
</div>

### Higher-Frequency Maneuvers

When the same general test was compressed into a 2-second window, the outputs became much sharper and more angular. The roll and pitch estimates still tracked the commanded motion directionally, but the response was visibly less smooth and less settled.

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/imu-attitude-estimation/roll-2s.png" loading="lazy" alt="Roll estimate during the faster 2-second maneuver.">
    <img src="/images/imu-attitude-estimation/pitch-2s.png" loading="lazy" alt="Pitch estimate during the faster 2-second maneuver.">
    <img src="/images/imu-attitude-estimation/roll-pitch-2s.png" loading="lazy" alt="Combined roll and pitch estimate during the faster 2-second maneuver.">
  </div>
  <em>The faster tests exposed the responsiveness limit of the low-cost hardware and the sampling chain.</em>
</div>

<div class="imu-project-grid">
  <div class="imu-project-card">
    <h3>What worked well</h3>
    <p>
      The EKF was effective for gradual attitude changes. In the slower 10-second tests, the filtered roll and pitch estimates looked smooth, stable, and well-behaved for both single-axis and combined motions.
    </p>
  </div>
  <div class="imu-project-card">
    <h3>What broke down first</h3>
    <p>
      High-frequency motion exposed the system's limits. The filter did not fail outright, but the curves became visibly more angular, which points back to hardware sampling constraints and the need for a faster, more robust IMU for aggressive maneuvers.
    </p>
  </div>
</div>

## Outcome and Next Steps

This project was a good example of how much value you can get from a modest hardware stack when the estimation logic is solid. With an Arduino, a BNO055, and MATLAB, we built a working attitude-estimation pipeline that could clean up pitch and roll measurements and clearly demonstrate the strengths and limits of a low-cost EKF-based solution.

The next steps are clear from the test results:

1. Add yaw estimation back in once the magnetic measurement path is more trustworthy.
2. Tune the EKF more aggressively for dynamic maneuvers.
3. Move to a higher-rate IMU if fast attitude changes are a real requirement.
4. Keep the same estimation framework, but apply it to a more flight-like embedded setup.

If you want the full derivation, the report and editable writeup linked above go into the equations, implementation details, and experimental discussion in much more depth.
