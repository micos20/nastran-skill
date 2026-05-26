# TEMP — Grid Point Temperature Field

Defines temperature at grid points for determination of thermal loading, temperature-dependent material properties, or stress recovery.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| TEMP | SID | G1 | T1 | G2 | T2 | G3 | T3 | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Temperature set identification number. Selected by Case Control `TEMP = SID` |
| Gi | Integer > 0 | Grid point identification number |
| Ti | Real | Temperature at grid point Gi |

## Key Remarks

- One to three grid point temperatures may be defined on a single entry. Use additional entries with the same SID to assign temperatures to more grid points.
- Selected by Case Control `TEMP = SID` (static sequences) or `TEMP(INIT) = SID` (nonlinear heat transfer initialization).
- In dynamic solution sequences, SID is referenced via the EXCITEID field of an RLOADi or TLOADi entry, or via LOADSET/LSEQ.
- SID must be unique with respect to all other LOAD type entries if `TEMP(LOAD)` is specified in Case Control.
- **Element temperature averaging:** when no element-level temperature entry (TEMPP1, TEMPRB, TEMPB3, etc.) is present for an element, the element temperature is computed as the simple average of the temperatures at its connected grid points. Directly defined element temperatures always take precedence over grid-point averages.
- **Temperature-dependent materials:** material properties (E, G, α, etc.) are evaluated at the average element temperature.
- For nonlinear heat transfer initialization (SOLs 153, 159), used together with TEMPD and TEMPN1 entries.

## Example

```
$       SID     G1      T1      G2      T2      G3      T3
TEMP    3       94      316.2   49      219.8
```

Two grid point temperatures in set 3: grid 94 at 316.2, grid 49 at 219.8.
