# SPC (Case) — Single-Point Constraint Set Selection

Selects the single-point constraint set to be applied in the analysis.

## Format

```
SPC = n
```

## Example

```
SPC = 100
```

## Describers

| Describer | Meaning |
|---|---|
| n | Set identification number of SPC, SPC1, SPC2 (SOL 700), FRFSPC1, or SPCADD Bulk Data entries. (Integer > 0) |

## Key Remarks

- In cyclic symmetry analysis, the SPC command must appear above the first SUBCASE.
- Multiple boundary conditions are supported only in SOLs 101, 103, 105, 145, 200. Use the `BC` command for the residual structure when superelements are present.
- In thermal analysis, SPC specifies a constant temperature boundary condition (not a mechanical constraint).
- The total applied load vector is: {P} = external LOAD + TEMP(LOAD) + DEFORM + SPC enforced displacement.

## Typical Usage (SOL 101)

```
SPC = 1

SUBCASE 1
  LOAD = 10
  DISP = ALL
```

## See Also

- SPC Bulk Data entry — defines constrained DOFs and enforced displacements
- SPC1 Bulk Data entry — alternate form for constraining a list of grids
- SPCADD Bulk Data entry — combines multiple SPC/SPC1 sets
