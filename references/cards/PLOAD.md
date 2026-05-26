# PLOAD — Static Pressure Load

Defines a uniform static pressure load on a triangular or quadrilateral surface defined directly by grid points (not by element ID).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PLOAD | SID | P | G1 | G2 | G3 | G4 | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| P | Real | Pressure value. Negative value reverses the direction of the load |
| G1, G2, G3 | Integer > 0 | Grid point IDs defining the loaded surface (required) |
| G4 | Integer > 0 or blank | Fourth grid point for quadrilateral surface. If zero or blank, a triangular surface is assumed |

## Key Remarks

- Selected by Case Control command `LOAD = SID` (directly or via a LOAD combination entry).
- **Direction:** computed by the right-hand rule applied to the sequence G1 → G2 → G3. A negative pressure P reverses this direction.
- **Triangular surface:** G4 blank or zero. Total load = P × area, distributed equally as three concentrated grid point loads.
- **Quadrilateral surface:** G4 specified; G1, G2, G3, G4 should form a consecutive sequence around the perimeter. The surface is split into two overlapping triangles for calculation, with one-half the pressure applied to each triangle.
- PLOAD applies pressure directly to the geometry defined by the grid points — it is not tied to a specific element. For element-based uniform pressure on shells, use PLOAD2; for more general face pressure on shells and solids, use PLOAD4.

## Example

```
$       SID     P       G1      G2      G3      G4
PLOAD   1       -4.0    16      32      11
```

Triangular surface (G4 blank), pressure −4.0 acting in the direction opposite to the G1→G2→G3 right-hand normal.
