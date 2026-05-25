# MAT1 — Isotropic Material

Defines the material properties for linear, temperature-independent, isotropic materials.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| MAT1 | MID | E | G | NU | RHO | A | TREF | GE | |
| | ST | SC | SS | MCSID | | | | | |

Continuation line is optional.

## Fields

| Name | Type | Description |
|---|---|---|
| MID | Integer > 0 | Unique material identification number (unique across all MAT* entries) |
| E | Real ≥ 0.0 or blank | Young's modulus. E and G may not both be blank |
| G | Real ≥ 0.0 or blank | Shear modulus. If one of E, G, NU is blank, it is computed from E = 2(1+NU)G |
| NU | −1.0 < Real ≤ 0.5 or blank | Poisson's ratio |
| RHO | Real | Mass density |
| A | Real | Thermal expansion coefficient |
| TREF | Real; Default = 0.0 | Reference temperature for thermal expansion. Default = 0.0 if A is specified |
| GE | Real | Structural damping coefficient. Ignored in transient analysis unless PARAM,W4 is specified |
| ST | Real ≥ 0.0 or blank | Tensile stress allowable (for margins of safety only, not used in SOL 400 with advanced elements) |
| SC | Real ≥ 0.0 or blank | Compressive stress allowable |
| SS | Real ≥ 0.0 or blank | Shear stress allowable |
| MCSID | Integer ≥ 0 or blank | Material coordinate system ID for use with PARAM,CURV |

## Key Remarks

- E and G may not both be blank; if one of E, G, NU is blank it is computed from E = 2(1+NU)G
- TREF and GE are ignored if this entry is referenced by PCOMP or PCOMPG (those entries define their own values)
- Temperature-dependent properties defined via MATT1 entry
- In beam elements: E governs extension and bending; G governs torsion and transverse shear
- ST, SC, SS used only for margin of safety calculations; not used in SOL 400 advanced element failure
