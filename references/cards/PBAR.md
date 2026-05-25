# PBAR — Simple Beam Element Property

Defines the properties of a simple beam element (CBAR).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PBAR | PID | MID | A | I1 | I2 | J | NSM | | |
| | C1 | C2 | D1 | D2 | E1 | E2 | F1 | F2 | |
| | K1 | K2 | I12 | | | | | | |

All continuation lines are optional.

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property ID (unique w.r.t. PBAR, PBARL, PBRSECT) |
| MID | Integer > 0 | Material ID — references MAT1 for structural; MAT4/MAT5 for heat transfer |
| A | Real; Default = 0.0 | Cross-sectional area |
| I1 | Real ≥ 0.0; Default = 0.0 | Area moment of inertia I_zz (bending in x-z plane) |
| I2 | Real ≥ 0.0; Default = 0.0 | Area moment of inertia I_yy (bending in x-y plane) |
| J | Real; Default = 0.0 | Torsional stiffness constant |
| NSM | Real | Nonstructural mass per unit length |
| C1, C2 | Real; Default = 0.0 | (y, z) coordinates of stress recovery point C in element coordinate system |
| D1, D2 | Real; Default = 0.0 | (y, z) coordinates of stress recovery point D |
| E1, E2 | Real; Default = 0.0 | (y, z) coordinates of stress recovery point E |
| F1, F2 | Real; Default = 0.0 | (y, z) coordinates of stress recovery point F |
| K1 | Real or blank | Transverse shear stiffness factor in plane 1: K1 × A × G. Blank = infinite shear stiffness (zero shear flexibility) |
| K2 | Real or blank | Transverse shear stiffness factor in plane 2 |
| I12 | Real; Default = 0.0 | Area product of inertia I_zy. Must satisfy I1 × I2 ≥ I12² |

## Key Remarks

- K1 and K2 must be blank (or 0.0 treated as blank) if A = 0.0
- I12 (product of inertia): if I12 ≠ 0, K1 and K2 are ignored
- Stress recovery points C, D, E, F are in the element y-z cross-section; up to 4 points may be specified
- Applied loads on CBAR act at the shear center (not necessarily the centroid); for non-symmetric sections use warping properties on CBEAM instead
- Both continuation lines may be omitted if stress recovery points, shear factors, and I12 are not needed
