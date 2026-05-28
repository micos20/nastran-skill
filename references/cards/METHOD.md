# METHOD (Case) — Eigenvalue Extraction Method Selection

Selects the real eigenvalue extraction method for normal modes, buckling, and related analyses.

## Format

```
METHOD [(BOTH | STRUCTURE | FLUID | COUPLED | SYMCOUP)] = n
```

## Examples

```
METHOD = 50
METHOD(STRUCTURE) = 100
METHOD(COUPLED) = 200
```

## Describers

| Describer | Meaning |
|---|---|
| BOTH | Method applies to both structural and fluid modes. **Default.** |
| STRUCTURE | Method applies to structural modes only. |
| FLUID | Method applies to fluid modes only. |
| COUPLED | Subspace iteration method for coupled fluid-structure eigenvalue problems with heavy fluid loading. |
| SYMCOUP | Lanczos method for symmetric coupled fluid-structure eigenvalue problems. |
| n | Set identification number of an EIGR, EIGRL, or EIGB Bulk Data entry. (Integer > 0) |

## Key Remarks

- **COUPLED** and **SYMCOUP** are applicable in SOLs 101, 103, 111, 112, and SOL 200/400 (ANALYSIS = LINEAR).
- If SID `n` exists on both an **EIGRL** and an EIGR and/or EIGB entry, **EIGRL takes precedence**.
- EIGB entries are intended specifically for **buckling** eigenvalue extraction.
- For normal modes: METHOD references EIGR or EIGRL.
- For buckling (SOL 105 or SOL 400 STEP with ANALYSIS=BUCK): METHOD references EIGB, EIGR, or EIGRL.
- For nonlinear buckling in SOL 400: METHOD may be used together with the NLBUCK command.

## Typical Usage (SOL 103 — Normal Modes)

```
METHOD = 100

SUBCASE 1
  DISP = ALL
  STRESS = ALL
```

## Typical Usage (SOL 105 — Buckling)

```
SUBCASE 1             $ Static analysis
  LOAD = 10
  SPC = 1

SUBCASE 2             $ Buckling analysis
  METHOD = 50
  STATSUB = 1
  SPC = 1
```

## See Also

- EIGR Bulk Data entry — real eigenvalue extraction parameters (general methods)
- EIGRL Bulk Data entry — Lanczos eigenvalue extraction parameters (preferred)
- EIGB Bulk Data entry — buckling eigenvalue extraction parameters
- NLBUCK Case Control command — nonlinear buckling in SOL 400 (also uses METHOD)
- STATSUB Case Control command — selects static subcase for differential stiffness
