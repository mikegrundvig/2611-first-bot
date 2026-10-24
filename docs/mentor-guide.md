# Mentor guide

How to take beginners from "never cloned a repo" to driving the swerve chassis with their own code. Sessions are suggestions, not a schedule: move on when the room is ready.

**The links**

- [Slides: How software gets made](slides.html) (press N for the speaker notes, F for full screen)
- [Installs and links](links.html): what each laptop needs, with download links
- [Robot code cheat sheet](cheat-sheet.html)
- [GitHub Desktop cheat sheet](github-desktop.html)
- The code: https://github.com/mikegrundvig/2611-first-bot
- Bench guide: https://github.com/mikegrundvig/frc-robot-runtime/blob/main/docs/bench.md

**The idea in one line:** every session ends with something they changed moving on the real chassis.

---

## Before the first session (you, alone)

### Accounts
- [ ] Every student has a GitHub account (their own email; school accounts are fine).
- [ ] Add each one as a collaborator on 2611-first-bot (Settings › Collaborators), or have them fork it. Collaborators must accept the email invite.

### Student laptops (Windows)
- [ ] Java 25 (Eclipse Temurin JDK, the Windows .msi: https://adoptium.net/temurin/releases/?version=25), with **Set JAVA_HOME variable** turned on in the installer (it's off by default). Gradle needs a Java just to start; without it `gradlew.bat` stops with `JAVA_HOME is not set`. Gradle itself needs no install.
- [ ] Git for Windows: optional, only for typing `git` commands. GitHub Desktop brings its own.
- [ ] GitHub Desktop, signed in (File › Options › Accounts).
- [ ] An editor: IntelliJ IDEA (preferred) or WPILib's VS Code. VS Code: accept the recommended extensions on first open, then restart VS Code once.
- [ ] Clone 2611-first-bot and run `gradlew.bat build` once **on good internet**. The first build downloads about 1 GB (Gradle, Java 25, WPILib). Ten laptops doing this at once on shop Wi‑Fi will eat the session.
- [ ] Check memory: IntelliJ wants 8 GB or more. Under 8 GB, use VS Code.

### The bench laptop (yours, or one mentor laptop)
- [ ] Windows, with Phoenix Tuner X **Preview** (2027 version, Microsoft Store: https://apps.microsoft.com/detail/9n17ns6nm06v; it installs beside the regular Tuner X).
- [ ] 2611-first-bot cloned and built.
- [ ] This is the only laptop that talks to the chassis. Students push their branch; the bench laptop pulls it and runs it.

### The chassis
- [ ] On blocks, wheels clear of everything. Battery charged.
- [ ] CANivore on the bench laptop's USB; Tuner X lists it. Note its name (code assumes `canivore`).
- [ ] Every device on Phoenix firmware **26.70.0.0** (Tuner X lists the files as `<device>-26.70.0.0-PostSeason2026.crf`), flashed with Phoenix Tuner X **Preview 2026.10.x** (the regular Tuner X doesn't offer it), to match the code's Phoenix 6 library **26.70.0-alpha-2**. Then **factory default** each one (clears last season's settings and the FRC lock).
- [ ] In Tuner X, read the IDs and compare with the top of `Step1OneMotor.java` and `ChassisRobot.java`: drive 23, steer 24, CANcoder 12 (front left), Pigeon 1. Fix the code if different.
- [ ] **Run Step 1 yourself first.** `gradlew.bat run -PhwSim`, drag the gamepad onto Joystick 0, click Teleoperated, push the stick: the front-left wheel spins at 20%. If it doesn't, fix it now, not in front of the room.
- [ ] Run Step 2 and turn the front-left wheel by hand: the printed angle should change, and read near 0 pointing straight ahead.
- [ ] Run Step 3, press A. The wheel should turn and **settle**. If it keeps spinning, the steering and the CANcoder disagree on direction: stop, and fix before showing students.
- [ ] Test that a breakpoint stops the motors (IntelliJ: Debug instead of Run, breakpoint in `teleopPeriodic`): the wheel should stop within a fraction of a second.

### Safety, every time the chassis runs
- One person's hand on the main breaker.
- Wheels off the ground, nobody's fingers near modules.
- Disable = click **Disabled** in the simulator window. Ctrl+C in the terminal also stops it.
- Only the bench laptop runs `-PhwSim`.

---

## Session 1: the big picture and setup (about 90 minutes)

### Talk (25–30 min)
- [ ] Present the deck. The speaker notes have what to say. Keep it moving: the goal is that the words are familiar, not memorized.
- [ ] Hand out (or link) both cheat sheets.

### Hands on: get the code (30 min)
Pairs are fine. Walk the room.

1. GitHub Desktop: **File › Clone repository › URL**, paste `https://github.com/mikegrundvig/2611-first-bot`, local path `Documents\GitHub`.
2. **Repository › Show in Explorer**: look at the folder. Point at `src/main/java/frc/robot`. This is the "a project is a folder" slide made real.
3. Open the folder in the editor (Repository › Open in…).
4. Open `Main.java` and `Step1OneMotor.java`. Read Step 1 together, line by line. It's 15 lines.

**Done when:** every laptop has 2611-first-bot open with Step 1 showing.

### First run on the chassis (20 min)
1. On the bench laptop: run Step 1 (`Run on chassis` task/button, or `gradlew.bat run -PhwSim`).
2. Before anyone pushes the stick, **ask: what will happen? Which wheel? How fast?** (Predict first; it's the whole method.)
3. Students take turns on the gamepad.
4. **Ask:** the code says `* 0.2`. What would `* 0.4` do? Why might we not want that on blocks?

**Done when:** they've seen code they read move a real wheel.

---

## Session 2: change it, commit it, see it (about 90 minutes)

Each pair makes one change on their own branch, pushes it, and watches it run on the chassis. Git now has a reason to exist: it's how their code reaches the bench laptop.

### Each pair
1. GitHub Desktop: make sure Current Branch is `main`, click **Fetch origin** (and Pull if it asks).
2. **New Branch**: name it `pairname-step1`, e.g. `sam-ana-step1`.
3. In `Step1OneMotor.java`, make one small change. Ideas, easiest first:
   - Change `0.2` to `0.1` (half speed). Predict first.
   - Make pushing the stick forward spin it the other way.
   - Use the right stick instead of the left.
   - Make the A button spin it at 10% while held (`gamepad.getAButton()`).
4. Build it in the editor (no red underlines). In a terminal: `gradlew.bat build`.
5. GitHub Desktop: **Changes** tab, look at the diff. Write a real summary ("Spin at half speed"). **Commit to pairname-step1**.
6. **Publish branch**.

### At the bench (you drive the bench laptop)
1. GitHub Desktop on the bench laptop: Fetch origin, Current Branch › their branch.
2. Run on chassis. The pair predicts out loud, then drives.
3. Did it do what they predicted? If not, why not? (This is the debugging lesson.)

### Then a pull request
1. Pick one pair's branch. They click **Preview Pull Request**, base `main`, create it.
2. On the projector: read the diff together, leave a comment or two (be kind and specific), show the green check once CI runs.
3. Merge that one. Close the others with a "nice work" comment (they were practice).

**Done when:** every pair has a commit on GitHub that ran on the real chassis.

---

## Session 3: read a sensor (Step 2)

1. Each pair: switch to `main`, fetch and pull, new branch `pairname-step2`.
2. In `Main.java`, change `Step1OneMotor` to `Step2ReadASensor`. That one line picks the step.
3. Read Step 2 together. **Ask:** what number will it print with the wheel straight ahead? Turned a quarter turn?
4. Bench: run it, turn the wheel by hand, watch the console and the simulator window's Telemetry.
5. Change ideas:
   - Print in rotations instead of degrees.
   - Print only when the angle changes by more than 5 degrees.
   - Print "LEFT" or "RIGHT" depending on which way it's turned.
6. Commit, publish, run at the bench, one PR reviewed on screen.

---

## Session 4: point the wheel (Step 3)

1. New branch, `Main.java` to `Step3PointTheWheel`.
2. Read it. New ideas: configuring a motor, a closed loop (the motor watches the sensor and corrects itself), current limits.
3. **Ask:** what does `kP` do? Predict what happens if it's 5 instead of 20. And 60? (Try 5 and 40 on blocks; watch for slow vs. jittery.)
4. Change ideas:
   - X button points it 45 degrees, Y button 180.
   - Make the current limit 10 A and try to stop the wheel by hand (gloves, carefully, mentor decides).
5. Commit, publish, bench, PR.

---

## Session 5: one whole module (Step 4)

1. New branch, `Main.java` to `Step4OneModule`.
2. Read it. The new idea is trigonometry: `atan2` turns the stick's direction into an angle. Draw it on a whiteboard.
3. **Ask:** push the stick straight left. Which way does the wheel point? Which way does it spin?
4. Change ideas:
   - Slow mode: holding the left bumper limits it to 5%.
   - Make letting go point the wheel straight ahead.
5. Commit, publish, bench, PR.

---

## Session 6: the whole chassis (`swerve-starter`)

1. Clone the whole-chassis project (published separately when the team gets there).
2. Open `ChassisRobot.java`. Point out it describes what they did by hand in Steps 1–4, times four modules: IDs, zero, gear ratio, current limits. The library does the math they just did.
3. Walk the **Unconfirmed** block at the top with Tuner X open on the bench laptop. Students check each ID and mark it "Checked" with the date. That's a real commit.
4. Bench: `gradlew.bat run -PhwSim`. Drive it on blocks. Then, mentor's call, on the floor with the bench limits (1.5 m/s).
5. `Controls.java`: add the wheel lock on the right trigger (the example in its comment). Commit, PR.

---

## When things go wrong

| Symptom | Likely cause | Fix |
|---|---|---|
| Red underline, build fails | Typo or missing `;` | Read the **first** error; it names the file and line |
| `JAVA_HOME is not set` | No Java, or the terminal predates the install | Install Java 25 (Temurin .msi) with Set JAVA_HOME on; reopen the terminal |
| `wpiutiljni could not be loaded` | Started from the Run link beside `main` | Use the run task/button or `gradlew.bat run` |
| First build takes forever | Downloading ~1 GB | Do it before the session, on good internet |
| `CANivore … not seen` | USB, or name mismatch | Check cable, Tuner X lists it, name matches the code |
| `CAN frame not received` | Wrong CAN ID or no power | Compare IDs with Tuner X |
| Enabled, nothing moves | FRC lock or old firmware | Factory default; firmware 26.70.0.0 (PostSeason2026) |
| Step 3 wheel spins and never settles | Steering motor and CANcoder disagree on direction | Disable. Turn the wheel by hand and watch the CANcoder (Step 2) and the steer motor's position (Tuner X): they must move the same way. Invert the one that's wrong (motor `Inverted` or CANcoder `SensorDirection`) |
| Gamepad does nothing | Not on Joystick 0 | Drag it onto Joystick 0 in the simulator window |
| Push rejected in GitHub Desktop | Someone pushed first | Fetch, Pull, then Push |
| "Conflict" when pulling or merging | Two people changed the same lines | Do it together on screen the first time |
| Student can't see the repo | Invite not accepted | Check their email for the GitHub invite |

---

## What to watch for

- **Who comes back with a question nobody assigned.** That student gets more of your time: pair with them, give them a piece to own.
- **Predict before every run.** If they skip it, the run teaches nothing.
- **Small wins every session.** If a session is running long, cut the change ideas, never the bench run.
- **Don't fix it for them.** Ask "what does the error say?" first. Read it out loud together.
