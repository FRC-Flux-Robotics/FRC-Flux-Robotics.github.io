# 3. Mechanisms & Shooting (`frc.lib.mechanism` & `frc.robot`)

Instead of writing separate subsystem classes for every motor on the robot, [`New_Robot`](https://github.com/FRC-Flux-Robotics/New_Robot) uses **two universal mechanism classes** in [`src/main/java/frc/lib/mechanism/`](https://github.com/FRC-Flux-Robotics/New_Robot/tree/main/src/main/java/frc/lib/mechanism) configured by [`MechanismConfigs.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismConfigs.java) and controlled by reusable commands in [`src/main/java/frc/robot/commands/`](https://github.com/FRC-Flux-Robotics/New_Robot/tree/main/src/main/java/frc/robot/commands).

<div style="margin: 1.25rem 0; overflow-x: auto;">
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 900 240" style="width: 100%; max-width: 900px; height: auto; display: block; margin: 0 auto; background: #0f172a; border: 1px solid #334155; border-radius: 10px; font-family: system-ui, -apple-system, sans-serif;">
<defs>
<marker id="m3-arr" viewBox="0 0 10 10" refX="8" refY="5" markerWidth="7" markerHeight="7" orient="auto-start-reverse">
<path d="M 0 1 L 10 5 L 0 9 z" fill="#fb923c"/>
</marker>
</defs>
<rect x="20" y="20" width="275" height="82" rx="8" fill="#1e293b" stroke="#38bdf8" stroke-width="2"/>
<text x="157" y="45" text-anchor="middle" fill="#38bdf8" font-family="monospace" font-size="13" font-weight="700">MechanismConfigs &amp; MechanismConfig</text>
<text x="157" y="65" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">ControlMode &amp; MechanismTuning</text>
<text x="157" y="85" text-anchor="middle" fill="#94a3b8" font-size="11">Defines INTAKE, TILT, INDEXER, FEEDER, SHOOTER, HOOD</text>
<line x1="295" y1="61" x2="325" y2="61" stroke="#fb923c" stroke-width="2" marker-end="url(#m3-arr)"/>
<rect x="330" y="20" width="250" height="82" rx="8" fill="#1e293b" stroke="#4ade80" stroke-width="2"/>
<text x="455" y="45" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">VelocityMechanism.java</text>
<text x="455" y="65" text-anchor="middle" fill="#86efac" font-family="monospace" font-size="14" font-weight="700">PositionMechanism.java</text>
<text x="455" y="85" text-anchor="middle" fill="#cbd5e1" font-size="11">2 Reusable Subsystems for All 6 Mechanisms</text>
<line x1="615" y1="61" x2="585" y2="61" stroke="#fb923c" stroke-width="2" marker-end="url(#m3-arr)"/>
<rect x="620" y="20" width="260" height="82" rx="8" fill="#1e293b" stroke="#facc15" stroke-width="2"/>
<text x="750" y="45" text-anchor="middle" fill="#fde047" font-family="monospace" font-size="13" font-weight="700">ShootCommand &amp; RangeShootCmd</text>
<text x="750" y="65" text-anchor="middle" fill="#cbd5e1" font-family="monospace" font-size="12">SetShooterRangeCmd &amp; VelocityCmd</text>
<text x="750" y="85" text-anchor="middle" fill="#94a3b8" font-family="monospace" font-size="11">RangeTable.java (Distance Interpolation)</text>
<line x1="455" y1="102" x2="455" y2="140" stroke="#fb923c" stroke-width="2" marker-end="url(#m3-arr)"/>
<rect x="20" y="145" width="275" height="72" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="157" y="172" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="13" font-weight="700">MechanismIOTalonFX.java</text>
<text x="157" y="192" text-anchor="middle" fill="#cbd5e1" font-size="12">1-Motor Hardware (Intake, Tilter,</text>
<text x="157" y="207" text-anchor="middle" fill="#cbd5e1" font-size="12">Indexer, Feeder, Hood)</text>
<rect x="330" y="145" width="250" height="72" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="455" y="172" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="14" font-weight="700">MechanismIO.java</text>
<text x="455" y="192" text-anchor="middle" fill="#cbd5e1" font-size="12">@AutoLog Motor Interface</text>
<text x="455" y="207" text-anchor="middle" fill="#94a3b8" font-size="11">+ MechanismIOReplay.java</text>
<rect x="620" y="145" width="260" height="72" rx="8" fill="#1e293b" stroke="#fb923c" stroke-width="2"/>
<text x="750" y="172" text-anchor="middle" fill="#fdba74" font-family="monospace" font-size="13" font-weight="700">MechanismIODualTalonFX.java</text>
<text x="750" y="192" text-anchor="middle" fill="#cbd5e1" font-size="12">2 Counter-Rotating Motors</text>
<text x="750" y="207" text-anchor="middle" fill="#94a3b8" font-size="11">Shooter Leader ID 11 + Follower ID 10</text>
<line x1="295" y1="181" x2="325" y2="181" stroke="#fb923c" stroke-width="2" stroke-dasharray="4,3" marker-end="url(#m3-arr)"/>
<line x1="620" y1="181" x2="585" y2="181" stroke="#fb923c" stroke-width="2" stroke-dasharray="4,3" marker-end="url(#m3-arr)"/>
</svg>
</div>

---

## Reusable Mechanism Subsystems (`frc.lib.mechanism`)

### [`VelocityMechanism.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java)
* **Source:** [`src/main/java/frc/lib/mechanism/VelocityMechanism.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java)
* **Why it exists:** Universal subsystem for any roller, wheel, belt, or flywheel controlled by target speed in **Rotations Per Second (`RPS`)**. Used for **4 subsystems on `FUEL`**: `intake`, `indexer`, `feeder`, and `shooter`.
* **What it does:**
  * Calls `m_io.updateInputs(m_inputs)` and `Logger.processInputs(m_config.name, m_inputs)` every 20 ms, plus logs `TargetVelocity` and `AtTarget`.
  * Exposes `setVelocity(velocityRPS)`, `stop()`, `getVelocity()`, and `atTarget()` (returns `true` when `|currentVelocity - targetVelocity| <= tolerance`).

### [`PositionMechanism.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/PositionMechanism.java)
* **Source:** [`src/main/java/frc/lib/mechanism/PositionMechanism.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/PositionMechanism.java)
* **Why it exists:** Universal subsystem for any pivot, arm, elevator, or hood controlled to a target angle/position in **motor rotations (`rot`)**. Used for **2 subsystems on `FUEL`**: `tilter` (intake pivot) and `hood` (shooter elevation).
* **What it does:**
  * Logs `TargetPosition` and `AtTarget` via AdvantageKit every 20 ms.
  * Exposes `setPosition(positionRotations)` (automatically uses MotionMagic if configured, or standard closed-loop `PositionVoltage`), `jogUp()`, `jogDown()`, `stop()`, `resetPosition()`, and `atTarget()`.

---

## AdvantageKit Mechanism IO Layer (`frc.lib.mechanism`)

| Class | Source Link | Why It Exists & What It Does |
| :--- | :--- | :--- |
| **`MechanismIO`** | [`MechanismIO.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIO.java) | Hardware abstraction interface defining `@AutoLog class MechanismIOInputs` (`positionRotations`, `velocityRPS`, `statorCurrentA`, `supplyCurrentA`, `appliedVoltage`, `tempCelsius`, `motorConnected`) and control methods (`setPosition`, `setMotionMagicPosition`, `setVelocity`, `stop`, `resetPosition`). |
| **`MechanismIOTalonFX`** | [`MechanismIOTalonFX.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIOTalonFX.java) | Real single-motor CTRE `TalonFX` implementation. Applies PID/feedforward gains, voltage limits, supply/stator current limits, and hardware soft limits from [`MechanismConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismConfig.java) (with 5-attempt retry via [`PhoenixUtil`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/PhoenixUtil.java)), and registers status signals with [`PhoenixSignals`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/util/PhoenixSignals.java). |
| **`MechanismIODualTalonFX`** | [`MechanismIODualTalonFX.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIODualTalonFX.java) | Real dual-motor `TalonFX` implementation (used by `FUEL`'s dual shooter: Leader ID `11` + Follower ID `10`). Supports both aligned followers and counter-rotating (`Opposed`) followers, averaging velocity and summing currents across both motors. |
| **`MechanismIOReplay`** | [`MechanismIOReplay.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismIOReplay.java) | No-op implementation used during AdvantageKit log replay (`Mode.REPLAY`). |
| **`MechanismConfig` & `ControlMode`** | [`MechanismConfig.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismConfig.java)<br>[`ControlMode.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/ControlMode.java) | Immutable builder configuration (`name`, `motorId`, `secondMotorId`, `canBus`, `pidGains`, `ControlMode.POSITION` or `VELOCITY`, `peakVoltage`, `supplyCurrentLimit`, `statorCurrentLimit`, `jogStep`, `softLimitForward`, `softLimitReverse`, MotionMagic parameters). |

---

## `FUEL` Mechanism Configs, Tuning & Shooting (`frc.robot` & `frc.robot.commands`)

| Class | Source Link | Why It Exists & What It Does |
| :--- | :--- | :--- |
| **`MechanismConfigs`** | [`MechanismConfigs.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismConfigs.java) | Declares the 6 static [`MechanismConfig`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/MechanismConfig.java) definitions on the `"Mech"` CANivore bus for `FUEL`: `INTAKE` (ID `1`), `TILT` (ID `20`), `INDEXER` (ID `4`), `FEEDER` (ID `3`), `SHOOTER` (IDs `11` & `10`), and `HOOD` (ID `12`). |
| **`MechanismTuning`** | [`MechanismTuning.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismTuning.java) | Publishes live-tunable mechanism speeds and hood/tilter positions to the `Tuning` tab in Elastic (`Tuning/IntakeInRPS`, `IndexerRPS`, `FeederRPS`, `ShooterDefaultRPS`, `HoodShort`, `HoodMid`, `HoodLong`, and `Save` button to persist to RoboRIO `Preferences`). |
| **`RangeTable`** | [`RangeTable.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RangeTable.java) | Lookup table mapping distance from the scoring Hub (in meters) to `(speed, elevation)` for the shooter and hood. |
| **`VelocityCmd`** | [`VelocityCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/VelocityCmd.java) | Runs any [`VelocityMechanism`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java) (`intake`, `indexer`, `feeder`) at a supplied `DoubleSupplier` speed (RPS) while held or until an optional timeout, then calls `stop()` in `end()`. |
| **`ShootCommand`** | [`ShootCommand.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/ShootCommand.java) | Spins up the `shooter` [`VelocityMechanism`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/lib/mechanism/VelocityMechanism.java) to a target RPS and stops it when cancelled or toggled off. |
| **`SetShooterRangeCmd`** | [`SetShooterRangeCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/SetShooterRangeCmd.java) | Sets both `shooter` velocity (RPS) and `hood` angle (rotations) to the Short (`0`), Medium (`1`), or Long (`2`) preset from [`MechanismTuning`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/MechanismTuning.java); stops the shooter and returns the hood to `0` when toggled off. |
| **`RangeShootCmd`** | [`RangeShootCmd.java`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/commands/RangeShootCmd.java) | Full automatic distance-based shooting command: calculates distance from `drive.getPose()` to [`FieldPositions.resolve("HUB")`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/FieldPositions.java), sets `shooter` RPS and `hood` elevation from [`RangeTable`](https://github.com/FRC-Flux-Robotics/New_Robot/blob/main/src/main/java/frc/robot/RangeTable.java), and **waits until `m_shooter.atTarget()` is true** before running `feeder` and `indexer`. |
