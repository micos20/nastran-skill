# PCOMP — Layered Composite Element Property

Defines the properties of an n-ply composite material laminate.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PCOMP | PID | Z0 | NSM | SB | FT | TREF | GE | LAM | |
| | MID1 | T1 | THETA1 | SOUT1 | MID2 | T2 | THETA2 | SOUT2 | |
| | MID3 | T3 | THETA3 | SOUT3 | -etc.- | | | | |

One or more ply continuation lines are required. Plies are numbered from the bottom (largest −Z).

## Fields

| Name | Type | Description |
|---|---|---|
| PID | 0 < Integer < 10,000,000 | Property ID (unique w.r.t. PSHELL, PCOMP, PCOMPG) |
| Z0 | Real; Default = −0.5 × total thickness | Distance from reference plane to bottom surface |
| NSM | Real | Nonstructural mass per unit area |
| SB | Real > 0.0 | Allowable interlaminar shear stress; required if FT is specified |
| FT | Character or blank | Failure theory: "HILL", "HOFF", "TSAI", "STRN", "HFAIL", "HTAPE", "HFABR". Blank = no failure calc |
| TREF | Real; Default = 0.0 | Reference temperature |
| GE | Real; Default = 0.0 | Structural damping coefficient (overrides material GE for all plies) |
| LAM | Character or blank | Laminate option: blank (all plies, all terms), "SYM" (half-plies, mirrored), "MEM", "BEND", "SMEAR", "SMCORE" |
| MIDi | Integer > 0 or blank | Material ID for ply i — references MAT1, MAT2, MAT8, or MATDIGI. MID1 must be specified |
| Ti | Real > 0.0 or blank | Ply thickness. T1 must be specified; others default to last specified Ti |
| THETAi | Real; Default = 0.0 | Ply orientation angle (deg) relative to element x-axis |
| SOUTi | "YES" or "NO"; Default = "NO" | Request ply stress/strain output |

## Key Remarks

- Plies numbered starting at 1 from the bottom layer (most negative Z face)
- MID2...MIDn default to last specified MIDi; same for Ti
- GE on PCOMP overrides GE values on all referenced material entries
- PID must be unique with respect to PSHELL, PCOMP, and PCOMPG entries
- For failure index output: need STRESS or STRAIN case control, SB+FT+SOUTi on PCOMP, and Xt/Xc/Yt/Yc/S on referenced MAT8 entries
- LAM="SYM": specify only bottom half of plies; remaining plies are mirrored
- Temperature-dependent ply properties only in SOL 106 and SOL 400 (see PARAM,COMPMATT)
