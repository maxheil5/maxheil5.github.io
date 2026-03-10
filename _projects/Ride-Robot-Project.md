---
title: "Ride Robot Project"
date: 2026-03-10 12:00:00 -0500
subtitle: Theme Park Robotics • Kinematics • Motion Planning
description: "A KUKA KR500 R2830 kinematics and motion-planning project for ride robotics, including forward kinematics, analytical inverse kinematics, RoboDK validation, and an interactive planning app."
image: '/images/ride-robot/reference/riders-on-kuka-arm.jpeg'
featured: false
---
<style>
  .ride-robot-intro {
    margin: 0 0 2rem;
    padding: 1.75rem;
    border: 1px solid var(--border-color);
    border-radius: 24px;
    background: linear-gradient(135deg, rgba(234, 117, 33, 0.12), rgba(24, 44, 97, 0.06));
  }

  .ride-robot-intro p:last-child,
  .ride-robot-card p:last-child,
  .ride-robot-note p:last-child {
    margin-bottom: 0;
  }

  .ride-robot-stats,
  .ride-robot-grid {
    display: grid;
    gap: 1rem;
    margin: 1.5rem 0 0;
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  .ride-robot-stats {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }

  .ride-robot-stat,
  .ride-robot-card,
  .ride-robot-note {
    padding: 1rem 1.1rem;
    border: 1px solid var(--border-color);
    border-radius: 20px;
    background: var(--background-alt-color);
  }

  .ride-robot-stat strong {
    display: block;
    margin-bottom: 0.25rem;
    color: var(--heading-font-color);
    font-size: 1.85rem;
    line-height: 1;
  }

  .ride-robot-stat span,
  .ride-robot-note,
  .ride-robot-table td small {
    color: var(--text-alt-color);
  }

  .ride-robot-links {
    display: flex;
    flex-wrap: wrap;
    gap: 0.75rem;
    margin-top: 1rem;
  }

  .ride-robot-link {
    display: inline-flex;
    align-items: center;
    gap: 0.5rem;
    padding: 0.85rem 1rem;
    border: 1px solid var(--border-color);
    border-radius: 999px;
    background: var(--background-color);
    text-decoration: none;
  }

  .ride-robot-scroll {
    overflow-x: auto;
    margin: 1.5rem 0;
  }

  .ride-robot-table {
    width: 100%;
    min-width: 560px;
    border-collapse: collapse;
    font-size: 0.96rem;
  }

  .ride-robot-table th,
  .ride-robot-table td {
    padding: 0.85rem 0.95rem;
    border-bottom: 1px solid var(--border-color);
    text-align: left;
    vertical-align: top;
  }

  .ride-robot-table th {
    color: var(--heading-font-color);
    background: rgba(24, 44, 97, 0.04);
  }

  .ride-robot-equation {
    margin: 1.5rem 0;
    padding: 1.25rem 1.4rem;
    border: 1px solid var(--border-color);
    border-radius: 22px;
    background: linear-gradient(180deg, rgba(24, 44, 97, 0.05), rgba(234, 117, 33, 0.08));
    overflow-x: auto;
  }

  .ride-robot-equation__line {
    min-width: max-content;
    font-family: "Old Standard TT", serif;
    font-size: clamp(1.05rem, 2.4vw, 1.45rem);
    line-height: 1.6;
    color: var(--heading-font-color);
  }

  .ride-robot-equation__note {
    margin-top: 0.55rem;
    color: var(--text-alt-color);
    font-size: 0.94rem;
  }

  .ride-robot-caption {
    margin-top: 0.85rem;
    color: var(--text-alt-color);
    font-size: 0.95rem;
  }

  .ride-robot-list {
    margin: 0;
    padding-left: 1.15rem;
  }

  .ride-robot-list li + li {
    margin-top: 0.45rem;
  }

  @media (max-width: 900px) {
    .ride-robot-stats {
      grid-template-columns: repeat(2, minmax(0, 1fr));
    }
  }

  @media (max-width: 600px) {
    .ride-robot-stats,
    .ride-robot-grid {
      grid-template-columns: minmax(0, 1fr);
    }

    .ride-robot-intro,
    .ride-robot-stat,
    .ride-robot-card,
    .ride-robot-note,
    .ride-robot-equation {
      border-radius: 16px;
    }
  }
</style>

<div class="ride-robot-intro">
  <p>
    This project started with a simple question: how do you model the motion of a ride robot in a way that is accurate enough to study, but still practical to build and validate in a semester? I focused on the <strong>KUKA KR500 R2830</strong> as a stand-in for ride-style KUKA RoboCoaster systems, derived the forward and inverse kinematics, checked the math against RoboDK, and wrapped everything into an interactive motion-planning app.
  </p>
  <p>
    The full paper is more mathematical than this page, but the core story is straightforward. I simplified the larger ride platform into a fixed-base 6-DOF arm, solved the pose relationships analytically, verified the results at machine precision, and used those same solvers to drive waypoint-based <strong>MoveJ</strong> and <strong>MoveL</strong> planning.
  </p>

  <div class="ride-robot-stats">
    <div class="ride-robot-stat">
      <strong>6</strong>
      <span>revolute joints in the fixed-base model</span>
    </div>
    <div class="ride-robot-stat">
      <strong>8</strong>
      <span>closed-form IK branches in the non-singular case</span>
    </div>
    <div class="ride-robot-stat">
      <strong>18</strong>
      <span>IK verification cases tested in MATLAB</span>
    </div>
    <div class="ride-robot-stat">
      <strong>2</strong>
      <span>planning modes, MoveJ and MoveL</span>
    </div>
  </div>

  <div class="ride-robot-links">
    <a class="ride-robot-link" href="/files/ME7751_Ride_Robot_Project_Paper.pdf" target="_blank" rel="noopener">
      <i class="fa fa-file-pdf-o"></i> Final Paper (PDF)
    </a>
    <a class="ride-robot-link" href="/files/ME7751_Ride_Robot_Project_Presentation.pdf" target="_blank" rel="noopener">
      <i class="fa fa-file-pdf-o"></i> Final Presentation (PDF)
    </a>
    <a class="ride-robot-link" href="https://github.com/maxheil5/ME7751_RideRobot_Project" target="_blank" rel="noopener">
      <i class="fa fa-github"></i> github.com/maxheil5/ME7751_RideRobot_Project
    </a>
  </div>
</div>

## Theme Park Context

The project was motivated by amusement ride robotics, especially systems that use large KUKA arms to move riders through tightly controlled motion profiles. Real ride installations can include a mobile base, proprietary hardware, and platform-specific geometry, so I reduced the problem to the robot arm itself. That made it possible to derive the kinematics cleanly while still keeping the work tied to a believable ride-robot use case.

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/ride-robot/reference/ride-vehicle-track.jpeg" loading="lazy" alt="Ride robot platform on a track.">
    <img src="/images/ride-robot/reference/riders-on-kuka-arm.jpeg" loading="lazy" alt="Riders seated on a KUKA-based attraction arm.">
  </div>
  <em>Ride robot references that motivated the fixed-base study used in this project.</em>
</div>

<div class="ride-robot-grid">
  <div class="ride-robot-card">
    <h3>Why the KR500 R2830?</h3>
    <p>
      The dedicated RoboCoaster variants do not expose much public geometry, so I used the KR500 R2830 as a close proxy. It is large, industrially realistic, and most importantly it has a <strong>spherical wrist</strong>, which makes a closed-form inverse kinematics solution possible.
    </p>
  </div>
  <div class="ride-robot-card">
    <h3>What was simplified?</h3>
    <p>
      The real ride architecture can include the rider carriage, a platform, and sometimes a mobile lower structure. In this project, the arm was treated as a <strong>stationary serial manipulator</strong> so the FK, IK, and planning logic could be isolated and validated first.
    </p>
  </div>
</div>

## Robot Model

The paper defines the robot as a 6-DOF serial manipulator with a base frame <code>{0}</code>, tool frame <code>{6}</code>, and intermediate frames assigned using the modified Denavit-Hartenberg convention. The pose map is written as a homogeneous transform from the base to the tool frame:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    <sup>0</sup>T<sub>6</sub>(q) =
    <span style="display:inline-block; vertical-align:middle; padding:0 0.2rem;">
      [ R<sub>06</sub>(q) &nbsp; p<sub>06</sub>(q) ;&nbsp; 0<sup>T</sup> &nbsp; 1 ]
    </span>
  </div>
  <div class="ride-robot-equation__note">
    In plain terms, the matrix stores both the end-effector orientation and its position in the base frame.
  </div>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/ride-robot/reference/kr500-render.png" loading="lazy" alt="Rendered KUKA KR500 R2830 model used for the project.">
    <img src="/images/ride-robot/reference/kr500-coordinate-frames.png" loading="lazy" alt="Coordinate frame layout used for the modified DH model.">
  </div>
  <em>The simplified KR500 model and the frame assignment used for the derivation.</em>
</div>

To match the reference convention, the effective joint angles were offset and signed before building the transforms:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    &theta;<sub>1</sub> = -q<sub>1</sub>,&nbsp;
    &theta;<sub>2</sub> = q<sub>2</sub>,&nbsp;
    &theta;<sub>3</sub> = q<sub>3</sub> - &pi;/2,&nbsp;
    &theta;<sub>4</sub> = -q<sub>4</sub>,&nbsp;
    &theta;<sub>5</sub> = q<sub>5</sub>,&nbsp;
    &theta;<sub>6</sub> = &pi; - q<sub>6</sub>
  </div>
  <div class="ride-robot-equation__note">
    This sign convention is what keeps the MATLAB model aligned with the RoboDK robot.
  </div>
</div>

<div class="ride-robot-scroll">
  <table class="ride-robot-table">
    <thead>
      <tr>
        <th>Joint</th>
        <th>a<sub>i</sub> (mm)</th>
        <th>&alpha;<sub>i</sub> (rad)</th>
        <th>d<sub>i</sub> (mm)</th>
        <th>&theta; offset</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>1</td><td>0</td><td>0</td><td>1045</td><td>0</td></tr>
      <tr><td>2</td><td>500</td><td>-&pi;/2</td><td>0</td><td>0</td></tr>
      <tr><td>3</td><td>1300</td><td>0</td><td>0</td><td>-&pi;/2</td></tr>
      <tr><td>4</td><td>-55</td><td>-&pi;/2</td><td>1025</td><td>0</td></tr>
      <tr><td>5</td><td>0</td><td>+&pi;/2</td><td>0</td><td>0</td></tr>
      <tr><td>6</td><td>0</td><td>-&pi;/2</td><td>290</td><td>+&pi;</td></tr>
    </tbody>
  </table>
</div>

## Forward Kinematics

The forward-kinematics chain is the backbone of the whole project. Each link transform is built from the modified DH parameters, and the final pose is obtained by multiplying the six consecutive transforms:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    <sup>0</sup>T<sub>6</sub>(q) =
    <sup>0</sup>T<sub>1</sub>
    <sup>1</sup>T<sub>2</sub>
    <sup>2</sup>T<sub>3</sub>
    <sup>3</sup>T<sub>4</sub>
    <sup>4</sup>T<sub>5</sub>
    <sup>5</sup>T<sub>6</sub>
  </div>
  <div class="ride-robot-equation__note">
    Once that chain is expanded, the pose can be evaluated directly for any joint vector q.
  </div>
</div>

One of the helpful shorthand terms in the paper is the scalar <code>B</code>, which collects the radial reach of the first three joints:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    B = 500 + 1300 cos(q<sub>2</sub>) + 1025 cos(q<sub>2</sub> + q<sub>3</sub>) - 55 sin(q<sub>2</sub> + q<sub>3</sub>)
  </div>
  <div class="ride-robot-equation__line">
    p<sub>06</sub> = p<sub>04</sub> + 290 [ R<sub>13</sub> &nbsp; R<sub>23</sub> &nbsp; R<sub>33</sub> ]<sup>T</sup>
  </div>
  <div class="ride-robot-equation__note">
    That expression makes the geometry easier to read: the arm builds a frame-4 position first, then the wrist offset extends to the end effector.
  </div>
</div>

Rather than stop at symbolic work, I checked the implementation against RoboDK. The representative configurations below match to numerical precision, which gave confidence that the frame assignment and sign conventions were correct.

<div class="gallery-box">
  <div class="gallery gallery-columns-3">
    <img src="/images/ride-robot/fk/Robot_0_-90_90_0_0_0.png" loading="lazy" alt="RoboDK validation image for the home FK pose.">
    <img src="/images/ride-robot/fk/Robot_30_-20_40_10_-30_60.png" loading="lazy" alt="RoboDK validation image for a moderate FK pose.">
    <img src="/images/ride-robot/fk/Robot_-120_10_-80_-150_90_-120.png" loading="lazy" alt="RoboDK validation image for a larger FK stress case.">
  </div>
  <em>Three representative forward-kinematics validation cases reproduced in RoboDK.</em>
</div>

To quantify the agreement, the paper compares both position and orientation residuals:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    e<sub>p</sub> = || p<sub>MAT</sub> - p<sub>RDK</sub> ||<sub>2</sub>
  </div>
  <div class="ride-robot-equation__line">
    e<sub>R</sub> = cos<sup>-1</sup> ( ( tr( R<sub>MAT</sub><sup>T</sup> R<sub>RDK</sub> ) - 1 ) / 2 )
  </div>
  <div class="ride-robot-equation__note">
    Using RoboDK as the reference, the paper reports translational errors around 10<sup>-13</sup> mm and angular errors around 10<sup>-6</sup> deg for the shown cases.
  </div>
</div>

<div class="ride-robot-note">
  <p>
    The full MATLAB export used for the FK spot checks is shown below. It includes the joint inputs and the resulting homogeneous transforms for the representative cases highlighted in the paper.
  </p>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-1">
    <img src="/images/ride-robot/fk/T_values_all.png" loading="lazy" alt="MATLAB summary table of representative FK test transforms.">
  </div>
  <em>Representative FK outputs used to compare MATLAB and RoboDK pose matrices.</em>
</div>

## Analytical Inverse Kinematics

The inverse-kinematics result is the part of the project that made the motion-planning app practical. Because the KR500 uses a spherical wrist, the IK can be split into two stages:

<div class="ride-robot-grid">
  <div class="ride-robot-card">
    <h3>Position stage</h3>
    <p>
      Solve <strong>q1, q2, q3</strong> from the wrist-center location. This reduces the first half of the robot to a planar geometric problem with shoulder and elbow branches.
    </p>
  </div>
  <div class="ride-robot-card">
    <h3>Orientation stage</h3>
    <p>
      Solve <strong>q4, q5, q6</strong> from the reduced wrist rotation. This is where the flipped wrist branches appear, unless the robot is near a wrist singularity.
    </p>
  </div>
</div>

The wrist center is found by backing out the final flange offset from the tool pose:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    p<sub>wc</sub> = p<sub>06</sub> - d<sub>6</sub> z&#770;<sub>6</sub>,&nbsp; d<sub>6</sub> = 290 mm
  </div>
  <div class="ride-robot-equation__line">
    (S, E, W) &isin; {a, b} &times; {+, -} &times; {1, 2}
  </div>
  <div class="ride-robot-equation__note">
    That branch product gives up to eight non-singular solutions: two shoulder choices, two elbow choices, and two wrist choices.
  </div>
</div>

The paper also enforced joint-limit filtering after each candidate solution. The limits used in the model were:

<div class="ride-robot-scroll">
  <table class="ride-robot-table">
    <thead>
      <tr>
        <th>Joint</th>
        <th>Allowed range (deg)</th>
        <th>Role in the solution search</th>
      </tr>
    </thead>
    <tbody>
      <tr><td>q1</td><td>[-185, 185]</td><td>Shoulder branch filtering</td></tr>
      <tr><td>q2</td><td>[-130, 20]</td><td>Upper-arm reach filtering</td></tr>
      <tr><td>q3</td><td>[-100, 144]</td><td>Elbow branch filtering</td></tr>
      <tr><td>q4</td><td>[-350, 350]</td><td>Wrist rotation wrapping</td></tr>
      <tr><td>q5</td><td>[-120, 120]</td><td>Singularity-sensitive wrist tilt</td></tr>
      <tr><td>q6</td><td>[-350, 350]</td><td>Tool-roll wrapping</td></tr>
    </tbody>
  </table>
</div>

## IK Verification and Branch Behavior

I verified the analytical IK with a round-trip check:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    <sup>0</sup>T<sub>6,test</sub> = FK(q<sub>test</sub>),&nbsp;
    { q<sup>(k)</sup> } = IK(<sup>0</sup>T<sub>6,test</sub>),&nbsp;
    FK(q<sup>(k)</sup>) &asymp; <sup>0</sup>T<sub>6,test</sub>
  </div>
  <div class="ride-robot-equation__note">
    In other words, generate a pose from FK, solve it with IK, then push every returned branch back through FK and make sure the pose comes back.
  </div>
</div>

The three representative results from the paper are summarized below.

<div class="ride-robot-scroll">
  <table class="ride-robot-table">
    <thead>
      <tr>
        <th>Case</th>
        <th>Test pose q (deg)</th>
        <th>Returned solutions</th>
        <th>Worst reported error</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Home</td>
        <td>[0, -90, 90, 0, 0, 0]</td>
        <td>3</td>
        <td>10<sup>-13</sup> mm, 10<sup>-6</sup> deg</td>
      </tr>
      <tr>
        <td>Moderate pose</td>
        <td>[30, -20, 40, 10, -30, 60]</td>
        <td>4</td>
        <td>10<sup>-12</sup> mm, 10<sup>-6</sup> deg</td>
      </tr>
      <tr>
        <td>Full-branch case</td>
        <td>[0, -129, 20, 0, 10, 0]</td>
        <td>8</td>
        <td>10<sup>-12</sup> mm, 10<sup>-6</sup> deg</td>
      </tr>
    </tbody>
  </table>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/ride-robot/ik/Robot_All_Test_Poses_Home.png" loading="lazy" alt="RoboDK station showing home and additional target poses.">
    <img src="/images/ride-robot/ik/Robot_Full_Branch_8_Sol.png" loading="lazy" alt="RoboDK view for a full-branch inverse kinematics pose.">
  </div>
  <em>Visual IK verification in RoboDK for the home case and a configuration with many valid branches.</em>
</div>

<div class="gallery-box">
  <div class="gallery gallery-columns-2">
    <img src="/images/ride-robot/ik/Joints_Home.png" loading="lazy" alt="RoboDK list of other configurations for the home pose.">
    <img src="/images/ride-robot/ik/Joints_Full_Branch_8_Sol.png" loading="lazy" alt="RoboDK list of other configurations for a full-branch pose.">
  </div>
  <em>RoboDK reports multiple equivalent joint solutions, especially when angle wrapping or singular families are involved.</em>
</div>

The complete MATLAB summary covered <strong>18</strong> test cases. Every case recovered the target pose to numerical precision, and the original configuration was recovered up to angle wrapping and joint-limit conventions.

## Interactive Motion-Planning App

Once the FK and IK were working reliably, I integrated them into a lightweight planning app. The interface accepts two waypoints as either joint angles or Cartesian poses, solves the necessary IK, and animates the resulting motion while plotting the joint histories.

<div class="gallery-box">
  <div class="gallery gallery-columns-1">
    <img src="/images/ride-robot/reference/motion-planner-app.png" loading="lazy" alt="Interactive motion-planning app for the ride robot project.">
  </div>
  <em>The interactive planner combines waypoint entry, FK/IK evaluation, trajectory generation, and 3-D playback.</em>
</div>

The app supports two motion styles:

<div class="ride-robot-grid">
  <div class="ride-robot-card">
    <h3>MoveJ</h3>
    <p>
      Interpolate directly in joint space. This is usually the more straightforward option and is good for quickly checking feasibility and branch continuity.
    </p>
    <div class="ride-robot-equation" style="margin-bottom:0;">
      <div class="ride-robot-equation__line">
        &sigma;(&tau;) = 3&tau;<sup>2</sup> - 2&tau;<sup>3</sup>
      </div>
      <div class="ride-robot-equation__line">
        q<sub>k</sub> = q<sub>0</sub> + &sigma;(&tau;<sub>k</sub>) ( q<sub>f</sub> - q<sub>0</sub> )
      </div>
    </div>
  </div>
  <div class="ride-robot-card">
    <h3>MoveL</h3>
    <p>
      Keep the tool position on a straight Cartesian path while interpolating orientation smoothly with matrix log and exponential operators.
    </p>
    <div class="ride-robot-equation" style="margin-bottom:0;">
      <div class="ride-robot-equation__line">
        p<sub>k</sub> = p<sub>0</sub> + &sigma;(&tau;<sub>k</sub>) ( p<sub>f</sub> - p<sub>0</sub> )
      </div>
      <div class="ride-robot-equation__line">
        R<sub>k</sub> = R<sub>0</sub> exp( &sigma;(&tau;<sub>k</sub>) log( R<sub>0</sub><sup>T</sup> R<sub>f</sub> ) )
      </div>
    </div>
  </div>
</div>

When multiple IK branches exist at a sample, the planner keeps the motion continuous by selecting the feasible branch closest to the previous one:

<div class="ride-robot-equation">
  <div class="ride-robot-equation__line">
    q<sub>k</sub> = arg min<sub>q &isin; S<sub>k</sub></sub> || wrap<sub>&pi;</sub>( q - q<sub>k-1</sub> ) ||<sub>W</sub>
  </div>
  <div class="ride-robot-equation__note">
    That continuity rule prevents abrupt jumps between otherwise valid branches and lets the animation stay visually smooth.
  </div>
</div>

## Outcome and Next Steps

This project gave me a full end-to-end view of ride robot modeling: define a believable robot proxy, derive the kinematics, verify them against a simulator, and then use those same tools inside a planning interface. The final result is not just a set of equations on paper, it is a working workflow for evaluating pose reachability, branch selection, and motion feasibility.

The main future directions from the paper and presentation are:

1. Build a more robust simulation package around the same FK/IK core.
2. Find or create a usable URDF for the KR500 geometry.
3. Bring the mobile lower ride platform back into the model.
4. Build a scaled physical version with 3D-printed hardware.

If you want the full derivations, detailed validation screenshots, or the app source, the paper and repository linked above are the best places to go deeper.
