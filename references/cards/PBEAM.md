# PBEAM — Tapered Beam Element Property

Defines the properties of a tapered beam element (CBEAM), with optional variation of cross-section along the span.

## Format

First two lines define end A (required):

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PBEAM | PID | MID | A(A) | I1(A) | I2(A) | I12(A) | J(A) | NSM(A) | |
| | C1(A) | C2(A) | D1(A) | D2(A) | E1(A) | E2(A) | F1(A) | F2(A) | |

Intermediate station lines (optional, repeatable for each station):

| | SO | X/XB | A | I1 | I2 | I12 | J | NSM | |
| | C1 | C2 | D1 | D2 | E1 | E2 | F1 | F2 | |

Optional final two continuation lines:

| | K1 | K2 | S1 | S2 | NSI(A) | NSI(B) | CW(A) | CW(B) | |
| | M1(A) | M2(A) | M1(B) | M2(B) | N1(A) | N2(A) | N1(B) | N2(B) | |

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property ID (unique w.r.t. PBEAM, PBEAML, PBCOMP, PBMSECT, PBEAM3) |
| MID | Integer > 0 | Material ID — references MAT1 for structural |
| A(A) | Real > 0.0 | Cross-sectional area at end A (required) |
| I1(A) | Real > 0.0 | Area moment of inertia I_zz at end A (required) |
| I2(A) | Real > 0.0 | Area moment of inertia I_yy at end A (required) |
| I12(A) | Real; Default = 0.0 | Area product of inertia at end A. Must satisfy I1×I2 − I12² > 0 |
| J(A) | Real ≥ 0.0; Default = 0.0 | Torsional stiffness constant at end A. Must be > 0.0 if warping is included |
| NSM(A) | Real; Default = 0.0 | Nonstructural mass per unit length at end A |
| C1(A)–F2(A) | Real; Default = 0.0 | (y, z) coordinates of stress recovery points C, D, E, F at end A (element cross-section CS) |
| SO | Character | Stress output option at station: "YES" = stress/strain output, "YESA" = same as end A, "NO" = no output (required for each station) |
| X/XB | 0.0 < Real ≤ 1.0 | Fractional distance from end A to station along span. X/XB = 1.0 defines end B. Must be unique and increasing. Required if any intermediate stations present |
| A, I1, I2... | Real | Cross-section properties at intermediate station — same meaning as end A fields. Blank fields are linearly interpolated from end A to end B |
| K1 | Real; Default = 1.0 | Transverse shear stiffness factor in plane 1. K1 = 0.0 → Bernoulli-Euler (no shear deformation) |
| K2 | Real; Default = 1.0 | Transverse shear stiffness factor in plane 2. K2 = 0.0 → Bernoulli-Euler |
| S1, S2 | Real; Default = 0.0 | Shear relief coefficients due to taper (planes 1 and 2) |
| NSI(A), NSI(B) | Real; Default = 0.0 | Nonstructural mass moment of inertia per unit length at ends A and B |
| CW(A), CW(B) | Real; Default = 0.0 | Warping coefficients at ends A and B |
| M1(A), M2(A) | Real; Default = 0.0 | (y, z) coordinates of nonstructural mass centroid at end A |
| M1(B), M2(B) | Real; Default = 0.0 | (y, z) coordinates of nonstructural mass centroid at end B |
| N1(A), N2(A) | Real; Default = 0.0 | (y, z) coordinates of neutral axis at end A |
| N1(B), N2(B) | Real; Default = 0.0 | (y, z) coordinates of neutral axis at end B |

## Key Remarks

- If intermediate station lines are present, a station with X/XB = 1.0 (end B) must be included as the last station
- Blank section properties at intermediate stations are linearly interpolated between the bounding defined stations
- K1 = K2 = 0.0 gives a Bernoulli-Euler beam (no transverse shear deformation)
- K1 = K2 = 1.0 (default) includes shear deformation; use a correction factor < 1.0 for realistic cross-sections (e.g. 0.833 for rectangles)
- To include torsional warping stiffness: specify CW(A), CW(B), and provide SA, SB on the CBEAM entry
- The optional last two continuation lines may be omitted if warping, NSI, CG offsets, and neutral axis offsets are not needed
- If no stations are defined, properties are constant along the beam (prismatic)
