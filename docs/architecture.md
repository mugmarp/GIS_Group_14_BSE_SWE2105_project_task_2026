# Architecture — GIS Group 14 SWE2105 Project

**Status:** ✅ **Decided** — Option 2 (Mobile Tracking), Android/Kotlin/Compose

---

## High-Level Architecture (Option 2 — Mobile Tracking)

```
┌─────────────────────────────────────────────────────────┐
│                    MOBILE APP (Android)                 │
├─────────────────────────────────────────────────────────┤
│  UI Layer (Jetpack Compose + Material3)                 │
│  └── Map View (MapLibre / osmdroid)                     │
├─────────────────────────────────────────────────────────┤
│  Domain Layer                                           │
│  ├── Location Service (FusedLocationProviderClient)     │
│  │   ├── Primary: GPS + WiFi + Cell fusion (adaptive)  │
│  │   └── Educational: Raw CellInfo + OpenCellID API     │
│  ├── Trail Recorder (adaptive intervals)                │
│  ├── Geofence Manager (home/school/work/custom)         │
│  └── Battery Optimizer (priority switching)             │
├─────────────────────────────────────────────────────────┤
│  Data Layer                                             │
│  ├── Local DB (Room) — LocationPoint entities          │
│  ├── Export (GPX / KML / CSV)                           │
│  └── Sync (Firebase Realtime DB — multi-user sharing)  │
└─────────────────────────────────────────────────────────┘
```

### Location Provider Strategy

| Layer | Implementation | Purpose |
|-------|----------------|---------|
| **Primary (Production)** | `FusedLocationProviderClient` with adaptive priority | Best available location automatically fused from GPS + WiFi + Cell by Google Play Services |
| **Adaptive Priority** | `HIGH_ACCURACY` when moving (GPS), `BALANCED_POWER_ACCURACY` when stationary (WiFi/Cell) | Battery-efficient real-time tracking |
| **Educational/Diagnostic (Optional)** | `TelephonyManager.getAllCellInfo()` → OpenCellID / Mozilla Location Service API | Demonstrates "true cell triangulation" for task discussion; shows raw CID/LAC/MCC/MNC vs fused result |

**Why this approach:**
- FusedLocationProvider = production-grade, handles all fusion internally, battery-optimized
- Raw cell + OpenCellID = satisfies task requirement to "discuss cell tower triangulation pros/cons" with real data
- No custom triangulation math needed — OpenCellID/Mozilla do the server-side lookup

---

## Android Package Structure (Kotlin/Compose)

```
app/
├── src/main/
│   ├── AndroidManifest.xml
│   ├── java/com/gisgroup14/locationtracker/
│   │   ├── MainActivity.kt
│   │   ├── LocationTrackerApp.kt
│   │   ├── data/
│   │   │   ├── db/ (Room entities, DAOs)
│   │   │   ├── repository/ (LocationRepository)
│   │   │   └── export/ (GpxExporter, KmlExporter)
│   │   ├── service/
│   │   │   ├── LocationForegroundService.kt
│   │   │   ├── BatteryOptimizer.kt
│   │   │   └── CellDiagnosticService.kt      // Optional: raw cell + OpenCellID
│   │   ├── ui/
│   │   │   ├── map/ (MapScreen, MapViewModel)
│   │   │   ├── tracking/ (TrackingScreen, TrackingViewModel)
│   │   │   ├── geofence/ (GeofenceScreen)
│   │   │   ├── sharing/ (SharingScreen)       // Family safety: share with contacts
│   │   │   ├── settings/ (SettingsScreen)
│   │   │   └── theme/ (Material3 theme)
│   │   └── di/ (Hilt modules)
│   └── res/
│       ├── values/strings.xml
│       └── xml/network_security_config.xml
└── build.gradle.kts
```

---

## Key Technical Decisions (from DECISION_LOG.md)

| Decision | Choice | Rationale |
|----------|--------|-----------|
| **Tracking Approach** | Option 2: Mobile (GPS/WiFi/Cell) | Real-time capable, meter-level accuracy, meets task |
| **Platform** | Android (Kotlin + Jetpack Compose) | Full sensor access, Uganda market dominant, free tooling |
| **Map Library** | *Pending* (MapLibre vs osmdroid) | Compose-compatible, free, offline-capable |
| **Local Database** | *Pending* (Room recommended) | Android standard, type-safe, coroutines/Flow |
| **Backend/Sync** | *Pending* (Firebase Realtime DB recommended) | Free tier, real-time sync for family sharing, auth |
| **Background Strategy** | *Pending* (Adaptive: HIGH_ACCURACY moving, BALANCED stationary) | Battery-efficient, meets Android 10+ requirements |

---

## Components to Document

- [x] Location data model (`LocationPoint` schema)
- [ ] Permission flow diagram
- [ ] Background service lifecycle
- [ ] Battery optimization strategy (adaptive priority)
- [ ] Map rendering pipeline (MapLibre/osmdroid in Compose)
- [ ] Export format specification (GPX/KML)
- [ ] Sync protocol (Firebase Realtime DB)
- [ ] Privacy policy / data retention
- [ ] Cell diagnostic screen (raw cell + OpenCellID comparison)