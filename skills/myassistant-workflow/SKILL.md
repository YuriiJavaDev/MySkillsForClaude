---
name: myassistant-workflow
description: Use when working on the owner's MyAssistant Android app (Java, Room, WorkManager, WebDAV backup) or any of his repositories; language, code style, git and verification rules.
---

# MyAssistant workflow

Project: https://github.com/YuriiJavaDev/MyAssistant. Java, Room, WorkManager, WebDAV backup. Modules: tasks, steps, scans, weather, currency, calculator, flights. The owner tests on a Samsung S23 Ultra and builds in Android Studio.

## Language

- Talk to the owner in Russian (he sometimes mixes in Ukrainian).
- All code, code comments and UI strings are in English. Text the user enters in the app is the only exception.

## Code

- Show full files, not fragments, when presenting code.
- Follow `clean-code-principles` (code, testing and git rules).
- Follow the existing UI style: `@color/background`, `@color/text_color`, `bg_spinner` and `bg_spinner_dropdown`, the `Widget.MyAssistant.ScanButton` style. Colours only through resources so night mode works.
- minSdk 24: no `String.repeat` or `String.join`; use `TextUtils.join`.
- Infer the obvious intent from context instead of asking or guessing wrong (for example, a landscape layout that should put a related value on the same line).

## Git

The universal git rules are in `clean-code-principles`. For this project:
- The default branch is `master`.
- After the owner merges, delete the local copies of the branch.
- Squash merges make old branches look "ahead"; their content is already in `master`.

## Verification

There is no Android SDK in the sandbox. Verify with:
- a syntax-only `javac` pass, filtered for real errors;
- a grep for unused imports;
- XML parse checks and resource and id reference checks;
- JUnit tests through a shim, and AWT-shim renders for drawing code.

Never claim the app was tested on a device.

## Replies

- Short, in Russian, what changed and what to try on the phone.
- Say which files changed and which branch the work is on.

## Postponed in the tasks module (do not touch until asked)

- B2 anchored repeats
- A5 notification-tap diagnosis
- silence in E4 and D6
- test-plan update
- tests F1 to F4 and G1 to G5
