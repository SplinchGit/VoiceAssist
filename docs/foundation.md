# Junction Foundation Layer

This document defines the initial foundation layer for Junction with a focus on
cross-platform Android + PC support.

## Scope (Phase 0)
- Planning + architecture only (no production integrations yet).
- One shared domain model for notification items and user actions.
- Platform shells for Android and PC that can render a basic feed using mock data.

## Recommended approach
**Kotlin + Compose Multiplatform** is the most direct path to share UI and business
logic across Android and desktop (Windows/macOS/Linux). If desktop is strictly a
companion controller rather than a full UI, the desktop layer can be minimal and
the shared core still stays in Kotlin.

### Proposed module layout (future)
```
/ shared
  /domain       # notification models, filters, grouping rules
  /usecases     # feed assembly, digest generation
  /data         # local store and mock data source
/androidApp     # Android app shell, notification permission flow
/desktopApp     # Desktop shell (Compose Desktop)
```

## Foundation tasks (next 2 steps)
1. **Define the core domain model**
   - `NotificationItem` with source, timestamp, type, content, priority, action links.
   - `FeedGroup` for dedupe and grouping.
2. **Mock feed pipeline**
   - Local mock data generator shared across Android + PC.
   - Render a unified feed list with grouping headers.

## Milestone 1 (first runnable target)
- Installable Android app.
- Home screen displays a mock notification feed.
- No external APIs, no permissions beyond basic app launch.

## Reliability notes
- "100% uptime" should be interpreted as "maximize availability" on mobile.
- Android background constraints require explicit design:
  - foreground service where needed
  - scheduled work with WorkManager
  - clear user-facing settings for quiet hours

## Product guardrails
- Silent by default.
- Everything visible in-app even if notifications are off.
- Encourage routines and sociability without intrusive alerts.
