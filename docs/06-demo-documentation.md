# Demo Documentation

## Typical Workflow

1. **Create or load a project**
2. **Import 3D model** (STL / OBJ) with explicit unit declaration
3. **Define stone properties** (type, density if available, notes)
4. **Select or configure machine profile** (blade/wire parameters, kerf, limits)
5. **Define target products** (dimensions, quantities, types such as slab)
6. **Run optimization** — system generates candidate scenarios
7. **Review results** with explicit labels (`GEOMETRY_VERIFIED`, `PACKING_ESTIMATE`, etc.)
8. **Select a scenario** and export:
   - Offline G-code
   - JSON / CSV data
   - Engineering report

## Sample Data (Internal)

The original development tree contained two sample meshes:

- Unit cube (`sample_block.stl`)
- Irregular open stone block (`sample_stone_block.stl`)

These files are **not** included in this public portfolio repository.

## Headless Operation

The application supports a headless mode suitable for automated testing and batch evaluation. Interactive UI is not required for core geometric and optimization paths.

## Notes for Demonstrations

When presenting the system to technical evaluators:

- Emphasize the explicit confidence labelling of every scenario.
- Show material balance reporting.
- Highlight that non-watertight or partial-face cases are never falsely marked as geometrically verified.
- Clarify that G-code is offline-only.

---

*This document describes the intended user workflow at a high level. No executable demonstration is provided in this repository.*
