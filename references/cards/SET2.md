# SET2 — Grid Point List

Defines a list of structural grid points in terms of aerodynamic macro elements. Used exclusively in aeroelastic and flutter analyses.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SET2 | SID | MACRO | SP1 | SP2 | CH1 | CH2 | ZMAX | ZMIN | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Unique set identification number |
| MACRO | Integer > 0 | Element identification number of an aerodynamic macro element (e.g. from a CAEROi entry) |
| SP1, SP2 | Real | Lower and higher span division points defining the prism containing the set |
| CH1, CH2 | Real | Lower and higher chord division points defining the prism containing the set |
| ZMAX, ZMIN | Real | Z-coordinates of the top and bottom of the prism (using right-hand rule with CAEROi corner ordering). A value of 0.0 implies infinity. Usually ZMAX ≥ 0.0 and ZMIN ≤ 0.0. |

## Key Remarks

- SET2 is referenced by SPLINEi entries (SPLINE1, SPLINE2, etc.) to identify the structural grid points within a spline region.
- All structural grid points located within the defined prism and within the ZMAX/ZMIN height range are automatically included in the set.
- Because points exactly on the prism boundary may be missed, use slightly extended ranges (e.g. SP1=−0.01, SP2=1.01) to ensure all boundary points are captured.
- To print the internal grid IDs found by the SET2 definition, use DIAG 18.
- SET2 is aerodynamics-specific. For general-purpose grid or element lists, use SET1.
