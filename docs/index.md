---
hide:
  - toc
---

# `New_Robot` Code Explainer — Architecture & Class Map

Welcome to the software architecture guide for **[`New_Robot`](https://github.com/FRC-Flux-Robotics/New_Robot)** (FRC Team 10413 FLUX Robotics — 2026 Season).

This guide explains **every Java class in the repository**, **why it exists**, **what it does during a match**, and **how all the classes connect together**—with direct links to the source code on GitHub.

---

## 1. Overall Class Relationship Diagram

The diagram below shows how all 38 Java classes in [`New_Robot/src/main/java/frc/`](https://github.com/FRC-Flux-Robotics/New_Robot/tree/main/src/main/java/frc) flow from **JVM Boot** at the top down to **AdvantageKit `*IO` Hardware & CAN Batching** at the bottom. Each of the 4 module pages linked below also includes a zoomed-in diagram for that specific package.

<div style="margin: 1.5rem 0; overflow-x: auto;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 960 820" style="width: 100%; max-width: 960px; height: auto; display: block; margin: 0 auto; background: #0f172a; border: 1px solid #334155; border-radius: 12px; font-family: system-ui, -apple-system, sans-serif;">
<defs>
<marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
<path d="M 0 1 L 10 5 L 0 9 z" fill="#94a3b8"/>
</marker>
<marker id="arrow-cyan" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
<path d="M 0 1 L 10 5 L 0 9 z" fill="#38bdf8"/>
</marker>
</defs>
<!-- Title Banner -->
<rect x="20" y="16" width="920" height="40" rx="8" fill="#1e293b" stroke="#475569"/>
<text x="480" y="42" text-anchor="middle" fill="#f8fafc" font-size="16" font-weight="700">New_Robot — Overall Class Relationship Map (Click Any Box to Open Source on GitHub)</text>
<!-- TIER 1: BOOT & ROBOT CONFIG -->
<text x="30" y="82" fill="#38bdf8" font-size="12" font-weight="700" letter-spacing="1">TIER 1 — JVM ENTRY, 20MS LOOP &amp; ROBOT IDENTITY (frc.robot)</text>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Main.java" target="_blank">
<rect x="30" y="92" width="210" height="72" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="135" y="120" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="15" font-weight="700">Main.java</text>
<text x="135" y="142" text-anchor="middle" fill="#cbd5e1" font-size="12">JVM Entry Point</text>
<text x="135" y="157" text-anchor="middle" fill="#94a3b8" font-size="11">RobotBase.startRobot()</text>
</a>
<line x1="240" y1="128" x2="310" y2="128" stroke="#38bdf8" stroke-width="2" marker-end="url(#arrow-cyan)"/>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robot.java" target="_blank">
<rect x="315" y="92" width="310" height="72" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="470" y="118" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="15" font-weight="700">Robot.java (LoggedRobot)</text>
<text x="470" y="138" text-anchor="middle" fill="#cbd5e1" font-size="12">20ms Loop + Mode (REAL / SIM / REPLAY)</text>
<text x="470" y="155" text-anchor="middle" fill="#94a3b8" font-size="11">Uses LogFileManager, LoggedTracer, Elastic</text>
</a>
<line x1="625" y1="128" x2="695" y2="128" stroke="#38bdf8" stroke-width="2" marker-end="url(#arrow-cyan)"/>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robots.java" target="_blank">
<rect x="700" y="92" width="230" height="72" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="815" y="118" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="15" font-weight="700">Robots.java</text>
<text x="815" y="138" text-anchor="middle" fill="#cbd5e1" font-size="12">FUEL &amp; CORAL Hardware Configs</text>
<text x="815" y="155" text-anchor="middle" fill="#94a3b8" font-size="11">DrivetrainConfig + CameraConfig[]</text>
</a>
<!-- Arrows Tier 1 -> Tier 2 -->
<line x1="470" y1="164" x2="470" y2="210" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="815" y1="164" x2="815" y2="210" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<!-- TIER 2: CONTAINERS & DRIVER INPUT -->
<text x="30" y="204" fill="#a78bfa" font-size="12" font-weight="700" letter-spacing="1">TIER 2 — ROBOT CONTAINERS &amp; JOYSTICK INPUT PROCESSING (frc.robot)</text>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/InputProcessing.java" target="_blank">
<rect x="30" y="214" width="250" height="84" rx="8" fill="#1e293b" stroke="#a78bfa" stroke-width="2"/>
<text x="155" y="238" text-anchor="middle" fill="#c4b5fd" font-family="monospace" font-size="14" font-weight="700">Driver Input Helpers</text>
<text x="155" y="257" text-anchor="middle" fill="#cbd5e1" font-size="12">InputProcessing &amp; PiecewiseSensitivity</text>
<text x="155" y="274" text-anchor="middle" fill="#cbd5e1" font-size="12">SensitivityTuner &amp; DriverPreferences</text>
<text x="155" y="290" text-anchor="middle" fill="#94a3b8" font-size="11">Deadband, Slew Rate &amp; Response Curves</text>
</a>
<line x1="280" y1="256" x2="310" y2="256" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RobotContainer.java" target="_blank">
<rect x="315" y="214" width="310" height="84" rx="8" fill="#1e293b" stroke="#a78bfa" stroke-width="2"/>
<text x="470" y="238" text-anchor="middle" fill="#c4b5fd" font-family="monospace" font-size="15" font-weight="700">RobotContainer.java</text>
<text x="470" y="258" text-anchor="middle" fill="#cbd5e1" font-size="12">Base Chassis Controls (Used on CORAL)</text>
<text x="470" y="275" text-anchor="middle" fill="#cbd5e1" font-size="12">Wires Swerve, Vision, Pose Reset &amp; Autos</text>
<text x="470" y="291" text-anchor="middle" fill="#94a3b8" font-size="11">Driver Xbox Controller (Port 0)</text>
</a>
<line x1="625" y1="256" x2="665" y2="256" stroke="#a78bfa" stroke-width="2" stroke-dasharray="5,4" marker-end="url(#arrow)"/>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FuelRobotContainer.java" target="_blank">
<rect x="670" y="214" width="260" height="84" rx="8" fill="#1e293b" stroke="#a78bfa" stroke-width="2"/>
<text x="800" y="238" text-anchor="middle" fill="#c4b5fd" font-family="monospace" font-size="15" font-weight="700">FuelRobotContainer.java</text>
<text x="800" y="258" text-anchor="middle" fill="#cbd5e1" font-size="12">Extends RobotContainer for FUEL</text>
<text x="800" y="275" text-anchor="middle" fill="#cbd5e1" font-size="12">Adds 6 Mechanisms &amp; NamedCommands</text>
<text x="800" y="291" text-anchor="middle" fill="#94a3b8" font-size="11">Adds Operator Xbox (Port 1) + MechanismTuning</text>
</a>
<!-- Arrows Tier 2 -> Tier 3 -->
<line x1="380" y1="298" x2="175" y2="346" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="470" y1="298" x2="480" y2="346" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="800" y1="298" x2="785" y2="346" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<!-- TIER 3: COMMANDS & AUTONOMOUS -->
<text x="30" y="338" fill="#facc15" font-size="12" font-weight="700" letter-spacing="1">TIER 3 — AUTONOMOUS ROUTINES &amp; COMMANDS (frc.robot &amp; frc.robot.commands)</text>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Autos.java" target="_blank">
<rect x="30" y="350" width="290" height="88" rx="8" fill="#1e293b" stroke="#facc15" stroke-width="2"/>
<text x="175" y="374" text-anchor="middle" fill="#fde047" font-family="monospace" font-size="14" font-weight="700">Autonomous &amp; Safety</text>
<text x="175" y="394" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">Autos.java &amp; SafeAutoBuilder.java</text>
<text x="175" y="411" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">FieldPositions.java</text>
<text x="175" y="428" text-anchor="middle" fill="#94a3b8" font-size="11">Pre-Flight Pose Check + Runtime Tilt/Stall Abort</text>
</a>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/DriveToTag.java" target="_blank">
<rect x="335" y="350" width="290" height="88" rx="8" fill="#1e293b" stroke="#facc15" stroke-width="2"/>
<text x="480" y="374" text-anchor="middle" fill="#fde047" font-family="monospace" font-size="14" font-weight="700">Vision &amp; Calibration Cmds</text>
<text x="480" y="394" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">DriveToTag &amp; ResetPoseFromVision</text>
<text x="480" y="411" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">CameraValidationCmd &amp; AutoCalibrateCmd</text>
<text x="480" y="428" text-anchor="middle" fill="#94a3b8" font-size="11">Tag Alignment, Pose Seed &amp; Extrinsic Calibration</text>
</a>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/RangeShootCmd.java" target="_blank">
<rect x="640" y="350" width="290" height="88" rx="8" fill="#1e293b" stroke="#facc15" stroke-width="2"/>
<text x="785" y="374" text-anchor="middle" fill="#fde047" font-family="monospace" font-size="14" font-weight="700">Shooting Commands</text>
<text x="785" y="394" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">ShootCommand &amp; RangeShootCmd</text>
<text x="785" y="411" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">SetShooterRangeCmd, VelocityCmd, RangeTable</text>
<text x="785" y="428" text-anchor="middle" fill="#94a3b8" font-size="11">Distance Lookup -&gt; Hood + Shooter Spin-Up -&gt; Feed</text>
</a>
<!-- Arrows Tier 3 -> Tier 4 -->
<line x1="175" y1="438" x2="175" y2="484" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="480" y1="438" x2="480" y2="484" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="785" y1="438" x2="785" y2="484" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<!-- TIER 4: CORE SUBSYSTEMS -->
<text x="30" y="476" fill="#4ade80" font-size="12" font-weight="700" letter-spacing="1">TIER 4 — SUBSYSTEMS (frc.lib.drivetrain, frc.robot, frc.lib.mechanism)</text>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java" target="_blank">
<rect x="30" y="488" width="290" height="92" rx="8" fill="#1e293b" stroke="#4ade80" stroke-width="2"/>
<text x="175" y="512" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">DriveInterface &amp; SwerveDrive</text>
<text x="175" y="532" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">DrivetrainConfig &amp; ModuleConfig</text>
<text x="175" y="549" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">PIDGains &amp; DriveState</text>
<text x="175" y="567" text-anchor="middle" fill="#94a3b8" font-size="11">Field-Centric Swerve, Odometry &amp; PathPlanner</text>
</a>
<line x1="335" y1="534" x2="322" y2="534" stroke="#4ade80" stroke-width="2" marker-end="url(#arrow)"/>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Vision.java" target="_blank">
<rect x="335" y="488" width="290" height="92" rx="8" fill="#1e293b" stroke="#4ade80" stroke-width="2"/>
<text x="480" y="512" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">Vision.java</text>
<text x="480" y="532" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">CameraConfig &amp; VisionRejectReason</text>
<text x="480" y="549" text-anchor="middle" fill="#cbd5e1" font-size="12">5 Rejection Filters + Dynamic Std Devs</text>
<text x="480" y="567" text-anchor="middle" fill="#94a3b8" font-size="11">Feeds addVisionMeasurement() to DriveInterface</text>
</a>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java" target="_blank">
<rect x="640" y="488" width="290" height="92" rx="8" fill="#1e293b" stroke="#4ade80" stroke-width="2"/>
<text x="785" y="512" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">Velocity &amp; PositionMechanism</text>
<text x="785" y="532" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">MechanismConfigs &amp; MechanismConfig</text>
<text x="785" y="549" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">ControlMode (Velocity / MotionMagic)</text>
<text x="785" y="567" text-anchor="middle" fill="#94a3b8" font-size="11">Intake, Tilter, Indexer, Feeder, Shooter, Hood</text>
</a>
<!-- Arrows Tier 4 -> Tier 5 -->
<line x1="175" y1="580" x2="175" y2="626" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="480" y1="580" x2="480" y2="626" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="785" y1="580" x2="785" y2="626" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<!-- TIER 5: ADVANTAGEKIT IO LAYER -->
<text x="30" y="618" fill="#fb923c" font-size="12" font-weight="700" letter-spacing="1">TIER 5 — ADVANTAGEKIT @AUTOLOG HARDWARE IO INTERFACES (REAL vs. REPLAY)</text>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIO.java" target="_blank">
<rect x="30" y="630" width="290" height="86" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="175" y="654" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="14" font-weight="700">DrivetrainIO.java</text>
<text x="175" y="674" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">DrivetrainIOTalonFX (Kraken + Pigeon 2)</text>
<text x="175" y="692" text-anchor="middle" fill="#94a3b8" font-family="monospace" font-size="12">DrivetrainIOReplay (Sim / Log Replay)</text>
</a>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/vision/VisionIO.java" target="_blank">
<rect x="335" y="630" width="290" height="86" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="480" y="654" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="14" font-weight="700">VisionIO.java</text>
<text x="480" y="674" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">VisionIOPhotonVision (OV9281 Cameras)</text>
<text x="480" y="692" text-anchor="middle" fill="#94a3b8" font-family="monospace" font-size="12">VisionIOReplay (Sim / Log Replay)</text>
</a>
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIO.java" target="_blank">
<rect x="640" y="630" width="290" height="86" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="785" y="654" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="14" font-weight="700">MechanismIO.java</text>
<text x="785" y="674" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">MechanismIOTalonFX &amp; DualTalonFX</text>
<text x="785" y="692" text-anchor="middle" fill="#94a3b8" font-family="monospace" font-size="12">MechanismIOReplay (Sim / Log Replay)</text>
</a>
<!-- Arrows Tier 5 -> Tier 6 -->
<line x1="175" y1="716" x2="320" y2="752" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<line x1="785" y1="716" x2="640" y2="752" stroke="#94a3b8" stroke-width="2" marker-end="url(#arrow)"/>
<!-- TIER 6: CAN SIGNAL BATCHING -->
<a href="https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/PhoenixSignals.java" target="_blank">
<rect x="180" y="752" width="600" height="52" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="480" y="775" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="14" font-weight="700">PhoenixSignals.java &amp; PhoenixUtil.java (frc.lib.util)</text>
<text x="480" y="794" text-anchor="middle" fill="#cbd5e1" font-size="12">Batches all 50Hz CTRE StatusSignals into a single BaseStatusSignal.refreshAll() call per 20ms loop</text>
</a>
</svg>
</div>

---

## 2. Why `New_Robot` Is Designed This Way (3 Core Principles)

1. **`frc.lib.*` (Reusable Library) vs. `frc.robot.*` (Season/Robot Logic):**
   * **[`src/main/java/frc/lib/`](https://github.com/FRC-Flux-Robotics/New_Robot/tree/main/src/main/java/frc/lib)** contains reusable swerve, vision, mechanism, and CAN batching building blocks that do not hardcode any specific game year or CAN ID.
   * **[`src/main/java/frc/robot/`](https://github.com/FRC-Flux-Robotics/New_Robot/tree/main/src/main/java/frc/robot)** contains the specific robot configs (`FUEL` vs. `CORAL`), controller bindings, field coordinates, and autonomous routines.
2. **AdvantageKit `*IO` Hardware Abstraction (`REAL` / `SIM` / `REPLAY`):**
   * Subsystems (`SwerveDrive`, `Vision`, `PositionMechanism`, `VelocityMechanism`) never read hardware sensors directly in their logic. Instead, they read an `@AutoLog` `*IOInputs` object populated by `*IOTalonFX` / `*IOPhotonVision` on the real robot or `*IOReplay` when replaying a `.wpilog` match file on a laptop.
3. **Config-Driven Subsystems (Zero Copy-Paste Subsystems):**
   * Instead of creating 6 separate classes for `IntakeSubsystem`, `TilterSubsystem`, `IndexerSubsystem`, `FeederSubsystem`, `ShooterSubsystem`, and `HoodSubsystem`, **all 6 mechanisms on `FUEL`** are instances of just two classes—[`VelocityMechanism`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java) and [`PositionMechanism`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/PositionMechanism.java)—configured in [`MechanismConfigs.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismConfigs.java).

---

## 3. Class-by-Class Explainer Pages

Click into each module below for a detailed breakdown of every class, why it exists, what it does, a zoomed-in module diagram, and links to its source file on GitHub:

| Module Page | Classes Covered |
| :--- | :--- |
| **[1. Boot, Robot Configs & Containers](classes/robot-and-containers.md)** | [`Main.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Main.java), [`Robot.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robot.java), [`Robots.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robots.java), [`RobotContainer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RobotContainer.java), [`FuelRobotContainer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FuelRobotContainer.java), [`PiecewiseSensitivity.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/PiecewiseSensitivity.java), [`SensitivityTuner.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/SensitivityTuner.java), [`InputProcessing.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/InputProcessing.java), [`DriverPreferences.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/DriverPreferences.java) |
| **[2. Swerve Drivetrain Library](classes/drivetrain.md)** | [`DriveInterface.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveInterface.java), [`SwerveDrive.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java), [`DrivetrainIO.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIO.java), [`DrivetrainIOTalonFX.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIOTalonFX.java), [`DrivetrainIOReplay.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIOReplay.java), [`DrivetrainConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainConfig.java), [`ModuleConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/ModuleConfig.java), [`PIDGains.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/PIDGains.java), [`DriveState.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveState.java) |
| **[3. Mechanisms & Shooting](classes/mechanisms.md)** | [`VelocityMechanism.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java), [`PositionMechanism.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/PositionMechanism.java), [`MechanismIO.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIO.java), [`MechanismIOTalonFX.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIOTalonFX.java), [`MechanismIODualTalonFX.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIODualTalonFX.java), [`MechanismIOReplay.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIOReplay.java), [`MechanismConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismConfig.java), [`ControlMode.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/ControlMode.java), [`MechanismConfigs.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismConfigs.java), [`MechanismTuning.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismTuning.java), [`RangeTable.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RangeTable.java), [`VelocityCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/VelocityCmd.java), [`ShootCommand.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/ShootCommand.java), [`SetShooterRangeCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/SetShooterRangeCmd.java), [`RangeShootCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/RangeShootCmd.java) |
| **[4. Vision, Autonomous & Utilities](classes/vision-autos-and-util.md)** | [`Vision.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Vision.java), [`VisionRejectReason.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/VisionRejectReason.java), [`CameraConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/CameraConfig.java), [`VisionIO.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/vision/VisionIO.java), [`VisionIOPhotonVision.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/vision/VisionIOPhotonVision.java), [`VisionIOReplay.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/vision/VisionIOReplay.java), [`DriveToTag.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/DriveToTag.java), [`ResetPoseFromVision.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/ResetPoseFromVision.java), [`CameraValidationCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/CameraValidationCmd.java), [`AutoCalibrateCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/AutoCalibrateCmd.java), [`Autos.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Autos.java), [`SafeAutoBuilder.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/SafeAutoBuilder.java), [`FieldPositions.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FieldPositions.java), [`PhoenixSignals.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/PhoenixSignals.java), [`PhoenixUtil.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/PhoenixUtil.java), [`LogFileManager.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/LogFileManager.java), [`LoggedTracer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/LoggedTracer.java), [`Elastic.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/util/Elastic.java) |
