---
trigger: always_on
---

# Command Permissions — Mobile

## Run automatically — never ask first
- `flutter pub get`
- `flutter analyze`
- `flutter test`
- `flutter format .` / `dart format .`
- (if React Native is the detected framework) `npm run lint`, `npx tsc --noEmit`

## Always ask for approval first
- `flutter pub add` / `flutter pub remove` / any dependency add, remove, or upgrade
- `flutter build apk` / `flutter build appbundle` / `flutter build ios` — produces a shippable artifact, confirm before running
- Any command that modifies `pubspec.yaml`, `pubspec.lock`, or env/config files
- Git commands that change history or remote state: `commit`, `push`, `merge`, `reset`, etc.
- Any destructive or irreversible command: deleting files/folders, wiping local SQLite db or emulator data, uninstalling the app from a connected device, etc.

Do not deviate from this list. If a command isn't listed above, treat it as "ask first."
