# CROD — Rod Element Connection

Defines a tension-compression-torsion rod element between two grid points.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CROD | EID | PID | G1 | G2 | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| EID | Integer; 0 < n < 100,000,000 | Element identification number |
| PID | Integer > 0; Default = EID | Property ID — references a PROD entry |
| G1, G2 | Integer > 0; G1 ≠ G2 | Grid point IDs at the two ends |

## Key Remarks

- CROD carries axial (tension/compression) and torsional loads only — no bending. Both translational DOFs at each end along the element axis are active.
- PID references a PROD entry which defines cross-sectional area, torsional constant, and material.
- Only one element may be defined on a single CROD entry.
- For an element that combines rod and beam properties, see CBAR or CBEAM.
- See CONROD for an alternative format that combines element connectivity and cross-section properties on a single entry (no separate PROD required).
