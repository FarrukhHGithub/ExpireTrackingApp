---
trigger: always_on
---

# Mobile App Rules (Universal)

Applies regardless of mobile framework. Framework-specific conventions live in the `flutter-mobile` or `react-native-mobile` skill and load automatically for that work — do not duplicate them here.

## Structure
- Group by responsibility, one clear folder per concern (exact folder names differ slightly by framework — see the matching skill):
  ```
  models/     data classes
  database/   local storage setup + queries
  screens/    one folder per screen
  services/   notifications, backup, device APIs
  widgets/ (or components/)   reusable UI pieces
  ```
- One screen/widget/component per file. Split a file once it does more than one job or its build/render method is composing more than 3–4 top-level pieces inline.

## Offline-first data integrity
- This app has no cloud backend — the local database is the only source of truth. Every write must survive an app kill/restart; test this explicitly, don't assume it.
- Never silently drop data on a schema change — write a migration, even a trivial one.
- Manual entry must always be available as a fallback wherever a device capability (camera, barcode scan) might fail or be denied.

## Permissions & device capabilities
- Request runtime permissions (camera, notifications, storage) with a clear reason and a graceful fallback UI if denied — never crash or silently no-op.
- Test any permission-gated feature with the permission both granted and denied.

## Responsiveness
- Design and test at more than one screen size/density — don't assume the exact device used for development. No fixed-pixel layouts that only work on one screen.
- Respect system font-scaling/accessibility settings — text shouldn't clip or overlap when the user increases system font size.

## Testing on a real device
- Test on an actual physical device before considering a feature "done," not just a simulator/emulator — background behavior (notifications, camera, permissions) often differs from the emulator, especially on Android.

## Scope discipline
- Build only the current phase. Do not pull in later/"future features" work (cloud sync, reports, multi-user, etc.) unless explicitly asked — see the project roadmap.
