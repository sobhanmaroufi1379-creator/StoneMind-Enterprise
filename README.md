# StoneMind Enterprise 3.1.1

**Industrial Stone-Block Cutting Planning & Optimization System**

> Professional offline software for geometric analysis, cutting scenario generation, material balance verification, and G-code export for stone processing machines.

---

## Project Overview

StoneMind Enterprise is a desktop engineering application designed for the stone processing industry. It assists engineers and operators in planning the cutting of large stone blocks into slabs or products while maintaining strict geometric and material conservation constraints.

The system emphasizes **traceable geometry**, **explicit confidence levels**, and **engineering auditability** rather than purely heuristic packing.

**Current Version:** 3.1.1  
**Status:** Engineering-validated (source code proprietary — not published in this repository)

---

## Problem Statement

In industrial stone cutting:

- Blocks are large, expensive, and often irregular.
- Cutting decisions directly affect material yield, waste (kerf + remainder), and machine time.
- Incorrect volume assumptions or overlapping defect accounting can lead to significant financial loss.
- Many existing tools either treat geometry approximately or lack clear confidence labeling.

StoneMind addresses these issues by enforcing a single authoritative geometry path, explicit scenario labeling, and material balance invariants.

---

## Core Capabilities

- **3D Model Import** with explicit unit handling (internal calculations in SI metres).
- **Geometry Kernel** as the single authoritative pathway for solid volume, oriented bounding box (OBB), and plane-split operations.
- **Material Balance Enforcement**  
  `V_ref = V_products + V_kerf + V_remainder + V_other`
- **Scenario Classification**  
  Every result is explicitly labelled:  
  `GEOMETRY_VERIFIED` | `PACKING_ESTIMATE` | `APPROXIMATE`
- **Defect Volume Accounting** with exact AABB union (no double-counting of overlaps).
- **Offline G-code Generation** with validation against block extents and rejection of invalid scenarios.
- **Engineering Reports** (JSON / CSV / structured output).
- **Headless mode** support for automated testing and batch evaluation.

---

## Technical Architecture (High-Level)

```
UI (PySide6)
    ↓
Services Layer (Project, Defects, Intelligence, Digital Twin)
    ↓
Engines
    • Geometry Kernel
    • Cutting Planner & Simulation
    • Optimization / Scenario Evaluation
    • Post-processors (G-code, CSV, JSON, Engineering Report)
    ↓
Core (Config, Units, Logging, Exceptions) — no UI dependency
```

Key design principles:
- Geometry Kernel is the single source of truth for solid metrics and plan verification.
- Scenarios that cannot be fully geometrically verified remain clearly labelled as estimates.
- Public APIs preserved across 3.1.x remediation cycles.

Detailed documentation is available in the [`docs/`](docs/) folder.

---

## Technology Stack

| Layer              | Technologies                                      |
|--------------------|---------------------------------------------------|
| Language           | Python 3.13                                       |
| UI                 | PySide6                                           |
| Geometry           | trimesh, shapely, numpy, scipy                    |
| 3D Viewport        | PyVista + pyvistaqt (recommended)                 |
| Testing            | pytest                                            |
| Reporting          | reportlab (PDF), structured JSON/CSV              |
| Persistence        | Project format v5 (backward compatible)           |

---

## Validation & Engineering Results (Evidence-Based)

### Version 3.1.1 (Critical Remediation)
- **65 tests passed**, 1 skipped
- New independent ground-truth suite for exact defect AABB union: **9 passed**
- Critical fixes:
  - Exact coordinate-compression union for defect volumes (handles identical, nested, partial, and multi-overlap cases)
  - Removal of unconditional geometry eligibility bypass
  - Dimensional thickness validation in plan verification

### Version 3.1.0
- Introduction of Geometry Kernel as authoritative path
- Two-stage scenario evaluation (packing estimate → geometric verification when mesh is watertight)
- Explicit scenario labelling and material balance invariant preservation
- Ground-truth cuboid and cavity cases verified

### Version 3.0.1
- Multiple high-severity persistence, twin, and defect-related issues remediated and verified

Full validation summaries are available in [`docs/04-testing-validation-summary.md`](docs/04-testing-validation-summary.md) and [`docs/07-engineering-audit-summary.md`](docs/07-engineering-audit-summary.md).

---

## Project Limitations (Explicit)

The following are **not** claimed or supported in the current architecture:

- Full arbitrary 3D nesting / multi-direction simultaneous cutting plans
- Automatic discovery of internal cracks from surface mesh alone
- Oriented (non-AABB) free-form defect solids for union volume
- Controller-ready multi-axis CNC post-processors (G-code is offline representation only)
- Continuous multi-objective solver beyond Pareto annotation of packing candidates

Partial-face products and non-watertight meshes correctly remain labelled as `PACKING_ESTIMATE` or lower confidence.

See [`docs/05-limitations.md`](docs/05-limitations.md) for the complete list.

---

## Documentation Structure

| Document | Description |
|----------|-------------|
| [Architecture Overview](docs/01-architecture-overview.md) | High-level system layering |
| [Geometry Kernel Overview](docs/02-geometry-kernel-overview.md) | Role of the authoritative geometry path |
| [Engineering Documentation](docs/03-engineering-documentation.md) | Core engineering principles |
| [Testing & Validation Summary](docs/04-testing-validation-summary.md) | Test results and ground-truth cases |
| [Limitations](docs/05-limitations.md) | Explicit unsupported cases |
| [Demo Documentation](docs/06-demo-documentation.md) | Typical workflow description |
| [Engineering Audit Summary](docs/07-engineering-audit-summary.md) | Remediation history 3.0.1 → 3.1.1 |

---

## Screenshots

Screenshots of the interactive UI and 3D viewport will be added to the [`screenshots/`](screenshots/) folder.

*(This repository currently contains documentation only. Real application screenshots can be supplied by the author upon request.)*

---

## Disclaimer

**Source code is proprietary and is not included in this repository.**

This public GitHub repository is intended solely as a professional portfolio artifact for:
- Technical resume / CV presentation
- Academic applications (graduate studies, research positions)
- High-level technical evaluation by industry or academic reviewers

No executable code, proprietary algorithms, machine databases, or production project data are published here.

See the [NOTICE](NOTICE) file for formal statement.

---

## Author

**MohammadJavad Maroufi**  
GitHub: [sobhanmaroufi1379-creator](https://github.com/sobhanmaroufi1379-creator)

---

*StoneMind Enterprise 3.1.1 — Engineering portfolio presentation*
