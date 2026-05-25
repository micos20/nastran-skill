# CBAR — Simple Beam Element Connection

Defines a simple beam element.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CBAR | EID | PID | GA | GB | X1 | X2 | X3 | OFFT | |
| | PA | PB | W1A | W2A | W3A | W1B | W2B | W3B | |

Alternate: G0 replaces X1/X2/X3:

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CBAR | EID | PID | GA | GB | G0 | | | OFFT | |

## Fields

| Name | Type | Description |
|---|---|---|
| EID | 0 < Integer < 100,000,000 | Unique element identification number |
| PID | Integer > 0; Default = EID | Property ID referencing a PBAR, PBARL, or PBRSECT entry |
| GA, GB | Integer > 0; GA ≠ GB | Grid points at ends A and B |
| X1, X2, X3 | Real | Components of orientation vector v̂ from GA in the displacement coordinate system at GA |
| G0 | Integer > 0; G0 ≠ GA, GB | Alternate: grid point for orientation vector — direction is from GA to G0 |
| OFFT | Character or blank | Offset vector interpretation flag (default = GGG). See Key Remarks |
| PA, PB | Integer 1–6, no blanks | Pin flags at ends A and B — removes selected DOFs. Up to 5 unique integers |
| W1A–W3B | Real; Default = 0.0 | Offset vectors at ends A (W*A) and B (W*B) in coordinate system per OFFT |

\* PID, OFFT defaults can be set via the BAROR entry.

## Key Remarks

- Element x-axis runs from GA to GB; orientation vector v̂ defines the x-y plane (y-elem in plane of v̂ and x-elem)
- Continuation line may be omitted if there are no pin flags or offsets
- OFFT character string controls coordinate system for orientation vector and offset vectors: first character = orientation vector system, second = end A offset, third = end B offset. G = global (displacement CS at grid), B = basic, O = offset CS. Default GGG
- For SOL 103 (with preloading), SOL 105, SOL 400 with offsets: add MDLPRM,OFFDEF,LROFF
- BAR is treated as linear in SOL 106 and SOL 400; for geometric nonlinear effects use MDLPRM,BRTOBM,1
