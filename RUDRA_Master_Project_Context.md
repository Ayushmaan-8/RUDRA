# RUDRA — Master Project Context & Development Plan

> **Purpose of this file:** A continuity document for the RUDRA MMA teaching app. It consolidates the decisions, architecture, development roadmap, previous implementation work, debugging history, and the agreed way of working from the conversations and uploaded RUDRA documents available in this chat.
>
> **Important date note:** The uploaded roadmap is labeled “Revised as of 2 Nov 2025” and targets an MVP by **20 Nov 2025**. This file preserves that project timeline exactly as documented rather than silently changing it.

---

## 1. What RUDRA Is

**RUDRA** is an **AI-driven MMA teaching/training app** built primarily in **Flutter/Dart**.

The core MVP idea is:

**Phone camera → human pose detection → pose/keypoint analysis → strike/motion analysis → real-time coaching feedback → session statistics/progress**

The app is intended to provide on-device, real-time training assistance rather than merely being a generic fitness tracker.

### MVP outcome

By **20 November**, the target is a functional demonstrable MVP that can:

- Detect human pose in real time
- Display a skeleton/pose overlay
- Provide strike feedback cues
- Show training statistics and session summaries
- Run offline on the test phone

The uploaded roadmap specifically defines the MVP around camera integration, pose detection, motion analysis, feedback, session flow, local progress tracking, testing, and a demo build.

---

# 2. Current Development Philosophy

The current plan is **not** to build the entire future RUDRA product at once.

The agreed approach is:

1. Start clean.
2. Build the MVP step by step.
3. Log what is completed.
4. When something breaks, debug that specific issue before moving forward.
5. Avoid unnecessary learning curves or architecture expansion while the MVP deadline is the priority.
6. Add future functionality only after the core MVP is stable.

The immediate focus is therefore **working Flutter/Dart code and integration**, not building an elaborate backend or production-scale system.

---

# 3. Technology Stack

## Frontend

- Flutter
- Dart
- Android Studio

## Computer Vision / ML

- Google ML Kit Pose Detection
- TensorFlow Lite is listed in the roadmap as part of the ML/CV layer
- Camera plugin

## Camera

- `camera`
- `camera_android_camerax` may be involved depending on the selected plugin/version setup

## Local MVP Storage

The revised roadmap specifies:

- `SharedPreferences` for local MVP progress/session data

The older development log also contains work/plans around:

- Hive
- `hive_flutter`
- `path_provider`

These should **not automatically override the revised roadmap**. The revised roadmap is the latest project direction. Use the simplest storage approach that matches the current implementation and MVP requirements.

## Future backend

The roadmap lists:

- Node.js
- PostgreSQL

Firebase was also explored/prepared in the previous implementation:

- `firebase_core`
- `firebase_auth`
- `cloud_firestore`

Firebase/backend/cloud sync are **future/secondary**, not the core MVP blocker.

## Test device

**Realme GT Neo 3T**

---

# 4. Revised MVP Roadmap

## Phase 0 — Foundation & Setup

**2 Nov – 3 Nov**

### Objective

Establish a clean, stable development environment and verify that the camera and ML modules work before building the application around them.

### 2 Nov — Setup Cleanup & Environment Check

Tasks:

- Delete old builds/caches.
- Update/check Flutter SDK.
- Check Android Studio.
- Verify plugin versions.
- Ensure ADB recognizes the Realme GT Neo 3T.
- Run `flutter doctor`.
- Ensure the development environment is stable before adding new application functionality.

### 3 Nov — Camera & ML Kit Integration Test

Create a fresh Flutter project:

`rudra_mvp`

Tasks:

- Implement the `camera` plugin.
- Show a live camera preview.
- Test `google_mlkit_pose_detection`.
- Verify that pose keypoints are detected.
- Render a basic live skeleton overlay.

**Deliverable:**
A basic live camera + pose skeleton running on the physical device.

---

# 5. Phase 1 — MVP Core Development

**4 Nov – 20 Nov**

## 4–6 Nov — App UI Framework

Build:

- Splash Screen
- Login/Signup
- Dashboard
- Training Start page

Design direction:

- Dark theme
- RUDRA red palette

