# RBE1 — Rigid Body Element, Form 1

Defines a rigid body connected to an arbitrary number of independent grid points, with a separate set of dependent grid points.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| RBE1 | EID | GN1 | CN1 | GN2 | CN2 | GN3 | CN3 | | |
| | GN4 | CN4 | GN5 | CN5 | GN6 | CN6 | | | |
| | "UM" | GM1 | CM1 | GM2 | CM2 | GM3 | CM3 | | |
| | GM4 | CM4 | -etc.- | ALPHA | TREF | | | | |

Notes:
- The first continuation line (GN4–CN6) is not required if fewer than 4 GN points are used.
- The `"UM"` literal and subsequent GM/CM pairs define the dependent DOFs.

## Fields

| Name | Type | Description |
|---|---|---|
| EID | Integer; 0 < n < 100,000,000 | Element identification number |
| GNi | Integer > 0 | Independent grid points |
| CNi | Integers 1–6; no embedded blanks | Independent DOF component codes at GNi in the global coordinate system |
| "UM" | Character literal | Marks the start of the dependent DOF section |
| GMj | Integer > 0 | Dependent grid points |
| CMj | Integers 1–6; no embedded blanks | Dependent DOF component codes at GMj in the global coordinate system |
| ALPHA | Real or blank | Thermal expansion coefficient |
| TREF | Real; Default = 0.0 | Reference temperature for thermal load calculation |

## Key Remarks

- **Linear method**: the total number of components in CN1 through CN6 must equal six. All six rigid body modes must be represented (e.g. CN1=123, CN2=2, CN3=2, CN4=3).
- **Lagrange method** (selected via RIGID Case Control): CN1=123456, CN2 through CN6 must be blank.
- A DOF cannot be simultaneously independent and dependent for the same element.
- Dependent DOFs (CMj) may not also be assigned as dependent on another rigid element or MPC.
- Forces of multipoint constraint are recoverable in all solution sequences except SOL 129 (use MPCFORCE Case Control).
- Rigid elements are ignored in heat transfer analyses.
- RBE1 is less common than RBE2/RBE3; prefer RBE2 for a single independent hub node or RBE3 for weighted interpolation constraints.
