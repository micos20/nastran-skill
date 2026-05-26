# RBE3 — Interpolation Constraint Element

Defines the motion at a reference grid point as a weighted average of the motions at a set of other grid points. Used to apply loads or to interpolate displacements — NOT a rigid element.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| RBE3 | EID | | REFGRID | REFC | WT1 | C1 | G1,1 | G1,2 | |
| | G1,3 | WT2 | C2 | G2,1 | G2,2 | -etc.- | WT3 | C3 | |
| | G3,1 | G3,2 | -etc.- | WT4 | C4 | G4,1 | G4,2 | -etc.- | |
| | "UM" | GM1 | CM1 | GM2 | CM2 | GM3 | CM3 | | |
| | GM4 | CM4 | GM5 | CM5 | -etc.- | | | | |
| | "ALPHA" | ALPHA | TREF | | | | | | |

Notes:
- Field 3 on the first line is always blank.
- The `"UM"` and `"ALPHA"` rows are optional; omit entirely if not needed.
- Blank spaces may be left at the end of a Gi,j sequence to pad to the next WTi entry.

## Fields

| Name | Type | Description |
|---|---|---|
| EID | Integer; 0 < n < 100,000,000 | Element identification number |
| REFGRID | Integer > 0 | Reference grid point — the constrained (dependent) node |
| REFC | Integers 1–6; no embedded blanks | DOF components at REFGRID that are constrained by the interpolation |
| WTi | Real | Weighting factor for grid group i |
| Ci | Integers 1–6; no embedded blanks | DOF components with weighting factor WTi at grid points Gi,j |
| Gi,j | Integer > 0 | Grid points in weighting group i that contribute with components Ci |
| "UM" | Character literal | Optional: marks start of explicit dependent DOF section |
| GMi | Integer > 0 | Dependent grid points (m-set override; only needed when default REFC assignment is insufficient) |
| CMi | Integers 1–6; no embedded blanks | DOF components at GMi to be placed in the m-set |
| "ALPHA" | Character literal | Marks that the next field is the thermal expansion coefficient |
| ALPHA | Real or blank | Thermal expansion coefficient |
| TREF | Real; Default = 0.0 | Reference temperature for thermal load calculation |

## Key Remarks

- **RBE3 is not a rigid element** — it does not add stiffness. It constrains the REFGRID DOFs to be the weighted average of the contributing grid DOFs. Stiffness is only affected through the constraint equations.
- **Typical use**: applying a total force or moment to a surface patch — define REFGRID at the load application point, list the surrounding grids with appropriate weights (usually WT=1.0, C=123 for each).
- For most applications, use only translational components (C = 123) for Ci at the contributing grids. Use rotational components only when Gi,j grids are collinear (to avoid rigid body modes).
- **Lagrange method**: REFC must be exactly "123", "456", or "123456". No other combination is allowed.
- The `"UM"` section is only needed when explicit control over which DOFs enter the m-set is required. By default, the components in REFC at REFGRID form the dependent set.
- If the `"UM"` section is used, the number of m-set components must equal the number of REFC components, and they must be a subset of the REFC and (Gi,j, Ci) DOFs.
- When the number of weighted grids Gi,j is large (> 3700) or they are far from REFGRID (Lc/Lmodel > 0.29), the constraint may not be fully enforced with RIGID=LAGRAN. Consider setting MDLPRM,LRGDHFLT to 1.0E-14 or smaller.
- Dependent DOFs assigned by RBE3 may not also be assigned dependent by another rigid element or MPC.
- Forces of multipoint constraint recoverable in all solutions except SOL 129 (use MPCFORCE).
