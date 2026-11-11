# Limitations

The following limitations are explicitly documented in the project’s verification reports and are not claimed as supported features.

## Architectural / Functional Limitations

1. **1D cutting architecture**  
   Full 3D verification of arbitrary (width × length) partial-face products is not supported. Such cases correctly remain labelled `PACKING_ESTIMATE` and do not receive `GEOMETRY_VERIFIED` status.

2. **Multi-direction simultaneous cutting**  
   Plans that require simultaneous cutting in multiple directions are not supported.

3. **Automatic internal defect discovery**  
   The system does not automatically discover internal cracks or voids from a surface mesh alone.

4. **Defect solid representation**  
   Defect union volume currently uses axis-aligned bounding boxes (AABB). Oriented or free-form defect solids are not supported for exact union.

5. **G-code scope**  
   Generated G-code is an **offline representation** only. Successful format and extent validation does **not** guarantee physical machine safety or controller readiness. Multi-axis CNC post-processors are not provided.

6. **Optimization scope**  
   The system performs Pareto annotation of packing candidates. It does not implement a continuous multi-objective solver beyond this.

7. **Defects without dimensions**  
   When defect dimensions are unavailable, the system falls back to scalar volume summation (with warning).

8. **Triangulation dependency**  
   Certain mesh repair paths degrade if required triangulation libraries are unavailable (pre-existing behaviour).

## UI Testing Note

Interactive PySide6 UI behaviour was not exercised in the automated 3.1.1 remediation test cycle. Headless and unit tests were the primary verification vehicles.

---

These limitations are stated so that technical evaluators have a clear and honest view of the current system boundaries.
