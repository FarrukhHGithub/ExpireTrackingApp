---
name: react-native-mobile
description: Use for any React Native work (bare or Expo) — components, local storage, barcode scanning, and local notifications. Not the framework currently used in this project (it's Flutter) — this exists so the same conventions apply automatically if this project, or a future one, switches to React Native. Does not apply to Flutter projects — use flutter-mobile for those.
---

# React Native Mobile

## Structure
- Mirror the same responsibility split as the Flutter skill, translated to RN/TS:
  ```
  src/
    models/      TS types/interfaces for domain data
    database/    SQLite setup (expo-sqlite, op-sqlite, or WatermelonDB) + queries
    screens/     one folder per screen
    services/    notification service, backup service, etc.
    components/  reusable UI pieces
  ```
- One component per file. Split a screen once its render logic is composing more than 3–4 top-level pieces inline.

## State management
- Default to local component state (`useState`/`useReducer`) for this scope. Only add Zustand/Redux/Jotai if state genuinely needs to be shared across more than 2–3 screens.

## Local database
- Use `expo-sqlite` (Expo projects) or `op-sqlite`/`react-native-sqlite-storage` (bare RN) — never `AsyncStorage` for structured/relational data.
- Same schema discipline as Flutter: verify every write survives an app restart; write a migration for schema changes; index any field used for lookup (e.g. barcode) or sort (e.g. expiry date).

## Barcode scanning
- Use `expo-camera`'s barcode scanning (Expo) or `react-native-vision-camera` + a barcode plugin (bare RN).
- Request camera permission explicitly with a rationale; always provide a manual-entry fallback.

## Notifications
- Use `notifee` or `expo-notifications` for local (non-push) scheduled notifications — no server/FCM/APNs needed for a fully offline app.
- Test on a real device; background notification behavior differs from the simulator/emulator, especially on Android.

## General
- Keep the same offline-first, local-db-is-the-source-of-truth philosophy as the rest of this project (see the universal mobile rule) — don't add network calls unless explicitly asked.
