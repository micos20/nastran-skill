# MPCADD — Multipoint Constraint Set Combination

Defines a multipoint constraint set as the union of multipoint constraint sets defined via MPC entries.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| MPCADD | SID | S1 | S2 | S3 | S4 | S5 | S6 | S7 | |
| | S8 | S9 | -etc.- | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Set identification number. Selected by Case Control `MPC = SID` |
| Si | Integer > 0 | Identification numbers of MPC constraint sets defined via MPC entries |

## Key Remarks

- Selected by Case Control `MPC = SID`, the same command that selects individual MPC entries.
- The Si values must be unique. No Si may be the identification number of a multipoint constraint set defined by another MPCADD entry (no nesting).
- **MPCADD takes precedence over MPC entries.** If both an MPCADD and an MPC entry reference the same SID, only the MPCADD is used.
- If Modules are present, this entry may only be specified in the main Bulk Data section.

## Example

```
$       SID     S1      S2      S3      S4      S5
MPCADD  101     2       3       1       6       4
```

MPC set 101 is the union of sets 2, 3, 1, 6, and 4.
