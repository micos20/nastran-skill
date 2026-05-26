# SPC1 — Single-Point Constraint, Alternate Form

Defines a set of single-point constraints by applying a single component code to a list (or THRU range) of grid points.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SPC1 | SID | C | G1 | G2 | G3 | G4 | G5 | G6 | |
| | G7 | G8 | G9 | -etc.- | | | | | |

**Alternate format (THRU range):**

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SPC1 | SID | C | G1 | "THRU" | G2 | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Identification number of the single-point constraint set. Selected by Case Control `SPC = SID` |
| C | Integer | Component numbers to be constrained. Any unique combination of integers 1–6 with no embedded blanks (e.g. `123456`). 0, 1, or blank for scalar points |
| Gi | Integer > 0 or "THRU" | Grid or scalar point identification numbers. For THRU: G1 < G2 |

## Key Remarks

- Selected by Case Control `SPC = SID`.
- Unlike SPC, **SPC1 has no enforced displacement field** — it always enforces zero displacement for the listed components. To specify non-zero enforced displacement, pair SPC1 with a SPCD entry.
- DOFs form members of the mutually exclusive s-set; they may not appear on other mutually exclusive set entries (another SPC, SPC1, or PS field on GRID).
- **THRU option:** if the alternate format is used, points in the range G1 through G2 are not required to exist. Points that do not exist produce a warning message but are otherwise ignored.
- DOFs may be redundantly specified as permanent constraints using the PS field on the GRID entry.
- **SOL 400 distinction:** SPC1 requests null enforced **relative** displacement per step. SPC requests enforced total displacement. See SPC, SPCD, and SPCR for comparison.
- **Thermal analysis:** used with SPCD to specify a temperature boundary condition on the selected grid or scalar point.

## Example

```
$       SID     C       G1      G2      G3      G4      G5      G6
SPC1    3       2       1       3       10      9       6       5
$               G7      G8
+               2       8
```

Constrain DOF 2 to zero at grids 1, 3, 10, 9, 6, 5, 2, 8.

```
$       SID     C       G1              G2
SPC1    313     12456   6       THRU    32
```

Constrain DOFs 1, 2, 4, 5, 6 to zero at all grids from 6 through 32.
