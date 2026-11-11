# Engineering Documentation

## Core Engineering Principles

StoneMind Enterprise is built around the following engineering principles:

### 1. Traceability
Every geometric quantity that influences yield or verification carries an explicit basis and confidence. Results are never presented as more certain than the underlying data allows.

### 2. Material Conservation
The fundamental invariant is enforced:

```
V_ref = V_products + V_kerf + V_remainder + V_other
```

`V_other` represents residual numerical or modelling error and is not interpreted as recoverable material.

### 3. Explicit Scenario Status
Scenarios are never ambiguously presented. Each carries one of:

- `GEOMETRY_VERIFIED`
- `PACKING_ESTIMATE`
- `APPROXIMATE`

Invalid scenarios are rejected before G-code generation.

### 4. Separation of Concerns
- Core layer has zero UI dependency.
- Geometry operations are centralized in the Geometry Kernel.
- Machine validation occurs before post-processing.

### 5. Offline Safety for G-code
G-code generation is strictly offline. Successful format and extent checks do **not** guarantee physical machine safety. Headers record calculation method, geometry-verified flag, and confidence level.

## Units Policy

- All internal calculations use **SI units (metres)**.
- User interface displays millimetres for convenience.
- Unit conversion is explicit at import boundaries.

## Project Format

- Current format version: **5**
- Backward-compatible loading of version 4 projects is supported.
- Public API surface was preserved during 3.1.x remediation cycles.

## Testing Philosophy

- Ground-truth analytical cases (cuboids, cavities, plane splits).
- Independent test suites that do not reuse production oracles (especially for defect union volume).
- Regression coverage retained across remediation versions.

---

*This document provides engineering context for evaluators. Detailed algorithms and source remain proprietary.*
