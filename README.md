# GIS Group 14 — SWE2105 Project Task 2026

**Group:** 14  
**Course:** BSE SWE2105 (Software Engineering)  
**Institution:** MUST (Mbarara University of Science and Technology)  
**Academic Year:** 2026

---

## Project Task

> **GROUP PROJECT WORK (PICK ONE)**
>
> Design a web or mobile app that can track the location of people or objects in real-time. A group can select any of following capabilities for your implementation:
>
> - **Option 1:** Track the location of a laptop computer based on its IP address. Research how IP locations are determined and discuss the potential accuracy/limitations.
> - **Option 2:** Track the movement of a person carrying a mobile phone through GPS or cell tower triangulation. Discuss the pros and cons of each approach.

---

## Repository Structure

```
GIS_Group_14_BSE_SWE2105_project_task_2026/
├── README.md                    # This file
├── task                         # Original task description (from instructor)
├── FEASIBILITY_ANALYSIS.md      # Detailed technical feasibility analysis
├── DECISION_LOG.md              # Group decisions and rationale
└── docs/                        # Additional documentation (to be added)
    ├── architecture.md
    ├── api-reference.md
    └── testing-plan.md
```

---

## Feasibility Analysis Summary

See **[FEASIBILITY_ANALYSIS.md](FEASIBILITY_ANALYSIS.md)** for the complete analysis with sources, data tables, and implementation details.

### Quick Comparison

| Aspect | Option 1: IP Address Tracking | Option 2: Mobile Phone Tracking |
|--------|-------------------------------|----------------------------------|
| **Real-time capability** | ❌ Impossible (IP changes hours/days) | ✅ Native (1 Hz GPS updates) |
| **Typical accuracy** | City-level (km to 100s of km) | 2–5 m (GPS), 10–20 m (WiFi), 50–500 m (cell) |
| **Indoor tracking** | ❌ No | ✅ WiFi triangulation |
| **Movement history** | ❌ No | ✅ Continuous trail |
| **Technical depth** | Low (single API call) | High (sensors, fusion, background, maps) |
| **Cost** | Free APIs available | Free (OSM/MapLibre, Firebase free tier) |
| **Portfolio value** | Low | **High** |

### Decision: **Option 2 (Mobile Phone Tracking)** — **Accepted**

**Rationale:**
1. Actually enables **real-time tracking** — the core requirement
2. Demonstrates appropriate **engineering complexity** for SWE2105
3. **Zero cost** — all tools and APIs free for student use
4. **Portfolio-worthy** — working mobile app with map visualization
5. **Testable on real devices** — collect actual GPS traces, measure accuracy

---

## Group Decision

**Status:** ✅ **Accepted** (Option 2: Mobile Phone Tracking)  
**Platform:** ✅ **Accepted** (Android / Kotlin / Jetpack Compose)  
**Tech Stack:** [To be decided per remaining decisions in `DECISION_LOG.md`]

*Update `DECISION_LOG.md` once the group agrees on direction.*

---

## Next Steps

1. **Read** `FEASIBILITY_ANALYSIS.md` — understand both options deeply
2. **Discuss** target problem (Family Safety Tracker vs others) — see `internal/target-problems.md`
3. **Resolve remaining decisions** in `DECISION_LOG.md` (Map Library, Database, Backend, Background Strategy)
4. **Initialize** project scaffold (Week 1 milestone)
5. **Build** incrementally per the 6-week plan in the analysis

---

## Suggested Plan (if Option 2 chosen)

| Week | Focus | Deliverable |
|------|-------|-------------|
| 1 | Project setup, permissions, basic map | Running app showing current location on map |
| 2 | Foreground location service, local storage | Continuous trail saved to Room/SQLite |
| 3 | Map visualization (polyline, markers) | Visual trail with current position |
| 4 | Battery optimization, geofencing | Adaptive tracking, stationary detection |
| 5 | Backend sync (optional) | Multi-user sharing via Firebase/Supabase |
| 6 | Polish, export (GPX/KML), demo | Complete app + demo video + documentation |

---

## References

All sources cited in `FEASIBILITY_ANALYSIS.md` including:
- BigDataCloud, MaxMind, ipapi.is (IP geolocation accuracy studies)
- Blues Wireless, AvantGuard (GPS vs cell vs WiFi comparison)
- Android Developers, Google Developers (FusedLocationProvider API)
- Free API documentation (ip-api.com, freeipapi.com, ipgeolocation.io, MaxMind GeoLite)

---

## Contact

**Group 14 Members:**
- [Add member names and GitHub handles]

**Repository:** https://github.com/mugmarp/GIS_Group_14_BSE_SWE2105_project_task_2026
