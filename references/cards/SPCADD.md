# SPCADD — Single-Point Constraint Set Combination

Defines a single-point constraint set as the union of single-point constraint sets defined on SPC or SPC1 entries.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SPCADD | SID | S1 | S2 | S3 | S4 | S5 | S6 | S7 | |
| | S8 | S9 | -etc.- | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Single-point constraint set identification number. Selected by Case Control `SPC = SID` |
| Si | Integer > 0 | Identification numbers of single-point constraint sets defined via SPC or SPC1 entries |

## Key Remarks

- Selected by Case Control `SPC = SID`, the same command that selects individual SPC/SPC1 entries.
- The Si values must be unique. No Si may be the identification number of a single-point constraint set defined by another SPCADD entry (no nesting).
- **SPCADD takes precedence over SPC entries.** If both an SPCADD and an SPC/SPC1 entry reference the same SID, only the SPCADD is used.
- If Modules are present, this entry may only be specified in the main Bulk Data section.

## Example

```
$       SID     S1      S2      S3      S4
SPCADD  101     3       2       9       1
```

SPC set 101 is the union of sets 3, 2, 9, and 1.
