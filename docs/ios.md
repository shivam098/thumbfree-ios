# ThumbFree for iPhone: design

The feature reference every ThumbFree app follows is
[PLAN.md, section 3](https://github.com/shivam098/thumbfree-android/blob/main/docs/PLAN.md#3-feature-reference-built-on-android-copy-to-every-platform).
This page is the iPhone side.

iOS doesn't allow a custom keyboard to use the microphone, and has no floating overlays. So the iPhone app is **an app
plus a keyboard extension that work together**, the same way Wispr Flow does it on iPhone.

## Decisions (November 2026)

- **One repo per platform**, each with its own copy of the shared test data (`tests/vectors`, `tests/audio`); a CI
  check fails while the copies differ.
- **The core is Swift**, shared with the Mac app (thumbfree-mac) and held to the same test data. No Kotlin
  Multiplatform.
- Work starts after the Mac app.

## Shape

- The **ThumbFree app** owns the microphone, the models and the speech engines.
- The **ThumbFree keyboard** (extension) shows the mic button, the live preview and the same keys as Android, and
  inserts text with `textDocumentProxy.insertText`.
- Both share an **App Group** container for settings and results. The keyboard needs **Full Access** for that; the
  disclosure says why: no network access.

## Starting a session

- The first time (or after the session has ended), the keyboard's **Start** button opens the app.
- The app starts an `AVAudioSession` with the background-audio mode, so recording can start later from the keyboard.
  It then asks you to swipe back (iOS shows "◀ back to app" at the top left).
- After that, the keyboard's mic starts and stops recording with no app switch: the keyboard signals the app through
  the App Group (plus a Darwin notification), and the app records, transcribes and writes the result back.
- The orange microphone dot is visible while the session is open.
- The session ends after a few idle minutes to save battery (configurable, for example 5 minutes). It ends at once when
  you tap "End session".

## Models and speed

- Models and engines run **in the app, never in the keyboard**: keyboard extensions have a tight memory limit (tens of
  MB).
- English: Kroko and Parakeet through sherpa-onnx (it has iOS builds). Evaluate a Core ML / Neural Engine build.
- Hindi + English: whisper.cpp with Metal / Core ML.

## Field info

- `documentContextBeforeInput` → continue the sentence, and "at start".
- `keyboardType` / `returnKeyType` / `textContentType` → single-line, search and email (`.emailAddress`, plus mail
  apps).
- Secure text fields switch to the system keyboard automatically. Never record there.

## Also

- **Instead of the bubble:** a Shortcuts / Action Button action "Dictate with ThumbFree" (runs in the app, copies the
  result, can type into a Shortcuts text step). Later, a Control Center control.
- **Notifications:** local notifications for "can't hear you" and "tap to copy".
- **Data sharing:** `UIActivityViewController`.
- **First-run guide**, with these iPhone steps: add the keyboard (Settings › General › Keyboard › Keyboards › Add New
  Keyboard › ThumbFree), turn on Allow Full Access (and why), then a first session.

## App Review

- Guideline 4.4.1: keyboards must provide typing input and a way to switch to the next keyboard, and must work without
  Full Access. So include a basic letters layout, and a clear "Allow Full Access to dictate" state.
- Honest microphone and background-audio usage strings.
- Privacy label: data not collected.

## Build and test

- GitHub's macOS runners build it and run the unit tests and the shared vectors on the iOS Simulator.
- The microphone, the session handoff and the keyboard need a **real iPhone** and TestFlight (Apple Developer Program,
  US$99 a year; the same account covers the Mac).
