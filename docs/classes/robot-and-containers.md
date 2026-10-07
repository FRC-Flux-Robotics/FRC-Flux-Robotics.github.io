# 1. Boot, Robot Configs & Containers (`frc.robot`)

These classes handle **starting the robot program**, **detecting which physical robot (`FUEL` or `CORAL`) is running**, **wiring up subsystems and Xbox controllers**, and **shaping joystick inputs**.

<div style="margin: 1.25rem 0; overflow-x: auto;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 900 230" style="width: 100%; max-width: 900px; height: auto; display: block; margin: 0 auto; background: #0f172a; border: 1px solid #334155; border-radius: 10px; font-family: system-ui, -apple-system, sans-serif;">
<defs>
<marker id="m1-arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
<path d="M 0 1 L 10 5 L 0 9 z" fill="#38bdf8"/>
</marker>
</defs>
<rect x="20" y="25" width="160" height="68" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="100" y="54" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="14" font-weight="700">Main.java</text>
<text x="100" y="74" text-anchor="middle" fill="#cbd5e1" font-size="12">JVM Entry Point</text>
<line x1="180" y1="59" x2="215" y2="59" stroke="#38bdf8" stroke-width="2" marker-end="url(#m1-arr)"/>
<rect x="220" y="25" width="210" height="68" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="325" y="54" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="14" font-weight="700">Robot.java</text>
<text x="325" y="74" text-anchor="middle" fill="#cbd5e1" font-size="12">LoggedRobot 20ms Loop</text>
<line x1="430" y1="59" x2="465" y2="59" stroke="#38bdf8" stroke-width="2" marker-end="url(#m1-arr)"/>
<rect x="470" y="25" width="190" height="68" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="565" y="54" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="14" font-weight="700">Robots.java</text>
<text x="565" y="74" text-anchor="middle" fill="#cbd5e1" font-size="12">FUEL &amp; CORAL Configs</text>
<line x1="565" y1="93" x2="565" y2="130" stroke="#38bdf8" stroke-width="2" marker-end="url(#m1-arr)"/>
<line x1="660" y1="59" x2="780" y2="130" stroke="#38bdf8" stroke-width="2" marker-end="url(#m1-arr)"/>
<rect x="20" y="135" width="380" height="72" rx="8" fill="#1e293b" stroke="#a78bfa" stroke-width="2"/>
<text x="210" y="160" text-anchor="middle" fill="#c4b5fd" font-family="monospace" font-size="13" font-weight="700">InputProcessing &amp; PiecewiseSensitivity</text>
<text x="210" y="180" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">SensitivityTuner &amp; DriverPreferences</text>
<text x="210" y="197" text-anchor="middle" fill="#94a3b8" font-size="11">Joystick Curves, Deadband &amp; Slew Limits</text>
<line x1="400" y1="171" x2="445" y2="171" stroke="#38bdf8" stroke-width="2" marker-end="url(#m1-arr)"/>
<rect x="450" y="135" width="210" height="72" rx="8" fill="#1e293b" stroke="#a78bfa" stroke-width="2"/>
<text x="555" y="162" text-anchor="middle" fill="#c4b5fd" font-family="monospace" font-size="14" font-weight="700">RobotContainer.java</text>
<text x="555" y="182" text-anchor="middle" fill="#cbd5e1" font-size="12">Base Swerve, Vision &amp; Autos</text>
<text x="555" y="198" text-anchor="middle" fill="#94a3b8" font-size="11">Used on CORAL</text>
<line x1="660" y1="171" x2="685" y2="171" stroke="#38bdf8" stroke-width="2" stroke-dasharray="4,3" marker-end="url(#m1-arr)"/>
<rect x="690" y="135" width="190" height="72" rx="8" fill="#1e293b" stroke="#a78bfa" stroke-width="2"/>
<text x="785" y="162" text-anchor="middle" fill="#c4b5fd" font-family="monospace" font-size="13" font-weight="700">FuelRobotContainer</text>
<text x="785" y="182" text-anchor="middle" fill="#cbd5e1" font-size="12">Extends RobotContainer</text>
<text x="785" y="198" text-anchor="middle" fill="#94a3b8" font-size="11">Adds 6 FUEL Mechanisms</text>
</svg>
</div>

---

