# A first robot

Course pages, slides and cheat sheets: https://mikegrundvig.github.io/2611-first-bot/

Four very short robot programs for the swerve test chassis. Each one adds one idea to the last:

| Step | File | What it does |
|---|---|---|
| 1 | `Step1OneMotor.java` | The left stick spins one wheel. |
| 2 | `Step2ReadASensor.java` | Prints which way that wheel points while you turn it by hand. |
| 3 | `Step3PointTheWheel.java` | Buttons A and B point the wheel straight ahead or sideways. |
| 4 | `Step4OneModule.java` | The left stick steers and spins the wheel: one whole swerve module. |
| 5 | coming next | The whole chassis, all four modules, driving for real. |

They are in `src/main/java/frc/robot/`. Each starts with a few sentences saying what to try.

## Get it

```bash
git clone https://github.com/mikegrundvig/2611-first-bot.git
```

## Pick a step

Open `src/main/java/frc/robot/Main.java`. It has one line that names the step to run:

```java
RobotBase.startRobot(Step1OneMotor::new);
```

Change `Step1OneMotor` to another step's name, like `Step2ReadASensor`, and run again.

## In VS Code

Open the folder in WPILib's VS Code (**File > Open Folder**). It needs Microsoft's Java extensions
and the WPILib extension: if VS Code offers to install the recommended extensions, say yes, then
close and reopen VS Code once (the WPILib **W** button only shows up after that restart).

To run it, pick **Terminal > Run Task**, then **Run on chassis** or **Run in simulator**. For the
simulator, WPILib's own command works too: **WPILib: Simulate Robot Code**.

Don't use the **Run** link above `main`: it starts Java without WPILib's libraries, and it stops
with `wpiutiljni could not be loaded`.

## In IntelliJ IDEA

Open the folder with **File > Open**. **Run on chassis** and **Run in simulator** are in the run
menu at the top. Don't use the green arrow beside `main`, for the same reason as above.

## Run it on the chassis

This spins real motors, so a mentor sets it up, the first time following the runtime's
[bench guide](https://github.com/mikegrundvig/frc-robot-runtime/blob/main/docs/bench.md):

1. The chassis is **on blocks**, its wheels clear of everything, a hand near the main breaker.
2. Every device is on **Phoenix 26.70.x** firmware and has been **factory-defaulted** in Phoenix
   Tuner X.
3. The chassis's **CANivore is plugged into the laptop's USB**.
4. Run **Run on chassis** (or type `gradlew.bat run -PhwSim` in a terminal).
5. In the simulator window that opens, drag your gamepad onto **Joystick 0**, then click
   **Teleoperated** to enable. Click **Disabled** to stop.

The CAN IDs and other numbers at the top of each step are copied from another robot. If nothing
moves, check them in Phoenix Tuner X.

## Run it in the simulator

**Run in simulator** (or `gradlew.bat run`) runs the same program with pretend motors, no robot
needed. Nothing moves that you can see, but you can check that a step starts and enables.

## When you open a pull request

Two checks run on every pull request, and both must pass before it can merge:

- **Build** compiles the code and runs it through Gradle on Linux and on Windows. A red X here
  means it doesn't compile: open the check, find the first error, and fix that line.
- **AI review** reads your change like a mentor and comments on it. Each comment starts with
  **Must fix** or **Suggestion**. While any must-fix is open, the check fails and the pull request
  can't merge. Its summary comment lists what's left. Suggestions are ideas, and never block.

To fix something, push new commits to the same branch. Both checks run again, and the review says
which points are now fixed. Mark each comment's conversation **Resolved** once you've dealt with
it. A mentor still reviews and merges.
