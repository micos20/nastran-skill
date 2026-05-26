# CGAP — Gap Element Connection

Defines a gap or friction element between two grid points.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CGAP | EID | PID | GA | GB | X1 | X2 | X3 | CID | |

Alternate format (orientation via grid point):

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CGAP | EID | PID | GA | GB | GO | | | CID | |

## Fields

| Name | Type | Description |
|---|---|---|
| EID | Integer; 0 < n < 100,000,000 | Element identification number |
| PID | Integer > 0; Default = EID | Property ID — references a PGAP entry |
| GA, GB | Integer > 0; GA ≠ GB | Grid point IDs at ends A and B |
| X1, X2, X3 | Real | Components of orientation vector v̄ from GA, in displacement coordinate system at GA. Defines the gap element y-axis. |
| GO | Integer > 0 | Alternative: orientation vector v̄ is directed from GA to GO |
| CID | Integer ≥ 0 or blank | Element coordinate system ID. Required if GA and GB are coincident (distance < 10⁻⁴). |

## Key Remarks

- CGAP is designed for nonlinear solutions (SOL 106, 129, 153, 159, 400). In linear solutions it produces a linear stiffness matrix using the initial stiffness (KA or KB depending on U0).
- The element x-axis is the gap/contact direction (compression-active). Force Fₓ is positive for compression.
- Element coordinate system is defined by two alternative methods:
  - **CID specified**: element x-axis = T1, y-axis = T2 of the CID coordinate system; orientation vector is ignored.
  - **CID blank, GA ≠ GB**: element x-axis = GA→GB direction; orientation vector v̄ defines the x-y plane (same convention as CBEAM).
- CID must be specified when GA and GB are coincident (e.g. for a node-to-node contact point).
- The element coordinate system does not rotate with deflections.
- Initial gap opening is specified in the PGAP entry (U0 field), not derived from the GA–GB separation distance.
- With large closed-gap stiffness KA, avoid structural damping via PARAM,G; instead specify damping on MATi entries with PARAM,W4.
- Request output with FORCE or NLSTRESS Case Control command; NLSTRESS (nonlinear only) also outputs gap STATUS.
