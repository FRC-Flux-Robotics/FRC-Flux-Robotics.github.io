# 2. Swerve Drivetrain Library (`frc.lib.drivetrain`)

All swerve drivetrain classes live in [`src/main/java/frc/lib/drivetrain/`](https://github.com/FRC-Flux-Robotics/New_Robot/tree/main/src/main/java/frc/lib/drivetrain). Other subsystems and commands never depend on [`SwerveDrive`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java) directly—they program against [`DriveInterface`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveInterface.java).

<div style="margin: 1.25rem 0; overflow-x: auto;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 900 230" style="width: 100%; max-width: 900px; height: auto; display: block; margin: 0 auto; background: #0f172a; border: 1px solid #334155; border-radius: 10px; font-family: system-ui, -apple-system, sans-serif;">
<defs>
<marker id="m2-arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
<path d="M 0 1 L 10 5 L 0 9 z" fill="#4ade80"/>
</marker>
</defs>
<rect x="20" y="25" width="240" height="72" rx="8" fill="#1e293b" stroke="#4ade80" stroke-width="2"/>
<text x="140" y="54" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">DriveInterface.java</text>
<text x="140" y="74" text-anchor="middle" fill="#cbd5e1" font-size="12">Abstract Drivetrain Contract</text>
<text x="140" y="90" text-anchor="middle" fill="#94a3b8" font-size="11">Used by Commands, Vision &amp; Autos</text>
<line x1="310" y1="61" x2="265" y2="61" stroke="#4ade80" stroke-width="2" stroke-dasharray="4,3" marker-end="url(#m2-arr)"/>
<rect x="315" y="25" width="270" height="72" rx="8" fill="#1e293b" stroke="#4ade80" stroke-width="2"/>
<text x="450" y="54" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">SwerveDrive.java</text>
<text x="450" y="74" text-anchor="middle" fill="#cbd5e1" font-size="12">CTRE SwerveDrivetrain + PathPlanner</text>
<text x="450" y="90" text-anchor="middle" fill="#94a3b8" font-size="11">Implements DriveInterface</text>
<line x1="630" y1="61" x2="590" y2="61" stroke="#4ade80" stroke-width="2" marker-end="url(#m2-arr)"/>
<rect x="635" y="25" width="245" height="72" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="757" y="50" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="13" font-weight="700">DrivetrainConfig &amp; ModuleConfig</text>
<text x="757" y="70" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">PIDGains &amp; DriveState</text>
<text x="757" y="88" text-anchor="middle" fill="#94a3b8" font-size="11">Immutable Hardware &amp; Telemetry</text>
<line x1="450" y1="97" x2="450" y2="132" stroke="#4ade80" stroke-width="2" marker-end="url(#m2-arr)"/>
<rect x="315" y="137" width="270" height="68" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="450" y="165" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="14" font-weight="700">DrivetrainIO.java</text>
<text x="450" y="185" text-anchor="middle" fill="#cbd5e1" font-size="12">@AutoLog Hardware Sensor Interface</text>
<line x1="260" y1="171" x2="310" y2="171" stroke="#4ade80" stroke-width="2" stroke-dasharray="4,3" marker-end="url(#m2-arr)"/>
<line x1="635" y1="171" x2="590" y2="171" stroke="#4ade80" stroke-width="2" stroke-dasharray="4,3" marker-end="url(#m2-arr)"/>
<rect x="20" y="137" width="240" height="68" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="140" y="165" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="13" font-weight="700">DrivetrainIOTalonFX.java</text>
<text x="140" y="185" text-anchor="middle" fill="#cbd5e1" font-size="12">Real Kraken X60 + Pigeon 2</text>
<rect x="635" y="137" width="245" height="68" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="757" y="165" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="13" font-weight="700">DrivetrainIOReplay.java</text>
<text x="757" y="185" text-anchor="middle" fill="#cbd5e1" font-size="12">Sim &amp; .wpilog Replay No-Op</text>
</svg>
</div>

---

## [`DriveInterface.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveInterface.java)
* **Source:** [`src/main/java/frc/lib/drivetrain/DriveInterface.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveInterface.java)
* **Why it exists:** Decouples commands, autonomous routines, and vision from the underlying CTRE Phoenix 6 hardware implementation. That allows unit tests to pass a lightweight mock/fake `DriveInterface` without needing physical motors.
* **What it does:** Declares the clean drivetrain contract:
  * **Movement:** `drive(xSpeed, ySpeed, rot, fieldRelative, periodSeconds)`, `driveFieldCentricFacingAngle(...)`, `setBrake()`, `setIdle()`
  * **Pose & State:** `getPose()`, `getVelocity()`, `getHeading()`, `getDriveState()`, `resetHeading()`, `resetPose(pose)`
  * **Vision & PathPlanner:** `addVisionMeasurement(visionPose, timestamp, stdDevs)`, `followPathCommand(pathName)`

---

## [`SwerveDrive.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java)
* **Source:** [`src/main/java/frc/lib/drivetrain/SwerveDrive.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java)
* **Why it exists:** Concrete CTRE Phoenix 6 swerve implementation of [`DriveInterface`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveInterface.java) for Kraken X60 drive/steer motors, CANcoders, and Pigeon 2 IMU.
* **What it does:**
  * Extends CTRE's `SwerveDrivetrain<TalonFX, TalonFX, CANcoder>` and configures current limits (`60A` stator / `120A` supply on drive, `60A` stator on steer), gear ratios, and PID gains from [`DrivetrainConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainConfig.java).
  * Pre-allocates reusable CTRE `SwerveRequest` objects (`FieldCentric`, `RobotCentric`, `FieldCentricFacingAngle`, `ApplyRobotSpeeds`, `SwerveDriveBrake`, `Idle`) so zero objects are allocated inside the 20 ms loop.
  * Configures **PathPlanner `AutoBuilder`** with a `PPHolonomicDriveController` for autonomous path following and alliance-aware path flipping.
  * Provides 3 **SysId characterization routines** (translation, steer, and rotation) capped at `4V` dynamic step to prevent brownouts.
  * Logs pose, velocity, and module states every cycle via [`DrivetrainIO`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIO.java) and AdvantageKit `Logger`.

---

## AdvantageKit Drivetrain IO Layer

| Class | Source Link | Why It Exists & What It Does |
| :--- | :--- | :--- |
| **`DrivetrainIO`** | [`DrivetrainIO.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIO.java) | Defines `@AutoLog class DrivetrainIOInputs` (`gyroYawDeg`, `gyroRateDegPerSec`, `batteryVoltage`, `driveCurrentA[4]`, `steerCurrentA[4]`, `moduleAngleDeg[4]`) so all drivetrain sensor signals are recorded into `.wpilog` files every 20 ms. |
| **`DrivetrainIOTalonFX`** | [`DrivetrainIOTalonFX.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIOTalonFX.java) | Real hardware implementation of [`DrivetrainIO`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIO.java). Registers all 14 drivetrain `StatusSignal` objects (2 Pigeon 2 gyro signals + 4 modules × 3 signals) with [`PhoenixSignals`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/PhoenixSignals.java) for single-call batched CAN refreshing. |
| **`DrivetrainIOReplay`** | [`DrivetrainIOReplay.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainIOReplay.java) | No-op implementation used in `Mode.REPLAY` when AdvantageKit injects logged sensor values from a `.wpilog` file. |

---

## Drivetrain Configuration & Data Records

| Class | Source Link | Why It Exists & What It Does |
| :--- | :--- | :--- |
| **`DrivetrainConfig`** | [`DrivetrainConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainConfig.java) | Immutable configuration object with a fluent `Builder` that holds CAN bus name, Pigeon 2 ID, all 4 [`ModuleConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/ModuleConfig.java) corners, gear ratios, wheel radius, max speed/angular rate, [`PIDGains`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/PIDGains.java), current limits, track dimensions, and `List<CameraConfig>`. |
| **`ModuleConfig`** | [`ModuleConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/ModuleConfig.java) | Holds the per-corner hardware settings for one swerve module: `driveMotorId`, `steerMotorId`, `encoderId` (CANcoder), `encoderOffset` (in rotations), `(xPosInches, yPosInches)` relative to robot center, and `invertDrive` / `invertSteer` flags. |
| **`PIDGains`** | [`PIDGains.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/PIDGains.java) | Compact value object storing closed-loop PID and feedforward gains (`kP`, `kI`, `kD`, `kS`, `kV`, `kA`) shared by both swerve modules and mechanisms. |
| **`DriveState`** | [`DriveState.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveState.java) | Snapshot of current drivetrain state (`Pose2d`, `ChassisSpeeds`, module states/positions, and odometry period) returned by `DriveInterface.getDriveState()`. |
