# CORD1R — Rectangular Coordinate System Definition, Form 1

Defines a rectangular coordinate system using three grid points. One or two systems may be defined on a single entry.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CORD1R | CIDA | G1A | G2A | G3A | CIDB | G1B | G2B | G3B | |

## Fields

| Name | Type | Description |
|---|---|---|
| CIDA, CIDB | Integer > 0 | Coordinate system identification number (must be unique across all CORD entries) |
| GiA, GiB | Integer > 0 | Grid point IDs defining the system: G1 = origin, G2 on z-axis, G3 in x-z plane. G1 ≠ G2 ≠ G3, must be noncolinear |

## Key Remarks

- Point location given by (X, Y, Z)
- Displacement directions at point P: (u_x, u_y, u_z)
- The defining grids must be defined in coordinate systems that do not involve this coordinate system
- CIDB is optional (fields 6–9 may be blank)
