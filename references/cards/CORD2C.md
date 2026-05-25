# CORD2C — Cylindrical Coordinate System Definition, Form 2

Defines a cylindrical coordinate system using the coordinates of three points (not grid IDs).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CORD2C | CID | RID | A1 | A2 | A3 | B1 | B2 | B3 | |
| | C1 | C2 | C3 | | | | | | |

Continuation entry is required.

## Fields

| Name | Type | Description |
|---|---|---|
| CID | Integer > 0 | Coordinate system ID (unique across all CORD entries) |
| RID | Integer ≥ 0; Default = 0 | Reference coordinate system (0 = basic system) |
| A1, A2, A3 | Real | Coordinates of origin point A in RID system |
| B1, B2, B3 | Real | Coordinates of point B in RID system — defines the z-axis direction |
| C1, C2, C3 | Real | Coordinates of point C in RID system — lies in plane of azimuthal origin |

## Key Remarks

- Points A, B, C must be unique and noncolinear
- Point location given by (R, θ, Z) where θ is in degrees
- Displacement directions at point P: (u_r, u_θ, u_z) — depend on location of P
- Avoid placing grids whose CD references this system directly on the z-axis (r ≈ 0)
- Changes to CORD2C on restart trigger a complete re-analysis
