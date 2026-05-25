# CBEAM — Beam Element Connection

Defines a beam element.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CBEAM | EID | PID | GA | GB | X1 | X2 | X3 | OFFT | |
| | PA | PB | W1A | W2A | W3A | W1B | W2B | W3B | |
| | SA | SB | | | | | | | |

Alternate: G0 replaces X1/X2/X3:

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CBEAM | EID | PID | GA | GB | G0 | | | OFFT | |

## Fields

| Name | Type | Description |
|---|---|---|
| EID | 0 < Integer < 100,000,000 | Unique element identification number |
| PID | Integer > 0; Default = EID | Property ID referencing PBEAM, PBCOMP, PBEAML, or PBMSECT |
| GA, GB | Integer > 0; GA ≠ GB | Grid points at ends A and B |
| X1, X2, X3 | Real | Components of orientation vector v̂ from GA in displacement coordinate system at GA |
| G0 | Integer > 0; G0 ≠ GA, GB | Alternate: grid point for orientation — direction from GA to G0 |
| OFFT | Character or blank | Offset/orientation vector interpretation flag (default = GGG) |
| PA, PB | Integer 1–6, no blanks | Pin flags at ends A and B — removes selected DOFs |
| W1A–W3B | Real; Default = 0.0 | Offset vectors from grid points to shear center at ends A and B |
| SA, SB | Integer ≥ 0 or blank | Scalar or grid point IDs for warping variable dθ/dx at ends A and B |

\* PID, OFFT defaults can be set via the BEAMOR entry.

## Key Remarks

- Both continuation lines may be omitted if no pin flags, offsets, or warping variables are needed
- If the second continuation (SA, SB) is omitted, torsional warping stiffness is not included
- If field 6 is integer → G0; if blank or real → X1, X2, X3
- OFFT: same code as CBAR (G = global/displacement CS, B = basic, O = offset CS); default GGG
- For SOL 103 (with preloading), SOL 105, SOL 400 with offsets: use MDLPRM,OFFDEF,LROFF
- Pin flags not allowed in SOL 700 and should not be used in nonlinear analysis with large displacement
