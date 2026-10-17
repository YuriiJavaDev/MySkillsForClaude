---
name: clean-code-principles
description: Use when writing, reviewing or refactoring code in any of the owner's projects; the owner's core engineering stance and the rules that follow from it.
---

# Clean code principles

## Core stance (the owner's own words)

"Я за скорость, легкость, оптимальность и современные технологии при соблюдении стандартов Чистого Кода и Единой ответственности с легкочитаемой и понятной архитектурой!"

Speed, lightness, optimality and modern technology, always inside Clean Code, the Single Responsibility Principle and an architecture that is easy to read and understand. When these pull apart, keep the code clean and readable first, then make it fast where it matters.

## Rules

1. **Single responsibility.** One class, one reason to change. One method does one thing and its name says what.
2. **Feature packages.** Group code by feature (`flights/`, `tasks/`), and inside a feature by role (`logic`, `data`, `ui`, `report`). Pure logic has no Android dependencies, so it is unit-testable.
3. **No duplication.** Extract the second copy, not the third. Shared behaviour gets one home.
4. **Readable names over comments.** Comment only what the code cannot say: the why, a non-obvious constraint, a unit or a formula source. No comments that restate the line below.
5. **Small and flat.** Short methods, early returns, little nesting, no flag parameters that switch behaviour.
6. **Modern but supported.** Use current language and platform features when they are available at the project's minimum SDK. Never use an API above it (for example no `String.repeat` or `String.join` on minSdk 24).
7. **Light and fast.** No work on the main thread that can block it. No allocations in hot paths such as draw loops and sensor callbacks. Reuse paint, path and buffer objects.
8. **Right tool for the job.** A one-off packer or generator is a script outside the build (`tools/`), not app code. App code stays in the app language.
9. **Tests for logic.** Pure logic gets JUnit tests. A test must check an independent fact, not re-run the implementation's own formula; a circular test proves nothing.
10. **Say what was verified.** Report only what was actually run. Never claim device testing that did not happen; say what to try on the device instead.
