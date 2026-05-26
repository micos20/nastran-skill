# PLOAD4 — Pressure Load on Surface and Faces of Solid Elements

Defines a pressure load on the face of a shell or solid element. The most general pressure entry — supports non-uniform (corner-varying) pressure, arbitrary direction vectors, and element ranges. Applicable to both 2D shell elements and faces of 3D solid elements.

## Format

Standard (single element or THRU range):

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PLOAD4 | SID | EID | P1 | P2 | P3 | P4 | G1 | G3 or G4 | |
| | CID | N1 | N2 | N3 | SORL | LDIR | | | |

Alternate (THRU range):

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PLOAD4 | SID | EID1 | P1 | P2 | P3 | P4 | "THRU" | EID2 | |
| | CID | N1 | N2 | N3 | SORL | LDIR | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| EID / EID1, EID2 | Integer > 0 | Element ID (standard) or ID range (THRU; EID1 < EID2) |
| P1 | Real or blank | Pressure at corner G1 of the loaded face. Required; sets the uniform pressure if P2–P4 are blank |
| P2, P3, P4 | Real or blank | Pressure at the remaining corners of the face. Default = P1 (uniform load) |
| G1 | Integer > 0 or blank | Grid point at a corner of the **loaded face** (required for solid elements; blank/omitted for shell elements) |
| G3 or G4 | Integer > 0 or blank | For CHEXA/CPENTA/CPYRAM: grid point diagonally opposite G1 on the face. Omit for CTETRA. |
| CID | Integer ≥ 0 | Coordinate system for the direction vector N. Default = 0 (basic) |
| N1, N2, N3 | Real | Components of direction vector in coordinate system CID. Defines direction only (not magnitude). If all blank, load is normal to the face |
| SORL | Character | "SURF" = surface pressure acting on the element face (Default). "LINE" = consistent edge loads (CQUADR/CTRIAR only) |
| LDIR | Character | Direction of line load when SORL = "LINE": "X", "Y", "Z", "TANG" (tangential), or "NORM" (Default = "NORM") |

## Applicable Elements

| Element type | Notes |
|---|---|
| CTRIA3, CTRIA6, CTRIAR | Shell triangles |
| CQUAD4, CQUAD8, CQUADR | Shell quadrilaterals |
| CHEXA | Solid hexahedron |
| CPENTA | Solid wedge |
| CPYRAM | Solid pyramid |
| CTETRA | Solid tetrahedron (G4 identifies excluded corner; not on loaded face) |

## Key Remarks

- Selected by Case Control command `LOAD = SID` (directly or via a LOAD combination entry).
- **Uniform pressure:** specify only P1; leave P2–P4 blank. The pressure is uniform across the entire face.
- **Non-uniform pressure:** specify P1–P4 at the four corners. Values are bilinearly interpolated; for triangular faces P4 has no meaning.
- **Shell elements:** pressure acts in the direction of the element's positive normal (right-hand rule on G1–G2–G3–G4 connection sequence). Positive pressure is in the positive normal direction.
- **Solid elements:** positive pressure (defaulted continuation) acts **inward** on the face. G1 identifies which face is loaded; G3/G4 provides the diagonally opposite corner to uniquely identify the face.
- **Direction vector (N1, N2, N3):** if specified, the load acts in the given direction regardless of the face normal. Useful for applying loads in a fixed global direction rather than following the face orientation (e.g. hydrostatic loads).
- **CTETRA:** G1 is a corner on the loaded face; G4 is the corner **not** on the loaded face (since CTETRA has only four corner points, G4 uniquely identifies the face).
- Follower force effects (pressure that tracks large deformations) are included in linear solutions with differential stiffness (SOLs 103, 105, 107–112, 115, 116) and in nonlinear solutions when PARAM,LGDISP,1 is set.

## Example

```
$       SID     EID     P1      P2      P3      P4      G1      G3/G4
PLOAD4  2       1106    10.0    8.0     5.0             48
$       CID     N1      N2      N3      SORL    LDIR
+       6       0.0     1.0     0.0
```

Non-uniform pressure on element 1106, with direction vector (0,1,0) in coordinate system 6.
G1=48 identifies the loaded face (solid element). P1=10, P2=8, P3=5 at three corners; P4 defaults to P1=10.
