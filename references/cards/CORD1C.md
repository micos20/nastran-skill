# CORD1C — Cylindrical Coordinate System Definition, Form 1

Defines a cylindrical coordinate system using three grid points. One or two systems may be defined on a single entry.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CORD1C | CIDA | G1A | G2A | G3A | CIDB | G1B | G2B | G3B | |

## Fields

| Name | Type | Description |
|---|---|---|
| CIDA, CIDB | Integer > 0 | Coordinate system identification number (must be unique across all CORD entries) |
| GiA, GiB | Integer > 0 | Grid point IDs defining the system: G1 = origin, G2 on z-axis, G3 in azimuthal origin plane. G1 ≠ G2 ≠ G3, must be noncolinear |

## Key Remarks

- Point location given by (R, θ, Z) where θ is in degrees
- Displacement directions at point P: (u_r, u_θ, u_z) — depend on location of P
- The defining grids must be defined in coordinate systems that do not involve this coordinate system
- Avoid placing grids whose CD references this system directly on the z-axis (r ≈ 0)
- CIDB is optional (fields 6–9 may be blank)
