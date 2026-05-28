# NLOPRM (Case) — Nonlinear Analysis Output and Debug Options

Controls output timing and debug diagnostics for SOL 400 nonlinear analyses using keyword=value pairs.

## Format

```
NLOPRM [OUTCTRL = {STD | SOLUTION | INTERM}]
       [NLDBG = {NONE | NLBASIC | NRDBG | ADVDBG | N3DBASE | N3DMED | N3DADV | N3DSUM}]
       [DBGPOST = {NONE | LTIME | LSTEP | LSUBC | ALL}]
       [MPCPCH = {NONE | BEGN | OTIME | STEP | YBEGN | YOTIME | YSTEP}]
       [DELIMIT = {No | Yes}]
       [GRIDINF = {No | MAXGRID | GID}]
```

Each keyword=value pair is written as a single token (no embedded spaces). Multiple options may appear on the same NLOPRM line separated by spaces.

## Examples

```
NLOPRM OUTCTRL=STD
NLOPRM OUTCTRL=SOLUTION NLDBG=NLBASIC DBGPOST=LSTEP
NLOPRM OUTCTRL=STD DELIMIT=No GRIDINF=No
```

## Describers

| Keyword | Values | Meaning |
|---|---|---|
| OUTCTRL | STD (default) | Standard post-processing output at end of run |
| | SOLUTION | Real-time output during the run at each converged solution |
| | INTERM | Output at each iteration |
| NLDBG | NONE (default) | No nonlinear debug output |
| | NLBASIC | Basic nonlinear diagnostics |
| | NRDBG | Newton-Raphson iteration debug |
| | ADVDBG | Advanced debug output |
| | N3DBASE / N3DMED / N3DADV / N3DSUM | 3D contact debug at base / medium / advanced / summary level |
| DBGPOST | NONE (default) | No debug post-processing output |
| | LTIME | Output at each load time |
| | LSTEP | Output at each load step |
| | LSUBC | Output at each load subcase |
| | ALL | Output at all levels |
| MPCPCH | NONE (default) | No MPC punch output |
| | BEGN | Punch at beginning only |
| | OTIME / STEP / YBEGN / YOTIME / YSTEP | Punch at output time / step / year-based intervals |
| DELIMIT | No (default) | No output delimiter |
| | Yes | Include delimiter in output |
| GRIDINF | No (default) | No grid information output |
| | MAXGRID | Output maximum grid information |
| | GID | Output grid ID information |

## Key Remarks

- **SOL 400 only.**
- NLOPRM uses a unique **keyword=value syntax** — each option must be a single token with no embedded spaces (e.g., `OUTCTRL=SOLUTION` not `OUTCTRL = SOLUTION`).
- Multiple keyword=value pairs may appear on the same NLOPRM line, separated by spaces.
- NLOPRM may appear at any level in the Case Control Section (above subcases, within a SUBCASE, or within a STEP).
- `OUTCTRL=SOLUTION` is useful for monitoring convergence during long runs; it writes results incrementally to the output file as each increment converges.

## Typical Usage

```
NLOPRM OUTCTRL=SOLUTION NLDBG=NLBASIC

SUBCASE 1
  STEP 1
    ANALYSIS = NLSTATICS
    LOAD = 10
    NLSTEP = 100
    SPC = 1
```

## See Also

- STEP Case Control command — step delimiter for SOL 400
- NLSTEP Case Control command — selects nonlinear step control parameters
- ANALYSIS Case Control command — specifies analysis type per STEP
