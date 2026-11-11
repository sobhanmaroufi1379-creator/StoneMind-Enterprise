# Testing & Validation Summary

All results below are taken from the project’s internal verification reports for versions 3.0.1, 3.1.0 and 3.1.1.

## Version 3.1.1 (Critical Remediation)

**Test command results:**
```
65 passed, 1 skipped in ~2.16s
```

**New independent defect union suite:**
```
9 passed (tests/test_defect_union_exact.py)
```

### Critical fixes verified
| Area | Change |
|------|--------|
| Defect union volume | Replaced pairwise inclusion-exclusion with exact AABB union via coordinate compression |
| Geometry eligibility | Removed unconditional bypass; full-face checks now mirror planner logic |
| Plan verification | Optional expected-thickness validation; volume balance alone no longer grants `GEOMETRY_VERIFIED` |

## Version 3.1.0

**Test results:** 57 passed (including 17 new ground-truth tests)

### Selected ground-truth cases
| Case | Expected | Result |
|------|----------|--------|
| Cuboid 2.0 × 1.5 × 1.0 m | V = 3.000000 m³ | PASS |
| Cuboid + cavity 0.6 × 0.5 × 0.7 | V_ref = 2.79 m³, occupancy 93% | PASS |
| OBB on axis-aligned box | extents correct, volume 3.0 | PASS |
| Single plane split (x=0.8, kerf=0.02) | balance residual ≈ 0 | PASS |
| Open mesh | V_ref = 0, confidence = unknown | PASS |
| Overlapping defects | union < sum | PASS |
| G-code rejects INVALID scenarios | ValidationException | PASS |
| Material balance invariant | residual = 0 | PASS |

## Version 3.0.1 (Remediation)

Multiple high-severity issues related to defect persistence, digital twin restore, and validation were fixed and covered by dedicated tests.

## Notes

- Headless mode (`python main.py --headless`) was exercised during verification.
- Interactive PySide6 UI was not part of the automated remediation test cycle for 3.1.1.
- All numbers above are taken directly from the project’s own verification reports.

---

*This summary reports only verified outcomes. Source code and full test suites remain proprietary.*
