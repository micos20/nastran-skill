# STEP (Case) — Step Delimiter for SOL 400

Delimits and identifies a step within a SUBCASE in SOL 400 (NONLIN) analysis.

## Format

```
STEP n
```

(Alternate form `STEP = n` is also accepted.)

## Example

```
STEP 1
```

## Describers

| Describer | Meaning |
|---|---|
| n | Step identification number. (Integer, 0 < n < 9999999) |

## Key Remarks

- `STEP` is valid in **SOL 400 (NONLIN) only**.
- Step IDs within a SUBCASE must be in **strictly increasing order**.
- The solution of each STEP is a **continuation** of the previous STEP within the same SUBCASE — ending conditions (stress, displacement, contact state) become the initial conditions of the next STEP.
- If no SUBCASE is specified in the Case Control Section, Nastran automatically creates a default SUBCASE 1.
- For **coupled multi-physics analysis**, a STEP contains SUBSTEP commands that each carry an `ANALYSIS` type.
- Each STEP must have its own `NLSTEP` (or `NLPARM`) command to control the incremental procedure.
- A **linear perturbation step** (e.g., `ANALYSIS = BUCK`, `ANALYSIS = MODES`) must follow a converged nonlinear static step within the same SUBCASE.

## Typical Usage (SOL 400 — Chained Steps)

```
SUBCASE 1
  STEP 1
    ANALYSIS = NLSTATICS
    LOAD = 10
    NLSTEP = 100
    NLPARM = 200
  STEP 2
    ANALYSIS = NLSTATICS
    LOAD = 20
    NLSTEP = 100
```

## Typical Usage (SOL 400 — Nonlinear Statics Followed by Buckling)

```
SUBCASE 1
  STEP 1
    ANALYSIS = NLSTATICS
    LOAD = 10
    NLSTEP = 100
  STEP 2
    ANALYSIS = BUCK
    METHOD = 50
    NLBUCK
```

## See Also

- SUBCASE Case Control command — parent delimiter; step IDs must increase within a SUBCASE
- ANALYSIS Case Control command — specifies the analysis type per STEP
- NLSTEP Case Control command — selects step control parameters for SOL 400
- NLBUCK Case Control command — requests nonlinear buckling prediction within a STEP
