# PROD — Rod Property

Defines the cross-sectional properties of a rod element (CROD entry).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PROD | PID | MID | A | J | C | NSM | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property identification number |
| MID | Integer > 0 | Material ID — references MAT1 for structural analysis |
| A | Real | Cross-sectional area |
| J | Real | Torsional stiffness constant |
| C | Real; Default = 0.0 | Torsional stress coefficient. Torsional stress τ = C × M₀ / J, where M₀ is the torsional moment. C = 0.0 → no torsional stress output. |
| NSM | Real | Nonstructural mass per unit length |

## Key Remarks

- PROD references MAT1 for structural analyses. For heat transfer analyses, MID references MAT4 or MAT5.
- A CROD element carries axial force (tension/compression) and torsional moment only — no bending or shear. Both A and J are required for the element to be stiff in both modes.
- Set C = 0.0 (default) if torsional stress output is not needed; set C to the appropriate cross-section factor (e.g. the radius for a solid circular rod) to recover torsional stress.
- PROD is a primary property entry — PID must be unique with respect to all other PROD entries.
