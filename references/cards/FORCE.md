# FORCE — Static Force

Defines a static concentrated force at a grid point by specifying a scale factor and a direction vector measured in a coordinate system.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| FORCE | SID | G | CID | F | N1 | N2 | N3 | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| G | Integer > 0 | Grid point identification number where the force is applied |
| CID | Integer ≥ 0 | Coordinate system for the direction vector. Default = 0 (basic coordinate system) |
| F | Real | Scale factor |
| N1, N2, N3 | Real | Components of the direction vector in coordinate system CID. At least one Ni must be nonzero unless F is zero |

## Key Remarks

- The applied force is **f** = F·**N**, where **N** = (N1, N2, N3). The magnitude is F × |**N**|; the direction is that of **N**.
- CID = 0 or blank references the basic coordinate system — no coordinate system card is needed in that case.
- Selected by Case Control `LOAD = SID` (directly or via a LOAD combination entry).
- In dynamic solution sequences, SID must be referenced in the EXCITEID field of an RLOADi or TLOADi entry (or via a LOADSET/LSEQ entry).

## Example

```
$       SID     G       CID     F       N1      N2      N3
FORCE   2       5       6       2.9     0.0     1.0     0.0
```

Force of magnitude 2.9 at grid point 5, in the +Y direction of coordinate system 6.
