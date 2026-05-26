# FORCE1 — Follower Force, Alternate Form 1

Defines a concentrated force at a grid point by specifying a magnitude and two grid points that determine the direction.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| FORCE1 | SID | G | F | G1 | G2 | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| G | Integer > 0 | Grid point where the force is applied |
| F | Real | Magnitude of the force |
| G1, G2 | Integer > 0 | Grid points defining the direction. G1 and G2 may not be coincident |

## Key Remarks

- The force direction is the unit vector **n̂** parallel to the vector from G1 to G2: **f** = F·**n̂** where **n̂** = (G2 − G1) / |G2 − G1|.
- **Follower force:** the direction tracks deformation. Follower force effects are included in:
  - Linear differential-stiffness solutions: SOLs 103, 105, 107–112, 115, 116 (see also PARAM,FOLLOWK).
  - Nonlinear solutions: SOLs 106, 129, 153, 159, 400 when PARAM,LGDISP,1 is set.
  - Follower force stiffness is included in nonlinear static solutions (SOLs 106, 153, 400) but not in nonlinear transient dynamic solutions (SOLs 129, 159).
- Selected by Case Control `LOAD = SID` (directly or via a LOAD combination entry).
- In dynamic solution sequences, SID must be referenced in the EXCITEID field of an RLOADi or TLOADi entry (or via LOADSET/LSEQ).

## Example

```
$       SID     G       F       G1      G2
FORCE1  6       13      -2.93   16      13
```

Force of magnitude −2.93 at grid 13, directed from grid 16 toward grid 13.
