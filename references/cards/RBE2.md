# RBE2 — Rigid Body Element, Form 2

Defines a rigid body with a single independent grid point and an arbitrary number of dependent grid points. The most common rigid element type for bolt/pin connections, rigid links, and MPC attachments.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| RBE2 | EID | GN | CM | GM1 | GM2 | GM3 | GM4 | GM5 | |
| | GM6 | GM7 | GM8 | -etc.- | ALPHA | TREF | | | |

The continuation line repeats as needed for additional dependent grid points (up to 8 GMi per line). ALPHA and TREF appear after the last GMi.

## Fields

| Name | Type | Description |
|---|---|---|
| EID | Integer; 0 < n < 100,000,000 | Element identification number |
| GN | Integer > 0 | Independent grid point — all six DOFs here drive the dependent DOFs |
| CM | Integers 1–6; no embedded blanks | DOF component numbers to be constrained at all dependent grids GMi in the global coordinate system |
| GMi | Integer > 0 | Dependent grid point IDs (any number) |
| ALPHA | Real or blank | Thermal expansion coefficient |
| TREF | Real; Default = 0.0 | Reference temperature for thermal load calculation |

## Key Remarks

- **Typical use**: GN = hub/reference node (e.g. bolt hole centre), GMi = surrounding shell or solid nodes, CM = 123456 (all 6 DOFs constrained). This is the standard "spider" rigid element.
- CM must include "456" (rotational DOFs) if large rotations are used in the model.
- The dependent DOFs listed in CM are constrained to follow GN as a rigid body at every GMi node simultaneously — they cannot be assigned as dependent by another rigid element or MPC.
- Dependent DOFs must not be in mutually exclusive m-set definitions.
- Forces of multipoint constraint are recoverable in all solutions except SOL 129 (use MPCFORCE Case Control).
- Rigid elements are ignored in heat transfer analyses.
- For the Lagrange method (RIGID Case Control = LAGRAN): the number of Lagrange multiplier DOFs equals CM × (number of GMi).
