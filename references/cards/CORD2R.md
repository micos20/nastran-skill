# CORD2R — Rectangular Coordinate System Definition, Form 2

Defines a rectangular coordinate system using the coordinates of three points (not grid IDs).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CORD2R | CID | RID | A1 | A2 | A3 | B1 | B2 | B3 | |
| | C1 | C2 | C3 | | | | | | |

Continuation entry is required.

## Fields

| Name | Type | Description |
|---|---|---|
| CID | Integer > 0 | Coordinate system ID (unique across all CORD entries) |
| RID | Integer ≥ 0; Default = 0 | Reference coordinate system (0 = basic system) |
| A1, A2, A3 | Real | Coordinates of origin point A in RID system |
| B1, B2, B3 | Real | Coordinates of point B in RID system — defines the z-axis direction |
| C1, C2, C3 | Real | Coordinates of point C in RID system — together with z-axis defines the x-z plane |

## Key Remarks

- Points A, B, C must be unique and noncolinear
- Point location given by (X, Y, Z)
- Displacement directions at point P: (u_x, u_y, u_z)
- Changes to CORD2R on restart trigger a complete re-analysis
