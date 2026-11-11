# Architecture Overview

## High-Level Layering

StoneMind Enterprise follows a clean layered architecture:

```
UI Layer (PySide6)
    ↓
Services Layer
    • Project Service
    • Defects Service
    • Intelligence Service
    • Digital Twin Service
    ↓
Engine Layer
    • Geometry Kernel
    • Cutting Planner & Simulation
    • Optimization / Scenario Evaluation
    • Post-processors (G-code, CSV, JSON, Engineering Report)
    ↓
Core Layer
    • Configuration
    • Units (SI internal, mm display)
    • Logging
    • Exceptions
```

## Key Design Decisions

- **Core has no PySide6 dependency** — enabling headless execution and automated testing.
- **Geometry Kernel** is the single authoritative pathway for solid metrics, oriented bounding box (OBB), plane+kerf splits, and plan verification.
- Every geometric result carries an explicit **basis** and **confidence** level.
- Scenario evaluation uses a two-stage approach:
  1. Packing estimate
  2. Geometric plane-split verification (when mesh is watertight)
- Results are never silently upgraded; they are explicitly labelled:
  - `GEOMETRY_VERIFIED`
  - `PACKING_ESTIMATE`
  - `APPROXIMATE`

## Data Flow (Simplified)

1. User creates or loads a project.
2. 3D model is imported with explicit units.
3. Stone properties, machine profile, and target products are defined.
4. Optimization engine generates candidate scenarios.
5. Geometry Kernel verifies eligible plans.
6. Valid scenarios can be selected for offline G-code / report export.

## Persistence

- Project format version 5 (backward-compatible load of version 4).
- Machine profiles stored separately.
- History of project states supported.

---

*This document describes architecture at a level suitable for technical evaluation. Implementation details and source code remain proprietary.*
