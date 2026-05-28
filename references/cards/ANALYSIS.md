# ANALYSIS (Case) — Analysis Type Selection

Specifies the type of analysis to be performed for a SUBCASE, STEP, or SUBSTEP.

## Format

```
ANALYSIS = type
```

## Examples

```
ANALYSIS = STATICS
ANALYSIS = NLSTATICS
ANALYSIS = MODES
ANALYSIS = BUCK
ANALYSIS = NLTRAN
```

## Describers

| Describer | Meaning |
|---|---|
| type | Character keyword identifying the analysis discipline. See the Analysis Types table below. |

## Analysis Types

| type | Analysis | Applicable SOLs |
|---|---|---|
| STATICS | Linear Static | SOLs 200, 400 |
| MODES | Normal Modes | SOLs 103, 106, 110, 111, 112, 200, 400 |
| BUCK | Buckling | SOLs 105, 200, 400 |
| DFREQ | Direct Frequency Response | SOLs 108, 200, 400 |
| MFREQ | Modal Frequency Response | SOLs 111, 200, 400 |
| MTRAN | Modal Transient Response | SOLs 112, 200, 400 |
| DCEIG | Direct Complex Eigenvalue | SOLs 107, 200, 400 |
| MCEIG | Modal Complex Eigenvalue | SOLs 110, 200, 400 |
| SAERO | Static Aeroelasticity | SOLs 144, 200, 400 |
| DIVERGE | Static Aeroelastic Divergence | SOLs 144, 200, 400 |
| FLUTTER | Flutter | SOLs 145, 200, 400 |
| DAERO | Dynamic Aeroelasticity | SOL 200 |
| HEAT | Heat Transfer Analysis | SOLs 153, 159 |
| STRUCTURE | Structural Analysis | SOLs 153, 159 (default for these two) |
| HSTAT | Steady-State Heat Transfer | SOL 400 |
| HTRAN | Transient Heat Transfer | SOL 400 |
| NLSTATICS | Nonlinear Static Analysis | SOL 400 |
| NLTRAN | Nonlinear Transient Analysis | SOL 400 |
| HOT2COLD | Hot-to-Cold Analysis | SOL 106 only |

## Key Remarks

- For **linear SOLs** (101, 103, 105, 107–112, 145, 146) and nonlinear SOLs (106, 153, 159, 200): one ANALYSIS command per SUBCASE. May appear above all SUBCASEs to apply to all.
- For **SOL 200**: EVERY SUBCASE must have an ANALYSIS command (or place one above all subcases as a default for all).
- For **SOL 400 single physics**: one ANALYSIS per STEP.
- For **SOL 400 coupled multi-physics**: one ANALYSIS per SUBSTEP (each SUBSTEP within a STEP has its own discipline).
- SOL 400 STEP solutions **chain**: end conditions of STEP n become initial conditions of STEP n+1 within the same SUBCASE.
- **Linear perturbation steps** (ANALYSIS = BUCK, MODES, DFREQ, etc.) must follow a converged nonlinear static step in SOL 400.

## Typical Usage (SOL 400 — Nonlinear Statics then Buckling)

```
SUBCASE 1
  STEP 1
    ANALYSIS = NLSTATICS
    LOAD = 10
    NLSTEP = 100
    SPC = 1
  STEP 2
    ANALYSIS = BUCK
    METHOD = 50
    NLBUCK = END
```

## Typical Usage (SOL 200 — Design Optimization)

```
SUBCASE 1
  ANALYSIS = STATICS
  LOAD = 10
  SPC = 1

SUBCASE 2
  ANALYSIS = MODES
  METHOD = 100
  SPC = 1
```

## See Also

- STEP Case Control command — SOL 400 step delimiter; each step carries its own ANALYSIS
- SUBCASE Case Control command — subcase delimiter
- NLBUCK Case Control command — nonlinear buckling request (used with ANALYSIS = BUCK in SOL 400)
- METHOD Case Control command — specifies eigenvalue extraction for MODES, BUCK, etc.