The UI should be functional and clean enough for the MVP. Avoid spending excessive time polishing details before the core training pipeline works.

---

## 7–9 Nov — Camera + Pose Module

Finalize:

- Camera preview
- Camera overlay
- Pose detection
- Real-time joints
- Real-time skeleton

Target:

- Approximately **<200 ms latency**

Technology:

- ML Kit / camera pipeline

---

## 10–11 Nov — Motion Analysis

Implement basic punch/motion metrics.

Initial metrics explicitly identified:

- Hip rotation
- Arm extension

Build reusable functions for:

- Joint angles
- Velocity from pose points

Display useful metrics on screen during development/testing.

---

## 12–13 Nov — Feedback Engine / SQI v1

Implement the first version of the:

**Strike Quality Index (SQI)**

The feedback engine should convert detected movement/pose information into simple coaching instructions.

Example feedback:

- “Rotate hips more”
- “Keep guard up”

The first version does not need to be a perfect AI coach. It needs to be:

- Understandable
- Deterministic enough to test
- Fast enough for real-time feedback
- Demonstrable

---

## 14–15 Nov — Session Flow & Controls

Implement:

- Start training
- Pause training
- Stop training

Track session information such as:

- Strike count
- Average SQI

Store session information locally.

---

## 16–17 Nov — Progress Dashboard

Build the progress screen.

Target elements:

- XP bar
- Streaks
- Charts
- Session time
- Average SQI

Use local storage for MVP data.

The revised roadmap specifically names:

**SharedPreferences**

for local MVP storage.

---

## 18–19 Nov — Testing & Optimization

Test on the actual phone.

Check:

- Camera lag
- Pose detection latency
- Crashes
- Memory issues
- UI responsiveness
- Camera frame rate
- Feedback responsiveness

Optimize before the final demo.

---

## 20 Nov — Final MVP

Final deliverables:

- Working MVP build
- Demo video
- Camera feed
- Pose overlay
- Real-time feedback
- Training/session statistics
- Submission package
- GitHub repository

---

# 6. Target Architecture

The conceptual MVP pipeline is:

```text
                 ┌──────────────────────┐
                 │     Flutter App      │
                 │      UI / Screens     │
                 └──────────┬───────────┘
                            │
                            ▼
                 ┌──────────────────────┐
                 │   Camera Service     │
                 │ CameraController     │
                 └──────────┬───────────┘
                            │ CameraImage
                            ▼
                 ┌──────────────────────┐
                 │   Pose Service       │
                 │ Google ML Kit Pose   │
                 └──────────┬───────────┘
                            │ Pose / landmarks
                            ▼
                 ┌──────────────────────┐
                 │ Motion Analysis      │
                 │ angles / velocity    │
                 │ hip rotation         │
                 │ arm extension        │
                 └──────────┬───────────┘
                            │ metrics
                            ▼
                 ┌──────────────────────┐
                 │ Feedback Engine      │
                 │ SQI v1               │
                 └──────────┬───────────┘
                            │
             ┌──────────────┴──────────────┐
             ▼                             ▼
   ┌───────────────────┐        ┌───────────────────┐
   │ Real-time Feedback│        │ Session / Progress│
   │ text / overlay    │        │ local storage     │
   └───────────────────┘        └───────────────────┘
```

---

# 7. Planned Folder Structure

The revised roadmap proposes:

```text
rudra_mvp/

├── lib/
│   ├── main.dart
│   │
│   ├── screens/
│   │   ├── splash_screen.dart
│   │   ├── login_screen.dart
│   │   ├── dashboard_screen.dart
│   │   ├── training_mode_screen.dart
│   │   ├── live_training_screen.dart
│   │   └── session_summary_screen.dart
│   │
│   ├── widgets/
│   │   ├── rudra_button.dart
│   │   ├── stat_tile.dart
│   │   └── feedback_overlay.dart
│   │
│   ├── models/
│   │   ├── session_model.dart
│   │   └── feedback_model.dart
│   │
│   └── services/
│       ├── pose_service.dart
│       ├── camera_service.dart
│       └── feedback_engine.dart
│
├── assets/
│   ├── icons/
│   └── images/
│
├── android/
├── ios/
├── pubspec.yaml
└── README.md
```

