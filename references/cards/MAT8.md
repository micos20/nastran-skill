# MAT8 — Orthotropic Material (Shell)

Defines the material properties for an orthotropic material for isoparametric shell elements.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| MAT8 | MID | E1 | E2 | NU12 | G12 | G1Z | G2Z | RHO | |
| | A1 | A2 | TREF | Xt | Xc | Yt | Yc | S | |
| | GE | F12 | STRN | | | | | | |

First continuation is optional; second continuation requires first.

## Fields

| Name | Type | Description |
|---|---|---|
| MID | 0 < Integer < 100,000,000 | Unique material ID (unique across all MAT* entries). Referenced by PSHELL, PCOMP, PCOMPG only |
| E1 | Real > 0.0 | Stiffness in fiber direction (1-direction) |
| E2 | Real > 0.0 | Stiffness in matrix direction (2-direction) |
| NU12 | Real | Poisson's ratio ε2/ε1 when stressed in 1-direction. NU21 = NU12 × E2/E1 |
| G12 | Real > 0.0; Default = 0.0 | In-plane shear modulus |
| G1Z | Real ≥ 0.0 or blank | Transverse shear modulus in 1-Z plane. Blank = infinite (rigid); must be > 0.0 for Advanced Nonlinear elements |
| G2Z | Real ≥ 0.0 or blank | Transverse shear modulus in 2-Z plane. Blank = infinite (rigid); must be > 0.0 for Advanced Nonlinear elements |
| RHO | Real | Mass density |
| A1 | Real | Thermal expansion coefficient in 1-direction |
| A2 | Real | Thermal expansion coefficient in 2-direction |
| TREF | Real or blank | Reference temperature. Ignored if referenced by PCOMP or PCOMPG |
| Xt | Real > 0.0 | Tensile failure allowable in fiber direction. Required for failure index calculation |
| Xc | Real > 0.0; Default = Xt | Compressive failure allowable in fiber direction |
| Yt | Real > 0.0 | Tensile failure allowable in matrix direction |
| Yc | Real > 0.0; Default = Yt | Compressive failure allowable in matrix direction |
| S | Real > 0.0 | In-plane shear failure allowable |
| GE | Real | Structural damping coefficient. Ignored if referenced by PCOMP/PCOMPG; ignored if PARAM,W4 not set |
| F12 | Real | Tsai-Wu interaction term (used with FT="TSAI" on PCOMP) |
| STRN | Real or blank | Failure interpretation: 1.0 = strain allowables; blank = stress allowables |

## Key Remarks

- Used with PSHELL, PCOMP, or PCOMPG entries; not valid for beam or solid elements
- TREF and GE are ignored when referenced by PCOMP or PCOMPG (those entries supply their own values)
- Allowables Xt, Xc, Yt, Yc, S required for failure index output — also need SB + FT on PCOMP and SOUTi = "YES" on affected plies
- G1Z and G2Z blank → no transverse shear flexibility (rigid in thickness direction); must be specified and > 0 for SOL 400 Advanced Nonlinear elements
- Temperature-dependent properties defined via MATT8 entry
