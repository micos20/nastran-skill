# EIGRL — Real Eigenvalue Extraction Parameters (Lanczos)

Defines parameters for real eigenvalue analysis using the Lanczos method. Preferred over EIGR for most normal modes analyses. Selected by Case Control `METHOD = SID`.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| EIGRL | SID | V1 | V2 | ND | MSGLVL | MAXSET | SHFSCL | NORM | |
| | option_1=value_1 | option_2=value_2 | -etc.- | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Unique set identification number |
| V1, V2 | Real | Lower and upper bounds of eigenvalue range. For structural analysis: frequency in Hz (when using FMETHOD in Case Control) or eigenvalue in rad²/s². Blank = no bound |
| ND | Integer | Number of desired eigenvectors. Default = all roots in the range (minimum 1) |
| MSGLVL | Integer (0–4) | Diagnostic output level. 0 = minimal (Default). Higher values produce more solver output |
| MAXSET | Integer | Maximum block (Lanczos set) size. Default chosen automatically based on problem size |
| SHFSCL | Real | Estimate of the first flexible (non-zero) eigenvalue. Improves convergence when provided; optional |
| NORM | Character | Eigenvector normalisation. "MASS" = normalise to unit generalized mass (Default). "MAX" = normalise to unit maximum displacement |

## Continuation Line (optional advanced options)

Additional solver options are specified as key=value pairs on continuation lines. Each pair is written as a single 8-character token with no embedded spaces (e.g., `ALPH=0.0`). Supported option keywords include:

| Option | Description |
|---|---|
| `ALPH` | Frequency shift for ill-conditioned mass matrices |
| `NUMS` | Number of starting vectors |
| `IRES` | Restart flag |
| `MBLKSZS` | Minimum block size |
| `NBLKSZ` | Nominal block size |
| `NCVFACi` | Convergence factor override |
| `OPTi` | Solver option flags |
| `SHFi` | Additional shift values |

## Key Remarks

- Selected by Case Control command `METHOD = SID`.
- EIGRL always takes precedence over an EIGR entry with the same SID.
- The Lanczos method is the recommended solver for structural normal modes (SOL 103, SOL 111, etc.).
- If neither V1 nor V2 is specified, all eigenvalues found by the Lanczos run are returned up to ND.

## Example

```
$       SID     V1      V2      ND      MSGLVL  MAXSET  SHFSCL  NORM
EIGRL   100     0.0     1000.0  10      0
```