The previous project used a somewhat different structure under `ui/`, `services/`, `models/`, and `utils/`. Because the roadmap was revised, we should not blindly preserve both structures. When restarting, we should choose one clean structure and keep it consistent.

---

# 8. Previous Development — What Was Already Done

The previous development log, covering work up to **23 Oct 2025**, records substantial groundwork.

## Project setup

Previously created:

- Flutter project: `rudra_app`
- Initial Git/GitHub setup was recommended
- Initial project folder structure

## UI scaffolding

Previously created/drafted:

- `main.dart`
- Login screen
- Landing screen
- Training screen
- Progress screen
- Profile screen
- Reusable widgets
- Theme

## Theme

An app theme was created around:

- Dark UI
- RUDRA red palette

## Camera

`camera_service.dart` was implemented.

The previous log says the camera preview was working on the device after the relevant environment/build configuration.

## Pose detection

`pose_service.dart` went through several ML Kit API iterations.

The previous log records a final approach aligned with:

- `google_mlkit_pose_detection: ^0.11.0`
- `google_mlkit_commons: ^0.7.1`

The newer ML Kit input pipeline used:

- `InputImage.fromBytes`
- `InputImageMetadata`
- `InputImageFormatValue.fromRawValue`
- camera rotation conversion

Older APIs such as:

- `InputImageData`
- `InputImagePlaneMetadata`

caused errors and were replaced.

## Training screen

A training screen was implemented around:

- Camera preview
- Pose detection initialization
- Placeholder overlay/feedback

The complete skeleton visualization was still pending.

## Motion utilities

An `angle_utils.dart` helper was drafted for joint-angle calculation.

## Models

A session model was drafted containing concepts such as:

- session ID
- date
- average SQI
- errors

A user model was also drafted.

## Storage

Hive was explored and a storage service skeleton was created.

## Firebase

Firebase authentication and Firestore synchronization were prepared/drafted.

However, these were not the main MVP priority in the revised roadmap.

---

# 9. Previous Implementation Files

The previous development log lists these important files:

```text
lib/
├── main.dart
│
├── ui/
│   ├── screens/
│   │   ├── login_screen.dart
│   │   ├── landing_screen.dart
│   │   ├── training_screen.dart
│   │   ├── progress_screen.dart
│   │   └── profile_screen.dart
│   │
│   ├── widgets/
│   │   ├── custom_button.dart
│   │   └── app_bar.dart
│   │
│   └── theme/
│       └── app_theme.dart
│
├── services/
│   ├── auth_service.dart
│   ├── camera_service.dart
│   ├── pose_service.dart
│   └── storage_service.dart
│
├── models/
│   ├── user_model.dart
│   ├── session_model.dart
│   └── feedback_model.dart
│
└── utils/
    ├── constants.dart
    └── angle_utils.dart
```

These represent the previous implementation/history, not necessarily the exact structure we must retain after the restart.

---

# 10. Dependency History

The previous log records these versions as used/tested:

```yaml
camera: ^0.10.5
google_mlkit_pose_detection: ^0.11.0
google_mlkit_commons: ^0.7.1

firebase_core: ^3.0.0
firebase_auth: ^5.0.0
cloud_firestore: ^5.0.0

hive: ^2.2.3
hive_flutter: ^1.1.0

permission_handler: ^11.0.1
fl_chart: ^0.65.0
path_provider: ^2.1.2
google_fonts: ^6.1.0
```

The previous log also says that `camera ^0.11.0` was tested during the evolution of the project.

### Important

Do **not** assume these dependency versions are automatically still correct simply because they appeared in the old log.

At the restart:

1. Check the installed Flutter/Dart environment.
2. Establish a clean project.
3. Choose compatible package versions.
4. Run `flutter pub get`.
5. Build a minimal app.
6. Add packages incrementally.

This is deliberately safer than throwing the entire old dependency set into the project at once.

---

# 11. Previous Android Environment Problems

This is extremely important because the previous project became stuck in debugging.

The main objective for the restart is:

> **Do not recreate the previous dependency/Gradle/SDK mess unnecessarily.**

The previous log records these issues.

## Problem 1 — Flutter not recognized

