# CTRIA3 — Triangular Plate Element Connection

Defines an isoparametric membrane-bending or plane strain triangular plate element.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CTRIA3 | EID | PID | G1 | G2 | G3 | THETA or MCID | ZOFFS | | |
| | | TFLAG | T1 | T2 | T3 | | | | |

Continuation line is optional.

## Fields

| Name | Type | Description |
|---|---|---|
| EID | 0 < Integer < 100,000,000 | Unique element identification number |
| PID | Integer > 0; Default = EID | Property ID referencing PSHELL, PCOMP, PCOMPG, or PLPLANE |
| G1–G3 | Integer > 0, all unique | Grid points at corners |
| THETA | Real; Default = 0.0 | Material property orientation angle (degrees). Ignored for hyperelastic elements |
| MCID | Integer ≥ 0 | Material coordinate system ID (used instead of THETA). If blank → THETA = 0.0 |
| ZOFFS | Real | Offset from grid point surface to element reference plane (along element z-axis). When used, MID1 and MID2 must be specified on PSHELL |
| TFLAG | Integer 0, 1, or blank | Thickness interpretation: 0/blank = actual thickness; 1 = fraction of PSHELL T |
| T1–T3 | Real ≥ 0.0 or blank | Nodal membrane thicknesses. Default = T on PSHELL entry |

## Key Remarks

- Continuation is optional; if omitted, T1–T3 = T from PSHELL
- ZOFFS: positive value offsets reference plane along positive element z-axis. Requires MID1 and MID2 on PSHELL. For SOL 103/105/400: add MDLPRM,OFFDEF,LROFF
- MCID: for CORD1R/CORD2R, x-axis projected; for CORD1C/CORD2C, r-axis projected
- All three edges considered straight by default
- Output coordinate system = element system (default for non-hyperelastic elements)
- Element x-axis lies along G1→G2 direction
