# Decision Log — GIS Group 14 SWE2105 Project

This file records key architectural and strategic decisions made by the group. Update as decisions are made.

---

## Decision Template

When recording a decision, use this format:

```markdown
### [DATE] Decision: [Short Title]

**Status:** [Proposed / Accepted / Rejected / Superseded]

**Context:** [What problem are we solving? What constraints exist?]

**Options Considered:**
1. [Option A] — [Pros/Cons]
2. [Option B] — [Pros/Cons]
3. [Option C] — [Pros/Cons]

**Decision:** [What we chose]

**Rationale:** [Why this choice over others]

**Consequences:** [What this means for implementation, timeline, team]

**Action Items:**
- [ ] [Task for specific person]
```

---

## Decisions

### 2026-11-01 Decision: Project Option Selection

**Status:** Proposed (awaiting group discussion)

**Context:** The project task requires choosing between two tracking approaches:
- Option 1: Laptop tracking via IP address
- Option 2: Person tracking via mobile phone (GPS/cell/WiFi)

**Options Considered:**
1. **Option 1: IP Address Tracking**
   - Pros: Simpler implementation, good for learning networking/APIs
   - Cons: Cannot do real-time tracking; accuracy fundamentally limited to city-level; IP changes infrequently; VPN defeats it
2. **Option 2: Mobile Phone Tracking (GPS/Cell/WiFi)**
   - Pros: True real-time tracking (1 Hz GPS); meter-level accuracy outdoors; indoor via WiFi; demonstrates mobile dev, sensor fusion, battery optimization, geospatial viz
   - Cons: Higher complexity; requires physical device testing; background location permissions tricky

**Decision:** [TO BE DECIDED BY GROUP]

**Rationale:** [TO BE FILLED AFTER DISCUSSION]

**Consequences:**
- If Option 1: Simpler 2-3 week project, but may not meet "real-time tracking" requirement
- If Option 2: 4-6 week project with substantial portfolio value, meets all requirements

**Action Items:**
- [ ] All members read `FEASIBILITY_ANALYSIS.md`
- [ ] Group meeting to discuss and vote
- [ ] Record final decision here
- [ ] Initialize project scaffold based on choice

---

### [DATE] Decision: Target Platform

**Status:** Pending (depends on Option 2 selection)

**Context:** If Option 2 chosen, need to pick development platform.

**Options Considered:**
1. **Android (Kotlin + Jetpack Compose)** — Full sensor access (`TelephonyManager.getAllCellInfo()`), free tools, free maps (OSM/MapLibre), direct device testing
2. **iOS (Swift + SwiftUI)** — Polish, but no raw cell tower API, $99/yr dev account for device testing, Mac required
3. **Flutter (Dart)** — Cross-platform, but limited background location on iOS, platform channels needed for raw cell info
4. **React Native (TypeScript)** — Cross-platform, same iOS limitations, Expo managed workflow simplifies but ejects for background
5. **Web (PWA, Leaflet/MapLibre GL JS)** — Cross-platform, zero install, Browser Geolocation API (fused), but no background tracking, no raw cell access

**Decision:** [TO BE DECIDED]

**Rationale:** [TO BE FILLED]

**Consequences:** Affects library choices, testing strategy, team skill requirements

**Action Items:**
- [ ] Assess team skills (Kotlin/Swift/Dart/JS)
- [ ] Check device availability (Android phones, iPhones, Macs)
- [ ] Decide and record

---

### [DATE] Decision: Map Library

**Status:** Pending

**Context:** Need free, no-API-key map solution for student project.

**Options Considered:**
1. **osmdroid (OpenStreetMap for Android)** — Mature, free, no key, raster tiles, Kotlin/Java
2. **MapLibre Native / Mapbox Maps SDK (v11+)** — Vector tiles, free (MapLibre), modern, Compose support
3. **Google Maps SDK** — Requires API key (free tier generous but needs billing account), best docs
4. **Leaflet (Web) / React Leaflet** — Web only, simple, raster tiles

**Decision:** [TO BE DECIDED]

**Rationale:** [TO BE FILLED]

**Consequences:** Affects UI framework choice (Compose vs XML vs Web)

**Action Items:**
- [ ] Spike: test osmdroid vs MapLibre in Compose
- [ ] Decide based on developer experience

---

### [DATE] Decision: Local Database

**Status:** Pending

**Context:** Need to store location points locally for offline-first trail recording.

**Options Considered:**
1. **Room (SQLite)** — Android standard, type-safe, Kotlin coroutines, LiveData/Flow
2. **Realm** — Object-oriented, reactive, but larger APK
3. **DataStore (Preferences/Proto)** — Not for relational location data
4. **Plain SQLite / File (JSON/GPX)** — Simple export, no query capability

**Decision:** [TO BE DECIDED]

**Rationale:** [TO BE FILLED]

**Consequences:** Room integrates best with Android architecture; GPX export easy from any

**Action Items:**
- [ ] Default to Room unless team has strong preference

---

### [DATE] Decision: Backend / Sync (Optional)

**Status:** Pending

**Context:** Multi-user sharing or cloud backup of traces.

**Options Considered:**
1. **Firebase Realtime Database** — Free tier (1 GB, 100 concurrent), easy auth, real-time sync
2. **Supabase (PostgreSQL)** — Free tier (500 MB), SQL, real-time, auth
3. **Custom (Node/Go/Python + PostgreSQL)** — Full control, but hosting cost/effort
4. **None (local-only)** — Simplest, privacy-first, export GPX/KML for sharing

**Decision:** [TO BE DECIDED]

**Rationale:** [TO BE FILLED]

**Consequences:** Adds scope; recommend defer to Week 5 if time permits

**Action Items:**
- [ ] Start local-only; add backend only if ahead of schedule

---

### [DATE] Decision: Background Tracking Strategy

**Status:** Pending

**Context:** Android 10+ requires `ACCESS_BACKGROUND_LOCATION` (user-granted, Play Store review) and Foreground Service with `FOREGROUND_SERVICE_LOCATION`.

**Options Considered:**
1. **Foreground Service + High Accuracy** — Continuous GPS, high battery, shows persistent notification
2. **Foreground Service + Balanced Power** — WiFi/Cell only, lower battery, ~block accuracy
3. **Adaptive: High Accuracy when moving, Balanced when stationary** — Best balance, uses activity recognition
4. **WorkManager periodic + FusedLocation** — Not real-time, ~15 min minimum interval

**Decision:** [TO BE DECIDED]

**Rationale:** [TO BE FILLED]

**Consequences:** Directly impacts battery life and Play Store compliance

**Action Items:**
- [ ] Implement adaptive (Option 3) as default
- [ ] Expose accuracy/battery tradeoff in settings

---

## Decision Index

| Date | Decision | Status |
|------|----------|--------|
| 2026-11-01 | Project Option Selection | Proposed |
| — | Target Platform | Pending |
| — | Map Library | Pending |
| — | Local Database | Pending |
| — | Backend / Sync | Pending |
| — | Background Tracking Strategy | Pending |

---

## How to Use This Log

1. **Before each decision:** Read relevant section in `FEASIBILITY_ANALYSIS.md`
2. **During discussion:** Fill in "Options Considered" with honest pros/cons
3. **After agreement:** Complete "Decision", "Rationale", "Consequences"
4. **Create action items** with checkboxes for tracking
5. **Reference decisions** in commit messages: `feat: add Room DB per DECISION_LOG.md#local-database`