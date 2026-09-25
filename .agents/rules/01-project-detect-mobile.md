---
trigger: always_on
---

# Mobile Framework Detection (run first)

Before any mobile app work, determine which mobile skill applies — do this once per session (or when project config files change), not on every message.

## How to detect
- `pubspec.yaml` at project root with a `flutter:` section → Flutter project → use the `flutter-mobile` skill.
- `package.json` with `"react-native"` as a dependency, or a `metro.config.js`/Expo's `app.json`/`app.config.js` → React Native project → use the `react-native-mobile` skill.
- Neither present, but native `android/` + `ios/` project folders exist with no cross-platform framework → flag this to the user rather than guessing which toolchain applies.

## Behavior
- State the detected framework in one line before starting work (e.g. "Detected: Flutter"), then proceed straight to the task — don't ask the user to confirm unless signals genuinely conflict.
- Once detected, apply the matching skill's rules for the rest of the session without re-checking.
- This is separate from `00-project-detect.md` (web/Next.js vs React) — a given repo will only ever match one detection rule; the other simply won't find its signals and can safely stay in place unused.
