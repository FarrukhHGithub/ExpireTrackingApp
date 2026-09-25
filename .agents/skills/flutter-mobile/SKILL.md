---
name: flutter-mobile
description: Use for any Flutter/Dart work — widgets, screens, local SQLite storage (drift/sqflite), barcode scanning (mobile_scanner), and local notifications (flutter_local_notifications). Applies to this project's offline, no-backend Android app. Does not apply to React Native projects — use react-native-mobile for those.
---

# Flutter (Dart) Mobile

## Structure
- Follow this folder layout:
  ```
  lib/
    models/      Product, Batch, etc. — plain Dart data classes
    database/    drift/sqflite setup, table definitions, queries
    screens/     one folder per screen (ExpiryList, AddProduct, Scan, Settings)
    services/    NotificationService, BackupService, etc.
    widgets/     reusable pieces (ProductCard, ExpiryBadge, etc.)
  ```
- One widget class per file. A screen's `build()` should compose smaller widgets, not inline a long tree — split into `widgets/` once `build()` is laying out more than 3–4 top-level pieces.
- Prefer `const` constructors wherever a widget's inputs don't change, to cut unnecessary rebuilds.

## State management
- Default to `StatefulWidget` + `setState` for this project's scope (offline, no complex shared state). Don't introduce Provider/Riverpod/Bloc unless state genuinely needs to be shared across more than 2–3 screens — adding a state-management library is a deliberate decision, not a default.

## Database (drift or sqflite)
- Two tables per the roadmap: `Products` (id, barcode, name) and `Batches` (id, product_id, expiry_date) — expiry lives on the batch, not the product, since one product can have several batches on the shelf with different expiry dates.
- Every write (add/update/delete) must be verified to survive an app restart — don't consider a database feature done without testing this.
- Index `barcode` (scan lookup) and `expiry_date` (sort for the expiry list).
- Write a migration for any schema change, even early in development — don't just delete and recreate the local db.

## Barcode scanning (mobile_scanner)
- Always request camera permission explicitly with a rationale, and provide a manual-entry fallback if permission is denied or the barcode can't be read.
- On scan: if the barcode exists in `Products`, pre-fill the name and only prompt for expiry date; if new, prompt for both name and expiry date.
- Debounce/lock the scanner after a successful read so the same barcode isn't captured multiple times in one pass.

## Notifications (flutter_local_notifications)
- Schedule the daily "N products expire within X days" check locally — no server, no push service.
- Test notification delivery on a real device, not just the emulator — background/battery-optimization behavior varies by Android OEM and can silently suppress local notifications.
- Make the "expiring within X days" threshold a user-adjustable setting (default 7), not a hardcoded constant.

## Expiry list UI
- Sort by nearest expiry date ascending.
- Colour-code: red = expired or 0–3 days, orange = 4–7 days, green = 8+ days — define as named constants/theme colors, not inline hex scattered across widgets.
- Show human-readable relative time ("Expires in 4 days"), not just the raw date.

## Scope discipline
- Build only what the current phase needs (see the project roadmap). Price, quantity, categories, backup/restore, reports, and cloud sync are later-phase features — don't scaffold them early "while I'm in there."