## [`Main.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Main.java)
* **Source:** [`src/main/java/frc/robot/Main.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Main.java)
* **Why it exists:** Standard Java entry point (`public static void main(String... args)`) required by the JVM on the RoboRIO.
* **What it does:** Calls `RobotBase.startRobot(Robot::new)`. You never need to modify this file.

---

## [`Robot.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robot.java)
* **Source:** [`src/main/java/frc/robot/Robot.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robot.java)
* **Why it exists:** Manages the WPILib/AdvantageKit `LoggedRobot` lifecycle (`robotPeriodic`, `autonomousInit`, `teleopInit`) and sets up hardware vs. log replay at boot.
* **What it does:**
  1. **Selects the Robot Hardware (`selectRobot()`):** Reads `RobotController.getComments()` on the RoboRIO. If the comment contains `"CORAL"`, it loads [`Robots.CORAL`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robots.java); otherwise it defaults to [`Robots.FUEL`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robots.java).
  2. **Starts AdvantageKit Logging (`Mode.REAL`, `Mode.SIM`, `Mode.REPLAY`):** Configures `WPILOGWriter` (writing `.wpilog` files to `/home/lvuser/logs/`) and `NT4Publisher` (live telemetry to Elastic / AdvantageScope), or `WPILOGReader` in replay mode.
  3. **Creates Hardware IO & Container:** Instantiates [`SwerveDrive`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java), the array of [`VisionIO`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/vision/VisionIO.java) cameras, and either [`FuelRobotContainer`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FuelRobotContainer.java) (for `FUEL`) or [`RobotContainer`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RobotContainer.java) (for `CORAL`).
  4. **Runs the 20 ms Heartbeat (`robotPeriodic()`):** Every 20 milliseconds, calls `PhoenixSignals.refreshAll()` to batch-read all CAN sensors, runs `CommandScheduler.getInstance().run()`, and records loop timing via [`LoggedTracer`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/LoggedTracer.java).

---

## [`Robots.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robots.java)
* **Source:** [`src/main/java/frc/robot/Robots.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Robots.java)
* **Why it exists:** Keeps all robot-specific swerve and camera hardware constants in one place so [`SwerveDrive`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/SwerveDrive.java) and [`Vision`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Vision.java) work on both `FUEL` and `CORAL` without code changes.
* **What it does:** Defines two static [`DrivetrainConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DrivetrainConfig.java) instances:
  * **`Robots.CORAL`:** `"CANdace"` CAN bus, Pigeon ID `24`, `23.5" × 23.5"` track, 4 corner [`ModuleConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/ModuleConfig.java) definitions, and 1 front `OV9281` camera.
  * **`Robots.FUEL`:** `"Drivetrain"` CAN bus, Pigeon ID `20`, `22.25" × 22.25"` track, 4 corner [`ModuleConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/ModuleConfig.java) definitions, and 3 `OV9281` cameras (`OV9281-2` center, `OV9281-1` left, `OV9281-3` right).

---

## [`RobotContainer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RobotContainer.java)
* **Source:** [`src/main/java/frc/robot/RobotContainer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RobotContainer.java)
* **Why it exists:** Base container that wires together the swerve drivetrain, vision subsystem, driver Xbox controller (Port 0), dashboard pose reset tools, and the autonomous routine chooser. Used directly on `CORAL` and subclassed by [`FuelRobotContainer`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FuelRobotContainer.java) on `FUEL`.
* **What it does:**
  * Sets the default teleop drive command on [`DriveInterface`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/drivetrain/DriveInterface.java), applying joystick sensitivity curves, optional slew-rate limiting, stick clamping, and slow mode.
  * Binds base driver buttons (X-pattern brake, heading reset, pose reset, D-pad snap-to-angle, and SysId characterization).
  * Builds the `Auto Chooser` dropdown from [`Autos.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/Autos.java) and publishes the `Field2d` widget to Elastic.

---

## [`FuelRobotContainer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FuelRobotContainer.java)
* **Source:** [`src/main/java/frc/robot/FuelRobotContainer.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FuelRobotContainer.java)
* **Why it exists:** Extends [`RobotContainer`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RobotContainer.java) with the 6 game-piece mechanisms and dual-controller bindings (Driver Port 0 + Operator Port 1) specific to the `FUEL` robot.
* **What it does:**
  * Instantiates the 6 mechanisms from [`MechanismConfigs.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismConfigs.java): `intake` ([`VelocityMechanism`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java)), `tilter` ([`PositionMechanism`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/PositionMechanism.java)), `indexer` (`VelocityMechanism`), `feeder` (`VelocityMechanism`), `shooter` (`VelocityMechanism`), and `hood` (`PositionMechanism`).
  * Registers PathPlanner `NamedCommands` (`startIntake`, `stopIntake`, `spinUpShooter`, `feed`, `rangeShoot`, `deployIntake`, `stowIntake`, `stopAll`) so autonomous paths can trigger mechanisms at event markers.
  * Configures Scott's Driver layout (Port 0: driving, brake, intake in/out, tilter deploy/retract/jog) and Operator layout (Port 1: feeder, indexer, auto-aim at Hub, Short/Mid/Long shooter range toggles, hood jog, and operator drive override).

---

## Driver Input & Feeling Helpers (`frc.robot`)

| Class | Source Link | Why It Exists & What It Does |
| :--- | :--- | :--- |
| **`PiecewiseSensitivity`** | [`PiecewiseSensitivity.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/PiecewiseSensitivity.java) | Implements a two-segment piecewise linear transfer curve (`Start_X`, `Middle_X`, `Start_Y`, `Middle_Y`, `Max_Y`) so small stick movements give fine low-speed control while full stick deflection gives high speed. |
| **`SensitivityTuner`** | [`SensitivityTuner.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/SensitivityTuner.java) | Wraps [`PiecewiseSensitivity`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/PiecewiseSensitivity.java) with live dashboard sliders (`Drive_*` and `Rot_*`) when `Drive/TunableSens` is enabled on the `Drive Style` tab in Elastic. |
| **`InputProcessing`** | [`InputProcessing.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/InputProcessing.java) | Clamps the combined $(X, Y)$ joystick vector magnitude to $\le 1.0$ (`clampStick`) so pushing the stick diagonally doesn't exceed 100% speed. |
| **`DriverPreferences`** | [`DriverPreferences.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/DriverPreferences.java) | Stores persistent driver settings (`maxSpeedScale`, `maxRotationScale`, `accelLimit`, `rotAccelLimit`, `slowModeScale`) in RoboRIO `Preferences` across reboots. |