Error:

```text
flutter : The term 'flutter' is not recognized...
```

Resolution:

- Flutter `bin` was added to Windows PATH.
- Terminal was restarted.

---

## Problem 2 — Windows symlink/plugin installation

Resolution:

- Windows Developer Mode was enabled.

---

## Problem 3 — ML Kit API mismatch

Older ML Kit code referenced APIs that no longer matched the installed `google_mlkit_commons` version.

Resolution:

- `google_mlkit_commons ^0.7.1`
- `google_mlkit_pose_detection ^0.11.0`
- Reworked `pose_service.dart` around the newer `InputImageMetadata` approach.

---

## Problem 4 — NDK mismatch

The project encountered conflicts between requested and installed NDK versions.

Versions appearing in the old debugging history include:

- NDK 27.0.12077973
- NDK 29.0.14206865

The previous project eventually worked around this by aligning the Android project with the installed/requested NDK.

### Restart rule

Do not randomly change `ndkVersion`.

First inspect:

- installed NDK
- Flutter's Android requirements
- plugin requirements
- project Gradle configuration

Then make one deliberate change.

---

## Problem 5 — Android API 36 / compileSdk mismatch

The previous project encountered:

```text
Failed to find Platform SDK with path: platforms;android-36
```

The project eventually moved toward:

```kotlin
compileSdk = 36
```

and Android 16/API 36 installation.

### Restart rule

Do not change compileSdk repeatedly in response to individual errors.

First determine the package requirements and installed SDKs, then configure the project consistently.

---

## Problem 6 — PowerShell vs CMD commands

Some cleanup commands failed because commands intended for one shell were used in another.

Examples:

PowerShell:

```powershell
Remove-Item -Recurse -Force .gradle
```

CMD:

```cmd
rd /s /q .gradle
```

We should always identify the shell before giving destructive cleanup commands.

---

## Problem 7 — Plugin version compatibility

Some newer plugins required higher Android SDK levels.

The previous log records camera/lifecycle/path_provider compatibility issues.

### Restart rule

Avoid adding every plugin at the beginning.

Add dependencies according to the MVP module being built.

---

## Problem 8 — Stale Gradle/build caches

The project sometimes retained old configuration after edits.

Previous cleanup included:

```bash
flutter clean
```

and removal of:

```text
.gradle
.dart_tool
build
```

### Restart rule

Use cleanup when the evidence indicates stale configuration.

Do not repeatedly delete caches blindly.

---

# 12. Critical Restart Strategy

The previous project had accumulated:

- package changes
- Android SDK changes
- NDK changes
- Gradle changes
- ML Kit API changes
- multiple experiments

That made debugging harder.

The restart should therefore follow a **controlled incremental integration strategy**.

## Rule 1 — Start minimal

Create:

```text
rudra_mvp
```

and prove that a minimal Flutter Android application builds and runs.

## Rule 2 — Verify the environment before dependencies

Run:

```bash
flutter doctor -v
flutter --version
dart --version
adb devices
```

Do not start writing RUDRA features until the environment is understood.

## Rule 3 — Add one subsystem at a time

Recommended sequence:

```text
Flutter app
    ↓
Camera
    ↓
Camera on physical device
    ↓
ML Kit
    ↓
Pose detection
    ↓
Pose overlay
    ↓
Motion analysis
    ↓
Feedback engine
    ↓
Session storage
    ↓
Progress UI
```

If something breaks, the number of possible causes stays small.

## Rule 4 — Build after major integration points

After adding a significant dependency/module:

```bash
flutter pub get
flutter run
```

Do not wait until ten changes have accumulated.

## Rule 5 — Do not blindly copy old code

The previous code is a reference.

Especially for:

- ML Kit
- camera
- Android configuration
- Gradle
- NDK

verify the API/configuration against the versions actually installed in the new project.

## Rule 6 — Preserve working checkpoints

After each stable milestone:

```text
Checkpoint 0 — Empty Flutter app builds
Checkpoint 1 — Camera works
Checkpoint 2 — Pose detection works
Checkpoint 3 — Skeleton overlay works
Checkpoint 4 — Motion analysis works
Checkpoint 5 — Feedback works
Checkpoint 6 — Session storage works
Checkpoint 7 — Progress dashboard works
Checkpoint 8 — Final MVP
```

