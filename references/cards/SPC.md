# SPC — Single-Point Constraint

Defines a set of single-point constraints and enforced motion (enforced displacements in static analysis; enforced displacements, velocities, or accelerations in dynamic analysis).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SPC | SID | G1 | C1 | D1 | G2 | C2 | D2 | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Identification number of the single-point constraint set. Selected by Case Control `SPC = SID` |
| Gi | Integer > 0 | Grid or scalar point identification number |
| Ci | Integer 0–6 | Component numbers. Up to six unique integers 1–6 with no embedded blanks (e.g. `123456`). 0 or 1 for scalar points |
| Di | Real | Enforced displacement value at components Ci of grid Gi. Default = 0.0 |

## Key Remarks

- **1 to 12 degrees-of-freedom** may be specified per entry (two G/C/D groups).
- Selected by Case Control `SPC = SID`.
- DOFs specified on this entry form members of the mutually exclusive s-set. They may not be specified on other entries that define mutually exclusive sets (e.g., they cannot also appear on another SPC, SPC1, or PS field of GRID).
- Single-point forces of constraint are recovered during stress data recovery.
- DOFs may be redundantly specified as permanent constraints using the PS field on the GRID entry.
- **Preferred method for enforced motion:** the SPCD entry is more efficient than the Di field here. Use SPCD when specifying non-zero enforced displacements.
- **SOL 400 distinction:** the SPC entry requests enforced **total** displacement (Di). SPC1 requests null enforced relative displacement per step. See SPCD and SPCR entries for more.
- **Thermal analysis:** specifies a constant temperature boundary condition on the selected grid or scalar point.
- SOL 129 transient analysis does not support the Di enforced motion option; use SOL 400.

## Example

```
$       SID     G1      C1      D1      G2      C2      D2
SPC     2       32      3       -2.6    5
```

Grid 32, DOF 3, enforced displacement −2.6. Grid 5, DOF blank (all), enforced displacement 0.0.
