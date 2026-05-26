# ITER — Iterative Solver Control

Defines parameters for the iterative (Krylov subspace) equation solver. Selected by Case Control `SMETHOD = SID`.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| ITER | SID | | | | | | | | |
| | OPTION1=VALUE1 | OPTION2=VALUE2 | -etc.- | | | | | | |

Fields 3–9 on the first line are blank. All options are specified on continuation lines.

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Unique set identification number. Selected by Case Control `SMETHOD = SID` |

## Continuation Line Options

Each option is written as a single 8-character token in the form `KEYWORD=VALUE` with **no embedded spaces**. Multiple options may appear on the same continuation line (one per field).

| Option | Default | Description |
|---|---|---|
| `PRECOND` | BIC (real) / BICCMPLX (complex) | Preconditioner type. Options: J, JS, C, CS, RIC, RICS, BIC, BICCMPLX, CASI, USER |
| `CONV` | AREX | Convergence criterion. Options: AR (absolute residual), GE (generalised error), AREX (absolute residual + extra), GEEX (generalised error + extra) |
| `MSGFLG` | NO | Diagnostic message output. YES or NO |
| `ITSEPS` | 1.0E-4 (CASI), 1.0E-8 (CASI+contact), 1.0E-6 (others) | Convergence tolerance (Real > 0) |
| `ITSMAX` | N/4 (N = matrix order) | Maximum number of solver iterations (Integer > 0) |
| `IPAD` | — | Padding parameter for RIC, RICS, BIC, BICCMPLX preconditioners (Integer ≥ 0) |
| `IEXT` | 0 | Block structuring level (Integer 0–7). Higher values may improve CASI performance |
| `PREFONLY` | 0 | 0 = run the analysis to completion; -1 = terminate after the preface phase |

## Key Remarks

- Selected by Case Control command `SMETHOD = SID`.
- Valid solution sequences: SOL 101, 106, 108, 111, 153, 200, 400.
- The ITER entry uses a **unique key=value field syntax**: each option keyword and its value are concatenated without spaces into a single 8-character field (e.g., `ITSMAX=50`). This is different from standard positional Bulk Data fields.
- The first line contains only `ITER` and `SID`; all options are on continuation lines.
- BIC (Blocked Incomplete Cholesky) is the recommended preconditioner for most symmetric real problems.
- CASI (Component Adaptive Subspace Iteration) is available for large eigenvalue-like problems.

## Example

```
$       SID
ITER    10
$       PRECOND CONV    MSGFLG  ITSEPS  ITSMAX
+       PRECOND=BIC     CONV=AREX       ITSMAX=100
```
