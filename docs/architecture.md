# Architecture — GIS Group 14 SWE2105 Project

> **Status:** Pending group decision on Option 1 vs Option 2 and platform.

This document will be populated once the group selects:
- Tracking approach (IP-based vs mobile GPS/cell/WiFi)
- Target platform (Android/iOS/Web/Cross-platform)
- Tech stack (Kotlin/Compose, Flutter, React Native, etc.)

---

## If Option 2 (Mobile Tracking) is Selected

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    MOBILE APP                           │
├─────────────────────────────────────────────────────────┤
│  UI Layer (Compose / SwiftUI / Flutter)                 │
│  └── Map View (OSM/MapLibre/Google Maps)                │
├─────────────────────────────────────────────────────────┤
│  Domain Layer                                           │
│  ├── Location Service (FusedLocationProvider /          │
│  │   CoreLocation / navigator.geolocation)              │
│  ├── Trail Recorder (adaptive intervals)                │
│  └── Battery Optimizer (priority switching)             │
├─────────────────────────────────────────────────────────┤
│  Data Layer                                             │
│  ├── Local DB (Room / SQLite / Hive / IndexedDB)        │
│  ├── Export (GPX / KML / CSV)                           │
│  └── Sync (Firebase / Supabase — optional)             │
└─────────────────────────────────────────────────────────┘
```

### Android Package Structure (Kotlin/Compose)

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
│   │   │   └── BatteryOptimizer.kt
│   │   ├── ui/
│   │   │   ├── map/ (MapScreen, MapViewModel)
│   │   │   ├── settings/ (SettingsScreen)
│   │   │   └── theme/ (Material3 theme)
│   │   └── di/ (Hilt modules)
│   └── res/
│       ├── values/strings.xml
│       └── xml/network_security_config.xml
└── build.gradle.kts
```

---

## If Option 1 (IP Tracking) is Selected

### High-Level Architecture

```
┌──────────────────────────────────────────────┐
│              WEB APP (Frontend)              │
│  └── Map View (Leaflet/MapLibre GL JS)      │
├──────────────────────────────────────────────┤
│           BACKEND (Optional)                 │
│  └── IP Lookup API (ip-api.com / freeipapi)  │
├──────────────────────────────────────────────┤
│           DATA LAYER                         │
│  └── Cache (IP → location, TTL-based)       │
└──────────────────────────────────────────────┘
```

---

## Components to Document

- [ ] Location data model (`LocationPoint` schema)
- [ ] Permission flow diagram
- [ ] Background service lifecycle
- [ ] Battery optimization strategy
- [ ] Map rendering pipeline
- [ ] Export format specification (GPX/KML)
- [ ] Sync protocol (if backend included)
- [ ] Privacy policy / data retention