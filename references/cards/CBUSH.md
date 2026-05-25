# CBUSH — Generalized Spring-and-Damper Connection

Defines a generalized spring-and-damper structural element that may be nonlinear or frequency dependent.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| CBUSH | EID | PID | GA | GB | GO/X1 | X2 | X3 | CID | |
| | S | OCID | S1 | S2 | S3 | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| EID | 0 < Integer < 100,000,000 | Unique element identification number |
| PID | Integer > 0; Default = EID | Property ID referencing a PBUSH entry |
| GA | Integer > 0 | Grid point at end A |
| GB | Integer ≥ 0 or blank | Grid point at end B. Blank = grounded terminal (displacement constrained to zero) |
| Xi | Real | Components of orientation vector v̂ from GA in displacement CS at GA |
| GO | Integer > 0 | Alternate: grid point for orientation vector, direction from GA to GO |
| CID | Integer ≥ 0 or blank | Element coordinate system ID. Overrides GO/Xi when specified. Blank → derived from GO or Xi |
| S | Real; Default = 0.5 | Location of spring-damper along GA–GB line (0 = GA, 1 = GB) |
| OCID | Integer ≥ −1; Default = −1 | Coordinate system for offset S1,S2,S3. −1 = use S (midpoint offset) |
| S1, S2, S3 | Real | Spring-damper offset in OCID system (used when OCID ≥ 0) |

## Key Remarks

- GB blank → grounded terminal (single-point spring to ground)
- If GA ≠ GB, GO/Xi not given, and CID not specified: element x-axis = line GA→GB. Valid only when K1/B1 or K4/B4 are the only nonzero stiffness/damping terms on PBUSH
- CID ≥ 0 overrides GO and Xi: element axes align with CID T1, T2, T3 directions
- OCID = −1: S used, S1/S2/S3 ignored. OCID ≥ 0: S ignored, S1/S2/S3 used
- In SOL 400 (geometric nonlinear): element axis GA→GB follows deformation
- CBUSH elements are not supported in thermal analysis
- PID may reference a PBUSHT entry for frequency-dependent properties (SOL 108/111)
