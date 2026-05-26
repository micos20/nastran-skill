# MPC — Multipoint Constraint

Defines a multipoint constraint equation of the form Σ Aj·uj = 0, where uj represents degree-of-freedom Cj at grid or scalar point Gj.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| MPC | SID | G1 | C1 | A1 | G2 | C2 | A2 | | |
| | G3 | C3 | A3 | -etc.- | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Set identification number. Selected by Case Control `MPC = SID` |
| Gj | Integer > 0 | Grid or scalar point identification number |
| Cj | Integer 0–6 | Component number. Any one of 1–6 for grid points; blank, 0, or 1 for scalar points |
| Aj | Real | Coefficient. Default = 0.0 except A1, which must be nonzero |

## Key Remarks

- The first term (G1, C1) defines the **dependent** degree-of-freedom. A1 must be nonzero.
- A dependent DOF assigned by one MPC entry cannot be assigned as dependent by another MPC entry or by a rigid element (RBE1, RBE2, RBE3, RBAR, etc.).
- All terms beyond the first are **independent** DOFs. The constraint is enforced as: A1·u1 + A2·u2 + A3·u3 + ... = 0, so u1 = −(A2·u2 + A3·u3 + ...) / A1.
- MPC constraint forces may be recovered in all solution sequences except SOL 129, using the Case Control command `MPCFORCE`.
- The m-set DOFs specified on this entry may not appear on other entries that define mutually exclusive sets (see Degree-of-Freedom Sets in the QRG).
- If both an MPC and an MPCADD entry reference the same SID, the MPCADD takes precedence — only the MPCADD is used.

## Example

```
$       SID     G1      C1      A1      G2      C2      A2
MPC     3       28      3       6.2     2               4.29
$               G3      C3      A3
+               1       4       -2.91
```

Constraint: 6.2·u(grid 28, dof 3) + 4.29·u(grid 2, dof blank) − 2.91·u(grid 1, dof 4) = 0.
