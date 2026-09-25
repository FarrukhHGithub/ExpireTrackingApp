---
trigger: always_on
---

# Stack — Shop Expiry Tracker (Mobile)

- Framework: Flutter (Dart)
- Platform: Android first; iOS possible later from the same codebase
- Database: SQLite on-device (drift or sqflite) — fully offline, no backend/cloud
- Barcode scanning: `mobile_scanner` package
- Reminders: `flutter_local_notifications` package
- No network calls anywhere in this app — do not add HTTP clients, cloud SDKs, or sync logic unless explicitly asked. Cloud/multi-user sync is Phase 5, deliberately deferred as "Hard."

Assume this stack is known. Do not re-explain it in responses.
