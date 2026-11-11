# Geometry Kernel Overview

## Role

The Geometry Kernel is the **single authoritative pathway** inside StoneMind Enterprise for all solid geometric operations that affect material accounting and plan verification.

It is responsible for:

- Solid volume computation
- Oriented Bounding Box (OBB) calculation (used as envelope metric only)
- Plane + kerf splits
- Plan geometry verification
- Returning every result with an explicit **basis** and **confidence** level

## Design Principles

1. **No silent upgrades**  
   An open or non-watertight mesh never receives a certified material volume.  
   `V_ref` remains 0 with confidence `unknown` until a valid basis is established.

2. **Priority for reference volume (`V_ref`)**  
   measured → closed valid mesh → repaired mesh → mass/density → unknown

3. **Envelope is never treated as certified volume**  
   Axis-aligned bounding box (AABB) volume is reported separately as `V_envelope` and is never silently used as `V_ref`.

4. **Material balance invariant**  
   ```
   V_ref = V_products + V_kerf + V_remainder + V_other
   ```
   This equality is enforced. Large residuals prevent geometric verification.

5. **Dimensional validation**  
   From version 3.1.1, plan verification can optionally validate derived slab thicknesses against expected product thicknesses. Volume balance alone is no longer sufficient for `GEOMETRY_VERIFIED` status.

## Confidence Labels

| Label                 | Meaning                                              |
|-----------------------|------------------------------------------------------|
| `GEOMETRY_VERIFIED`   | Watertight mesh + full-face products + conserved plane-split volumes + thickness match |
| `PACKING_ESTIMATE`    | Usable packing result but geometric verification incomplete or partial-face products |
| `APPROXIMATE`         | Significant geometric uncertainty remains            |

## Defect Handling

Defect volumes use exact AABB union via coordinate compression (introduced in 3.1.1).  
This correctly handles identical, nested, disjoint, partial, and multi-overlap cases without double-counting.

---

*Implementation algorithms remain proprietary. This document describes only the engineering role and guarantees of the Geometry Kernel.*
