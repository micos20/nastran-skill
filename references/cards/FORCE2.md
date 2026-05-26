# FORCE2 — Follower Force, Alternate Form 2

Defines a concentrated force at a grid point by specifying a magnitude and four grid points whose cross product determines the direction.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| FORCE2 | SID | G | F | G1 | G2 | G3 | G4 | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| G | Integer > 0 | Grid point where the force is applied |
| F | Real | Magnitude of the force |
| G1, G2, G3, G4 | Integer > 0 | Grid points defining direction. G1 and G2 may not be coincident; G3 and G4 may not be coincident |

## Key Remarks

- The force direction is parallel to the cross product of vectors (G1→G2) × (G3→G4). The magnitude equals |F|.
- **Follower force:** the direction tracks deformation. Same follower force effects as FORCE1:
  - Linear differential-stiffness solutions: SOLs 103, 105, 107–112, 115, 116 (see also PARAM,FOLLOWK).
  - Nonlinear solutions: SOLs 106, 129, 153, 159, 400 when PARAM,LGDISP,1 is set.
  - Follower force stiffness is included in nonlinear static solutions (SOLs 106, 153, 400) but not in nonlinear transient dynamic solutions (SOLs 129, 159).
- Selected by Case Control `LOAD = SID` (directly or via a LOAD combination entry).
- In dynamic solution sequences, SID must be referenced in the EXCITEID field of an RLOADi or TLOADi entry (or via LOADSET/LSEQ).

## Example

```
$       SID     G       F       G1      G2      G3      G4
FORCE2  6       13      -2.93   16      13      17      13
```

Force of magnitude −2.93 at grid 13, in the direction of (G16→G13) × (G17→G13).
