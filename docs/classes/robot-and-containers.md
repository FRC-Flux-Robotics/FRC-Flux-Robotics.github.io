# 1. Boot, Robot Configs & Containers (`frc.robot`)

These classes handle **starting the robot program**, **detecting which physical robot (`FUEL` or `CORAL`) is running**, **wiring up subsystems and Xbox controllers**, and **shaping joystick inputs**.

```mermaid
flowchart LR
    Main["Main.java"] --> Robot["Robot.java"]
    Robot -->|selectRobot()| Robots["Robots.java\n(FUEL / CORAL)"]
    Robot -->|CORAL| RC["RobotContainer.java"]
    Robot -->|FUEL| FRC["FuelRobotContainer.java"]
    FRC -.->|extends| RC
    Sens["PiecewiseSensitivity\nSensitivityTuner\nInputProcessing\nDriverPreferences"] --> RC
```

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
