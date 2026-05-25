# GRID — Grid Point

Defines the location of a geometric grid point, the directions of its displacement, and its permanent single-point constraints.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| GRID | ID | CP | X1 | X2 | X3 | CD | PS | SEID | |

## Fields

| Name | Type | Description |
|---|---|---|
| ID | Integer, 0 < ID < 100,000,000 | Grid point identification number (unique among all point types) |
| CP | Integer ≥ 0 or blank | Coordinate system in which X1, X2, X3 are defined. Blank/0 = basic system |
| X1, X2, X3 | Real; Default = 0.0 | Location in coordinate system CP |
| CD | Integer ≥ −1 or blank | Coordinate system for displacements, DOFs, constraints, and solution vectors. Blank/0 = basic |
| PS | Integer (1–6, no blanks) or blank | Permanent single-point constraints at this grid point |
| SEID | Integer ≥ 0; Default = 0 | Superelement identification number |

\* CP, CD, PS, SEID defaults can be set globally via GRDSET entry.

## Key Remarks

- X1/X2/X3 interpretation depends on CP type: Rectangular → X, Y, Z; Cylindrical → R, θ (deg), Z; Spherical → R, θ (deg), φ (deg)
- CD = −1 defines a fluid grid point (valid only for CAABSF, CHACBR, CHACAB, CHEXA, CPENTA, CPYRAM, CTETRA elements)
- CP and CD are independent: CP locates the point, CD orients its DOFs
- Avoid defining CD in a cylindrical/spherical system for points on the z-axis
