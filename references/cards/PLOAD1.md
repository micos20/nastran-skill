# PLOAD1 — Applied Load on CBAR, CBEAM or CBEND Element

Defines concentrated, uniformly distributed, or linearly distributed loads applied to CBAR or CBEAM elements at user-chosen positions along the element axis. For CBEND elements, only distributed loads over the entire length are allowed.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PLOAD1 | SID | EID | TYPE | SCALE | X1 | P1 | X2 | P2 | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| EID | Integer > 0 | CBAR, CBEAM, or CBEND element identification number |
| TYPE | Character | Load type. See table below |
| SCALE | Character | Interpretation of X1, X2 values. See table below |
| X1 | Real (0 ≤ X1) | Position along element axis from end A where load starts |
| P1 | Real or blank | Load magnitude at X1 |
| X2 | Real (X1 ≤ X2) or blank | Position where load ends. If blank or equal to X1, a concentrated load P1 is applied at X1 |
| P2 | Real or blank | Load magnitude at X2. If blank, P2 = P1 (uniform distribution) |

### TYPE values

| TYPE | Meaning |
|---|---|
| FX, FY, FZ | Force in x, y, z direction of the basic coordinate system |
| FXE, FYE, FZE | Force in x, y, z direction of the element coordinate system |
| MX, MY, MZ | Moment about x, y, z axis of the basic coordinate system |
| MXE, MYE, MZE | Moment about x, y, z axis of the element coordinate system |

### SCALE values

| SCALE | Meaning |
|---|---|
| LE | X1, X2 are actual distances along the element axis (length units) |
| FR | X1, X2 are fractional ratios of the total element length (0.0–1.0) |
| LEPR | X1, X2 are actual distances; load intensity specified per unit **projected** length |
| FRPR | X1, X2 are fractional ratios; load intensity per unit projected length |

## Key Remarks

- Selected by Case Control command `LOAD = SID` (directly or via a LOAD combination entry).
- **Concentrated load:** X2 blank or X2 = X1 → load P1 applied as a point load at position X1.
- **Uniformly distributed load:** X2 ≠ X1 and P1 = P2 → intensity P1 over the span X1–X2.
- **Linearly varying load:** X2 ≠ X1 and P1 ≠ P2 → load varies linearly from P1 at X1 to P2 at X2.
- For CBEND elements: only distributed loads over the entire element length may be applied; SCALE = LEPR or FRPR (projected) are not applicable.
- Loads on CBEAM elements are applied along the line of the shear center.
- Element coordinate system (E-suffix TYPE codes): x is along the element axis from GA to GB; y and z are defined by the element orientation vector.

## Example

```
$       SID     EID     TYPE    SCALE   X1      P1      X2      P2
PLOAD1  25      1065    MY      FRPR    0.2     2.5E3   0.8     3.5E3
```

Linearly varying moment My (in element coordinates) from 2500 at 20% of the element length to 3500 at 80%, applied as projected-fractional distributed load.
