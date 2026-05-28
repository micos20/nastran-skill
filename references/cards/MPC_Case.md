# MPC (Case) — Multipoint Constraint Set Selection

Selects the multipoint constraint set to be applied in the analysis.

## Format

```
MPC = n
```

## Example

```
MPC = 10
```

## Describers

| Describer | Meaning |
|---|---|
| n | Set identification number of MPC or MPCADD Bulk Data entries. (Integer > 0) |

## Key Remarks

- In cyclic symmetry analysis, the MPC command must appear above the first SUBCASE.
- In superelement analysis, multiple MPC sets are not allowed; the second and subsequent sets are ignored with a warning.
- MPC = n can also select rigid elements defined via `SET3,n` entries where the DES field is `RBEin` or `RBEex`. In this case, `RIGID = LINEAR` must also be present in the Case Control Section to support rigid element set selection in SOL 400.
- MPC force recovery is available via the `MPCFORCE` Case Control command (except SOL 129).

## Typical Usage

```
MPC = 5
SPC = 1

SUBCASE 1
  LOAD = 10
  DISP = ALL
  MPCFORCE = ALL
```

## See Also

- MPC Bulk Data entry — defines a linear multipoint constraint equation
- MPCADD Bulk Data entry — combines multiple MPC sets
- SET3 Bulk Data entry — used to include/exclude rigid elements via RBEin/RBEex
