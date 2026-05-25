# CQUAD4 — Quadrilateral Plate Element Connection

Defines an isoparametric membrane-bending or plane strain quadrilateral plate element.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CQUAD4 | EID | PID | G1 | G2 | G3 | G4 | THETA or MCID | ZOFFS | |
| | | TFLAG | T1 | T2 | T3 | T4 | | | |

Continuation line is optional.

## Fields

| Name | Type | Description |
|---|---|---|
| EID | 0 < Integer < 100,000,000 | Unique element identification number |
| PID | Integer > 0; Default = EID | Property ID referencing PSHELL, PCOMP, PCOMPG, or PLPLANE |
| G1–G4 | Integer > 0, all unique | Grid points at corners, ordered consecutively around the perimeter |
| THETA | Real; Default = 0.0 | Material property orientation angle (degrees). Ignored for hyperelastic elements |
| MCID | Integer ≥ 0 | Material coordinate system ID (used instead of THETA). If blank → THETA = 0.0 |
| ZOFFS | Real | Offset distance from grid point plane to element reference plane (along element z-axis) |
| TFLAG | Integer 0, 1, or blank | Thickness interpretation: 0/blank = actual thickness; 1 = fraction of PSHELL T |
| T1–T4 | Real ≥ 0.0 or blank | Nodal membrane thicknesses. Default = T on PSHELL entry |

## Key Remarks

- G1–G4 must be ordered consecutively around the perimeter; all interior angles < 180°
- Continuation is optional; if omitted, T1–T4 = T from PSHELL
- ZOFFS offsets the reference plane; when used, both MID1 and MID2 must be specified on PSHELL. For SOL 103/105/400: add MDLPRM,OFFDEF,LROFF
- MCID: for CORD1R/CORD2R, x-axis projected onto shell surface; for CORD1C/CORD2C, r-axis projected
- Output coordinate system = element system (or material system with PARAM,OMID)
- If PSHLN1 or PLCOMP with BEHi=COMPS is associated, THETA/MCID is ignored
