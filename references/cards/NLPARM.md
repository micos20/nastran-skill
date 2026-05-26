# NLPARM — Nonlinear Static Analysis Parameters

Defines iteration and convergence parameters for nonlinear static analysis (SOL 106 and SOL 400). Selected by Case Control `NLPARM = ID` in each nonlinear subcase.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| NLPARM | ID | NINC | DT | KMETHOD | KSTEP | MAXITER | CONV | INTOUT | |
| | EPSU | EPSP | EPSW | MAXDIV | MAXQN | MAXLS | FSTRESS | LSTOL | |
| | MAXBIS | | | | MAXR | | RTOLB | MINITER | |

## Fields

| Name | Type | Description |
|---|---|---|
| ID | Integer > 0 | Unique identification number. Referenced by Case Control `NLPARM = ID` |
| NINC | Integer > 0 | Number of load increments per subcase. Default = 10 |
| DT | Real | Time increment for creep analysis. Default = 0.0 (no creep) |
| KMETHOD | Character | Stiffness matrix update strategy. Default = "AUTO" (SOL 106, SOL 400 without contact); "FNT" (SOL 400 with contact). Options: AUTO, ITER, SEMI, FNT, PFNT |
| KSTEP | Integer | Number of iterations between stiffness matrix updates (KMETHOD = ITER or SEMI). Default = 5 (SOL 106), 10 (SOL 400) |
| MAXITER | Integer | Maximum number of iterations per increment. Default = 25 |
| CONV | Character | Convergence criterion flags — any combination of: "U" (displacement), "P" (load), "W" (work), "V" (vector), "N" (no test), "A" (auto-select). Default = "PW" |
| INTOUT | Character or Integer | Intermediate output control. "YES" = output at each increment, "NO" = output only at subcase end (Default), "ALL" = output at each iteration. Integer > 0 (SOL 400) = output every N increments |
| EPSU | Real > 0.0 | Displacement convergence tolerance (CONV includes "U"). Default = 1.0E-2 |
| EPSP | Real > 0.0 | Load convergence tolerance (CONV includes "P"). Default = 1.0E-2 |
| EPSW | Real > 0.0 | Work/energy convergence tolerance (CONV includes "W"). Default = 1.0E-2 |
| MAXDIV | Integer | Maximum number of diverging iterations before bisection is attempted. Default = 3 |
| MAXQN | Integer | Maximum number of quasi-Newton (BFGS) update vectors. Default = MAXITER |
| MAXLS | Integer | Maximum number of line-search operations per iteration. Default = 4 |
| FSTRESS | Real (0.0–1.0) | Fraction of yield stress used as convergence criterion for plasticity. Default = 0.2 |
| LSTOL | Real (0.01–1.0) | Tolerance for line-search convergence. Default = 0.5 |
| MAXBIS | Integer | Maximum number of bisections per increment. Positive = adaptive bisection (Default = 5); negative = fixed bisection at \|MAXBIS\| levels |
| MAXR | Real ≥ 1.0 | Maximum ratio for arc-length increment adjustment. Default = 20.0 |
| RTOLB | Real ≥ 1.0 | Maximum ratio of arc-length adjustment allowed between increments. Default = 20.0 |
| MINITER | Integer | Minimum number of iterations per increment. Default = 1; Default = 2 when contact is present |

## Key Remarks

- Every nonlinear subcase must include `NLPARM = ID` in the Case Control section.
- KMETHOD = "AUTO": stiffness updated at beginning of each increment and after divergence. Recommended for most analyses.
- KMETHOD = "FNT" (Full Newton): stiffness updated at every iteration. Default for SOL 400 with contact; more robust but more expensive.
- KMETHOD = "PFNT" (Pure Full Newton): like FNT but without quasi-Newton updates.
- CONV = "PW" (default) checks both load and energy; suitable for most structural problems. Use "U" to add displacement checking for displacement-controlled cases.
- INTOUT = "YES" writes results at every converged increment — useful for tracing load-displacement paths but increases output file size.

## Example

```
$       ID      NINC    DT      KMETHOD KSTEP   MAXITER CONV    INTOUT
NLPARM  1       10              AUTO            25      PW      NO
$       EPSU    EPSP    EPSW    MAXDIV  MAXQN   MAXLS   FSTRESS LSTOL
+       1.0E-3  1.0E-3  1.0E-7  3       25      4       0.2     0.5
```