Git commits should ideally correspond to these milestones.

---

# 13. What Still Needs to Be Built

Based on the latest roadmap and previous log, the most important unfinished technical work is:

## Camera → Pose pipeline

Need to ensure:

```text
CameraController
      ↓
startImageStream
      ↓
CameraImage
      ↓
InputImage conversion
      ↓
PoseDetector
      ↓
List<Pose>
```

The previous log says this pipeline was only partially wired.

---

# 14. Skeleton Overlay

Need a `CustomPainter` capable of:

- Drawing pose keypoints
- Drawing bones/lines
- Mapping ML Kit coordinates to screen coordinates
- Handling camera preview scaling
- Handling front-camera mirroring if applicable
- Staying synchronized with the camera preview

This is a major MVP milestone.

---

# 15. Motion Analysis

Initial strike-analysis metrics:

### Arm extension

Use relevant joints such as:

```text
Shoulder
Elbow
Wrist
```

Calculate the elbow/arm geometry using joint coordinates.

### Hip rotation

Use torso/hip/shoulder relationships to estimate rotation.

### Velocity

Track pose-point positions across frames:

```text
position(t)
position(t-Δt)
      ↓
distance / Δt
      ↓
approximate velocity
```

This should initially be simple and robust rather than scientifically over-engineered.

---

# 16. Strike Quality Index — SQI v1

SQI is the first feedback/scoring layer.

The goal is not a perfect martial-arts biomechanics model.

The MVP version should answer:

> “Was this strike reasonably executed, and what should the user improve?”

Potential scoring dimensions identified so far:

- Arm extension
- Hip rotation
- Guard position
- Other simple pose-derived criteria

Example:

```text
Strike detected
      ↓
Calculate metrics
      ↓
Evaluate thresholds
      ↓
Calculate component scores
      ↓
Combine into SQI
      ↓
Generate feedback
```

Example feedback:

```text
"Rotate hips more"
"Extend your arm"
"Keep guard up"
```

---

# 17. Session Tracking

A training session should eventually record:

- Start time
- End time
- Session duration
- Strike count
- Average SQI
- Feedback/errors

The revised roadmap wants these data points to support the progress dashboard.

---

# 18. Progress Dashboard

MVP-level progress features:

- XP bar
- Streak
- Session history
- Session time
- Average SQI
- Simple charts

The goal is a convincing functional dashboard, not a complete production analytics system.

---

# 19. Future Features — Explicitly Not MVP-Critical

The previous log identifies these as lower priority/post-MVP:

- Full gamification engine
- XP/belts beyond the basic MVP display
- Grappling drills
- Decision rounds
- Leaderboards
- Cloud analytics
- Full backend
- Advanced cloud synchronization

These should not distract from the 20 Nov MVP.

---

# 20. How We Will Work Together

The user will:

- Log what was completed.
- Ask questions whenever blocked.
- Share code/errors/screenshots when debugging.
- Move through the roadmap step by step.

Assistant support should cover all three areas:

### A. Daily build support

Translate each day's roadmap into concrete coding tasks.

Example:

```text
Today:
1. Create X
2. Add dependency Y
3. Implement Z
4. Run test A
5. Expected result B
```

### B. Progress tracking

Maintain awareness of:

- Completed work
- Current task
- Blockers
- Next task
- MVP deadline

Do not unnecessarily restart completed work.

### C. Coding/debugging support

When a problem occurs:

1. Identify the exact error.
2. Determine whether it is:
   - Dart
   - Flutter
   - package/API
   - Android
   - Gradle
   - SDK
   - NDK
   - device/permission
3. Make the smallest appropriate change.
4. Test.
5. Only then move forward.

---

# 21. Debugging Protocol

When an error occurs, do **not** immediately change five files.

Instead provide:

### 1. Exact error

Copy the relevant error from Android Studio/terminal.

### 2. Context

Tell what command was run:

```text
flutter run
flutter pub get
flutter build apk
```

or what happened in Android Studio.

### 3. Relevant file

If the error points to a file, provide that file or the relevant section.

