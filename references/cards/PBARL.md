# PBARL — Simple Beam Cross-Section Property

Defines the properties of a simple beam element (CBAR entry) by cross-sectional dimensions rather than explicit section properties. MSC Nastran internally computes the equivalent PBAR section properties from the dimensions.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PBARL | PID | MID | GROUP | TYPE | | | | | |
| | DIM1 | DIM2 | DIM3 | DIM4 | DIM5 | DIM6 | DIM7 | DIM8 | |
| | DIM9 | -etc.- | NSM | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property identification number |
| MID | Integer > 0 | Material ID — references MAT1 for structural analysis |
| GROUP | Character; Default = "MSCBML0" | Cross-section group. Use "MSCBML0" for all standard library types. |
| TYPE | Character | Cross-section type (see table below). Required; no default. |
| DIMi | Real > 0.0 | Cross-sectional dimensions. Number of DIMi values depends on TYPE. |
| NSM | Real; Default = 0.0 | Nonstructural mass per unit length. Specified after the last DIMi on the continuation line. |

## Cross-Section Types and DIM Count (GROUP = "MSCBML0")

| TYPE | # DIMs | Description |
|---|---|---|
| "ROD" | 1 | Solid circular rod — DIM1 = radius |
| "TUBE" | 2 | Hollow circular tube — DIM1 = outer radius, DIM2 = inner radius |
| "TUBE2" | 2 | Hollow tube variant — DIM1 = outer radius, DIM2 = wall thickness |
| "BAR" | 2 | Solid rectangular bar — DIM1 = width, DIM2 = height |
| "I" | 6 | Symmetric I-section — DIM1=height, DIM2=bottom flange width, DIM3=top flange width, DIM4=bottom flange thickness, DIM5=top flange thickness, DIM6=web thickness |
| "CHAN" | 4 | Channel (C-section) — DIM1=height, DIM2=flange width, DIM3=web thickness, DIM4=flange thickness |
| "T" | 4 | T-section — DIM1=flange width, DIM2=height, DIM3=flange thickness, DIM4=web thickness |
| "BOX" | 4 | Closed box — DIM1=width, DIM2=height, DIM3=wall thickness (sides), DIM4=wall thickness (flanges) |
| "CROSS" | 4 | Cross (+) section — DIM1=half-width of horizontal arm, DIM2=half-width of vertical arm, DIM3=height of horizontal arm, DIM4=height of vertical arm |
| "H" | 4 | H-section (double channel) |
| "T1" | 4 | T-section, alternate form |
| "I1" | 4 | I-section, alternate form |
| "CHAN1" | 4 | Channel, alternate form |
| "CHAN2" | 4 | Channel, second alternate form |
| "Z" | 4 | Z-section |
| "T2" | 4 | T-section, second alternate form |
| "BOX1" | 6 | Box with variable wall thicknesses |
| "HEXA" | 3 | Regular hexagonal section |
| "HAT" | 4 | Hat section |
| "HAT1" | 5 | Hat section, alternate form |
| "DBOX" | 10 | Double-box section (DIM5–DIM8 default to DIM4; DIM9–DIM10 default to DIM6 if not provided) |

See QRG Figures 9-106 through 9-109 for the exact dimension geometry and stress recovery point locations for each TYPE.

## Key Remarks

- PBARL references MAT1 for structural analysis; MAT4 or MAT5 for heat transfer.
- Primary property entry — PID must be unique with respect to all PBAR, PBARL, and PBRSECT entries.
- The sorted echo will print the internally derived PBAR entry, not the PBARL.
- PBEAML is recommended over PBARL for asymmetric cross-sections (CHAN, Z, T, CHAN1, CHAN2, T1, T2, I1, BOX1) because PBARL computes section properties relative to the neutral axis without accounting for shear center offsets, leading to approximations.
- For response spectra analysis with stress recovery, use the CBEAM/PBEAML instead of CBAR/PBARL — CBAR stress recovery results may be inaccurate.
- For DBOX TYPE: DIM5–DIM8 default to DIM4 and DIM9–DIM10 default to DIM6 if not provided. These defaults do not apply during design optimization property updates.
