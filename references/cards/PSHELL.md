# PSHELL — Shell Element Property

Defines the membrane, bending, transverse shear, and coupling properties of thin shell elements.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PSHELL | PID | MID1 | T | MID2 | 12I/T**3 | MID3 | TS/T | NSM | |
| | Z1 | Z2 | MID4 | | | | | | |

Continuation line is not required.

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property ID (unique w.r.t. PSHELL, PCOMP, PCOMPG) |
| MID1 | Integer ≥ 0 or blank | Material ID for membrane stiffness. Blank = no membrane or coupling stiffness |
| T | Real or blank | Default membrane thickness. If blank, Ti must be specified on each element |
| MID2 | Integer ≥ −1 or blank | Material ID for bending stiffness. Blank = no bending/coupling/shear. −1 = plane strain |
| 12I/T**3 | Real > 0.0; Default = 1.0 | Bending inertia ratio: actual I / (T³/12). Default = 1.0 (homogeneous shell) |
| MID3 | Integer > 0 or blank | Material ID for transverse shear flexibility. Blank if MID2 is blank or −1 |
| TS/T | Real > 0.0; Default = 0.833333 | Transverse shear thickness ratio Ts/T |
| NSM | Real | Nonstructural mass per unit area |
| Z1, Z2 | Real | Fiber distances for stress recovery. Default: Z1 = −T/2, Z2 = +T/2 |
| MID4 | Integer > 0 or blank | Material ID for membrane-bending coupling. Requires MID1 > 0 and MID2 > 0; ≠ MID1, MID2 |

## Key Remarks

- Referenced by CQUAD4, CTRIA3, CQUAD8, CTRIA6, CQUADR elements via PID
- MIDi must reference MAT1, MAT2, or MAT8 for structural problems
- MID1 blank → no membrane stiffness; MID2 blank → no bending; MID3 blank → no transverse shear flexibility; MID4 blank → no membrane-bending coupling
- MID4 is for modeling non-symmetric laminates via offset; prefer ZOFFS on the connection entry instead — MID4 may produce ill-conditioned stiffness if used incorrectly
- MID4 effect is not included in differential stiffness — leave blank in buckling analysis
- Structural damping GE obtained from MID1 material (or MID2 if MID1 blank)
- For plane strain: set MID2 = −1, MID1 references MAT1
