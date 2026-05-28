# NLBUCK (Case) — Nonlinear Buckling Analysis Request

Requests nonlinear buckling load prediction within a SOL 400 step by projecting eigenvalues from the current tangent stiffness.

## Format

```
NLBUCK [= END | ALL | r]
```

## Examples

```
NLBUCK
NLBUCK = END
NLBUCK = ALL
NLBUCK = 5.0
```

## Describers

| Describer | Meaning |
|---|---|
| END | Project eigenvalues at the **end** of the step only. **Default.** |
| ALL | Project eigenvalues after **each converged load increment** within the step. |
| r | Project eigenvalues every **r load steps** (Real). Also projects at the final increment. Tolerance: 1.0E-6. |

## Key Remarks

- **SOL 400 only.** Not supported in NLPERF SOL 400.
- NLBUCK may be placed in any STEP or SUBCASE that also contains an `NLSTEP` command.
- **`NLPARM` is NOT allowed** in the same step or subcase as NLBUCK. Use `NLSTEP` instead.
- A `METHOD` command (referencing EIGB, EIGR, or EIGRL) may also appear in the same STEP to specify the eigenvalue extraction method. For asymmetric follower stiffness effects, use `CMETHOD` with an EIGC entry instead.
- For best accuracy: set `NO = 1` on the NLSTEP Bulk Data entry, and set `PARAM,LGDISP,1` (large displacement effects).
- The buckling load factor λ is applied to the current applied load to estimate the critical load.

## Typical Usage (SOL 400 — Nonlinear Buckling)

```
SUBCASE 1
  STEP 1
    ANALYSIS = NLSTATICS
    LOAD = 10
    NLSTEP = 100
    SPC = 1
  STEP 2
    ANALYSIS = NLSTATICS
    LOAD = 20
    NLSTEP = 200
    NLBUCK = END
    METHOD = 50
```

## See Also

- NLSTEP Case Control command — required in the same step; NLPARM not allowed with NLBUCK
- METHOD Case Control command — specifies eigenvalue extraction method (EIGR/EIGRL/EIGB)
- EIGB Bulk Data entry — buckling eigenvalue extraction parameters
- STEP Case Control command — SOL 400 step delimiter
