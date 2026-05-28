# NLSTEP (Case) — Nonlinear Step Control Selection

Selects the NLSTEP Bulk Data entry that controls the nonlinear incremental procedure for SOL 400.

## Format

```
NLSTEP = n
```

## Example

```
NLSTEP = 100
```

## Describers

| Describer | Meaning |
|---|---|
| n | Identification number of a NLSTEP Bulk Data entry. (Integer > 0) |

## Key Remarks

- Valid solution sequences: **SOL 400** and **SOL 101** (linear contact analysis only).
- If NLSTEP is specified **anywhere within a SUBCASE**, all `NLPARM` commands in that SUBCASE are **ignored**. NLSTEP and NLPARM are mutually exclusive within a SUBCASE.
- For **coupled multi-physics analysis**: a single NLSTEP command must appear **above the first SUBSTEP** command within the STEP.
- NLSTEP supersedes NLPARM as the preferred nonlinear step control entry for SOL 400.

## Typical Usage (SOL 400)

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
    NLSTEP = 100
```

## See Also

- NLSTEP Bulk Data entry — defines nonlinear step parameters (increments, convergence criteria, time steps)
- NLPARM Bulk Data entry — older nonlinear control entry (superseded by NLSTEP in SOL 400)
- STEP Case Control command — step delimiter for SOL 400
- ANALYSIS Case Control command — specifies analysis type per STEP
