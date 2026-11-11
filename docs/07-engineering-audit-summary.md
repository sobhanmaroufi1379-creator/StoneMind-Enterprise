# Engineering Audit Summary

## Version History of Engineering Focus

### 3.0.1 — Remediation Cycle
Focused on fixing previously identified high-severity issues related to:
- Defect persistence and JSON round-trip integrity
- Digital Twin capture / restore / backup behaviour
- Defect-aware validation paths
- Measurement and reporting consistency

Multiple issues were fixed and covered by dedicated regression tests.

### 3.1.0 — Architecture Upgrade
Major engineering improvements:

- Introduction of the **Geometry Kernel** as the single authoritative path for solid metrics, OBB, plane splits and plan verification.
- Two-stage scenario evaluation (packing estimate → geometric verification).
- Explicit scenario labelling (`GEOMETRY_VERIFIED` / `PACKING_ESTIMATE` / `APPROXIMATE`).
- Overlap-aware defect volume accounting (AABB pairwise inclusion-exclusion at that stage).
- Material balance invariant preserved and tested.
- G-code generation rejects invalid scenarios and records method + confidence in headers.
- Project format advanced to version 5 (backward compatible).

Ground-truth analytical cases (cuboids, cavities, plane splits, open meshes) were added and verified.

### 3.1.1 — Critical Geometric Remediation
Targeted fixes for remaining geometric correctness issues:

- **Defect union volume**: replaced pairwise method with exact coordinate-compression AABB union. Correct for identical, nested, disjoint, partial and multi-overlap configurations.
- **Geometry eligibility**: removed unconditional bypass that could incorrectly accept dimensions.
- **Plan verification**: added optional expected-thickness validation so that volume balance alone cannot grant `GEOMETRY_VERIFIED` status.

An independent ground-truth test suite for the new union algorithm was introduced and passed (9 tests).

Overall test suite after 3.1.1: **65 passed, 1 skipped**.

## Design Intent Across Versions

The evolution from 3.0.1 → 3.1.1 shows a consistent engineering priority:

> Prefer explicit, auditable geometric status over optimistic or silent assumptions.

This makes the system more suitable for industrial and academic scrutiny where incorrect material accounting has real cost.

---

*This summary is based solely on the project’s internal verification reports and changelog. Source code remains proprietary.*
