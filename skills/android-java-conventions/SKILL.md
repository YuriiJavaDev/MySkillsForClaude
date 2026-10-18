---
name: android-java-conventions
description: Use when writing or reviewing an Android app in Java for the owner, in any project: structure, threading, Room, resources, sensors, drawing, networking, PDF and the traps already met.
---

# Android Java conventions

Applies on top of `clean-code-principles`. These are the rules and traps learned in real projects, not general advice.

## Structure

- Feature packages, and inside a feature by role: `logic` (pure Java, no Android imports), `data` (repositories), `database` (Room), `model`, `ui`, plus roles such as `sensor` or `report` when the feature needs them.
- Pure logic is a class of its own with a unit test. If a rule can be tested without a device, it does not belong in a Fragment or a Service.
- Fragments and views only show state and pass events on. Repositories own the threading: they run work on a single-thread executor and post the result to the main thread.
- One responsibility per class: do not let a repository also draw, a view also fetch, or a manager also format text.

## Threading

- Database, network, file and PDF work never run on the main thread.
- Deliver results with a `Handler` on the main looper or `LiveData`; never touch views from a worker.
- Long recurring work goes to WorkManager. Work that must keep running with the screen off (a recording, a sensor) is a foreground service with a notification.

## Room

- Any schema change bumps the version and adds a `Migration`; never rely on destructive fallback for user data. A migration may also be used to mark old rows for rebuilding.
- A cursor window holds about 2 MB. Never `SELECT` a row with a BLOB that can be bigger: read it in pieces with `substr(blob, from, length)` (the first byte is number 1) or keep the data in a file. Keep big blobs out of list queries and out of `SELECT *`.
- If the database file itself is copied as a backup, use `JournalMode.TRUNCATE` so the whole database is in one file. Close the singleton before replacing the file.
- Room maps nullable results to boxed types (`Long`, `Boolean`): use them to tell "no row" from zero or false.

## Resources and UI

- Colours only from colour resources, so night mode works; no hard-coded colour values in layouts or styles. Sizes in dp and sp.
- Use the existing styles and spinner, button and background resources instead of new ones.
- A landscape layout lives in `layout-land` with the same view ids as the portrait one, so the code does not branch on orientation. Put related values on one line when the width allows.
- All code, comments and UI strings are in English unless the owner says otherwise.

## Platform level

- Check `minSdk` before using an API. On minSdk 24 there is no `String.repeat` and no `String.join` (use `TextUtils.join`), and several `java.util.function` and `java.time` features need care. Do not claim an API is fine without checking its level.

## Sensors and the screen

- Read the display rotation on every sensor event; do not cache it. Remap the sensor axes by the rotation, and check the mapping with a physical example (phone upright, rotated 90 degrees, camera pointing north gives heading 0) instead of a test that repeats the formula.
- Smooth noisy sensors with a filter whose constant can be tested; keep the filter in `logic`.

## Drawing

- No allocations in `onDraw` or in sensor callbacks: create `Paint`, `Path`, `RectF` and shaders once and reuse them; rebuild a shader only when the size changes.
- Draw to a `Canvas` through small painter classes, so the same code can draw on screen and into a PDF.

## Network

- `HttpURLConnection` or an equivalent with connect and read timeouts, a `User-Agent`, `disconnect()` in `finally`, and a clear error for each failure. Stop a batch at the first failure when the rest cannot succeed.
- Credentials and tokens only in encrypted storage, never in code, logs or backups.
- Free map and data services have terms. Check them, keep the required attribution on the output, and use no key that must be paid for.

## PDF and bitmaps

- `PdfDocument` writes every bitmap as raw pixels: a photo tile can be about seven times its JPEG size. Recompress the photos as JPEG inside the finished file and fall back to the original when anything is unexpected.
- Do not recycle a bitmap that a PDF canvas still has to read; recycle only after the document is written.

## Checks without the SDK

When no Android SDK is present: syntax-only `javac` filtered for real errors (ignore the missing Android packages), a scan for unused imports, XML parsing, resource and id reference checks, JUnit through a shim, and AWT renders for drawing code. State plainly that nothing was run on a device.
