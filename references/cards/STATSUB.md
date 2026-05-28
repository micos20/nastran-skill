# STATSUB (Case) — Static Solution Selection for Differential Stiffness

Selects the static solution to use in forming the differential stiffness for buckling analysis, normal modes, complex eigenvalue, frequency response, and transient response analysis.

## Format

```
STATSUB [(BUCKLING | PRELOAD)] = n
```

## Examples

```
STATSUB = 23
STAT = 4
STATSUB(PREL) = 7
```

(Note: `STAT` is an acceptable 4-character abbreviation per Case Control free-format rules.)

## Describers

| Describer | Meaning |
|---|---|
| BUCKLING | Subcase ID corresponding to the static subcase with the varying/buckling load. **Default in buckling analysis.** |
| PRELOAD | Subcase ID corresponding to the static subcase with the constant preload. **Default in dynamic analysis.** |
| n | Subcase identification number of a prior static analysis subcase. (Integer > 0) |

## Key Remarks

- Applicable solution sequences: SOLs 101, 103, 105, 107–112, 115, 116, 200, 400. The BUCKLING option is only valid in SOL 200 and SOL 400 (ANALYSIS = BUCKLING).
- STATSUB must be specified in the **same subcase** that contains:
  - `METHOD` — for buckling or normal modes
  - `CMETHOD` — for complex eigenvalue analysis
  - `TSTEP` — for transient response
  - `FREQ` — for frequency response
- In SOL 105, the default is the first static subcase ID; STATSUB is not required unless a different static subcase is intended.
- For buckling with preload: both `STATSUB(BUCKLING)` and `STATSUB(PRELOAD)` must be present in the buckling subcase. This combination is **not supported** in SOL 200 or SOL 400.
- In dynamic analysis, only one STATSUB command may appear per dynamic subcase.
- In SOLs 108/111, a subcase without a `FREQUENCY` command is treated as a static subcase. In SOLs 109/112, a subcase without `TSTEP` is treated as static.

## Typical Usage (Buckling, SOL 105)

```
SUBCASE 1             $ Static analysis — applied load
  LOAD = 10
  SPC = 1

SUBCASE 2             $ Buckling analysis
  METHOD = 50         $ Eigenvalue extraction
  STATSUB = 1         $ Differential stiffness from SUBCASE 1
  SPC = 1
```