### 4. Environment

If it is an Android/build problem, check:

```bash
flutter doctor -v
flutter --version
dart --version
adb devices
```

### 5. Fix one cause at a time

Avoid speculative edits.

---

# 22. Important Distinction: Dart vs Android Problems

RUDRA is being written in **Dart/Flutter**, but Flutter Android builds depend on native Android infrastructure.

Therefore:

### Dart/Flutter errors

Examples:

- syntax errors
- null-safety errors
- type errors
- widget errors
- state management errors
- undefined methods/classes

These should be debugged in Dart/Flutter code.

### Android build errors

Examples:

- Gradle
- compileSdk
- minSdk
- NDK
- AndroidManifest
- Java/Kotlin
- plugin compatibility

These should be debugged in Android configuration.

This distinction will prevent unnecessary Dart changes when the actual problem is Gradle/SDK.

---

# 23. Definition of Done for the MVP

RUDRA MVP is considered successful when the following can be demonstrated on the Realme GT Neo 3T:

```text
App opens
  ↓
User enters training flow
  ↓
Camera starts
  ↓
Human pose is detected
  ↓
Skeleton appears over the camera
  ↓
User throws a basic strike
  ↓
Motion metrics are calculated
  ↓
Feedback appears
  ↓
Strike/session data is recorded
  ↓
Session ends
  ↓
Summary is shown
  ↓
Progress screen shows stored results
```

And the system can operate sufficiently smoothly for a demonstration.

---

# 24. The Main Principle Going Forward

**We are building the MVP, not the final RUDRA product.**

Whenever there is a choice between:

- complex vs simple
- production-scale vs demonstrable
- new dependency vs existing capability
- perfect algorithm vs robust basic rule
- additional feature vs fixing the core pipeline

the MVP priority should normally win.

The core success criterion is:

> **Camera → Pose → Analysis → Feedback → Session → Progress**

Everything else is secondary until that pipeline works.

---

# 25. Immediate Restart Checklist — 2 Nov

Before serious RUDRA coding begins:

- [ ] Open the new/clean `rudra_mvp` project.
- [ ] Confirm Flutter installation.
- [ ] Confirm Dart installation.
- [ ] Run `flutter doctor -v`.
- [ ] Confirm Android SDK setup.
- [ ] Confirm required Android platform(s).
- [ ] Confirm NDK situation rather than changing it blindly.
- [ ] Confirm ADB.
- [ ] Connect Realme GT Neo 3T.
- [ ] Run `adb devices`.
- [ ] Create/verify the minimal Flutter project.
- [ ] Run the untouched app on the phone.
- [ ] Confirm the minimal app builds successfully.
- [ ] Only then begin camera integration.

---

# 26. Immediate Next Milestone — 3 Nov

After the environment is stable:

**Camera + ML Kit Integration Test**

Success means:

```text
Live camera preview
        +
ML Kit pose detection
        +
visible pose keypoints/skeleton
        =
Phase 0 complete
```

Only after this is stable should Phase 1 UI/module development begin.

---

# 27. Source / Continuity Notes

This document is based on:

1. **Rudra Project Report.pdf**
   - Latest revised roadmap
   - MVP target of 20 Nov
   - Phase 0 and Phase 1 schedule
   - Latest proposed folder structure
   - Latest technology stack

2. **Untitled document (1).pdf**
   - Previous development log through 23 Oct
   - Previous implementation status
   - Previous code/file structure
   - Dependency history
   - Android/Gradle/NDK debugging history
   - Previous remaining tasks

Where the latest roadmap and previous log differ, the **latest roadmap is treated as the current plan**, while the old log is treated as implementation history and a record of things that caused problems.

---

# 28. Continuation Instruction

When continuing RUDRA work in a new conversation, treat this file as the project context.

Do not assume that old code is still the best implementation.

Do not assume that an old dependency version is still correct without checking the current project.

Do not rebuild features that have already been verified as working unless there is a reason.

Prioritize:

**clean environment → incremental integration → frequent checkpoints → MVP pipeline → testing → demo.**

The project is an **MMA teaching app**, and all coding work is primarily being done in **Dart using Flutter**, with Android/ML infrastructure supporting the application.

