# EIGR — Real Eigenvalue Extraction Parameters

Defines parameters for real eigenvalue analysis (normal modes). Selected by Case Control `METHOD = SID`.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| EIGR | SID | METHOD | F1 | F2 | NE | ND | | | |
| | | NORM | G | C | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Unique set identification number |
| METHOD | Character | Eigenvalue extraction method. Recommended: "LAN" (Lanczos) or "AHOU". Obsolete (supported but not recommended): "INV", "SINV", "GIV", "MGIV", "HOU", "MHOU", "AGIV" |
| F1, F2 | Real | Lower and upper bounds of frequency range of interest (Hz). Blank = no bound |
| NE | Integer ≥ 0 | Estimated number of roots in the frequency range (used by Lanczos). Default = 0 |
| ND | Integer | Number of desired eigenvalues. Default = 3 × NE; if NE = 0, all roots in range are returned |
| NORM | Character | Eigenvector normalisation scheme. "MASS" = normalise to unit mass (Default), "MAX" = normalise to unit maximum displacement, "POINT" = normalise to displacement at grid G, component C |
| G | Integer | Grid point ID for NORM = "POINT" normalisation |
| C | Integer (1–6) | DOF component at G for NORM = "POINT" normalisation |

## Key Remarks

- Selected by Case Control command `METHOD = SID`.
- If an EIGRL entry with the same SID also exists, EIGRL takes precedence over EIGR.
- G and C are required only when NORM = "POINT"; ignored otherwise.
- For the Lanczos method ("LAN"), providing a good estimate for NE improves performance.

## Example

```
$       SID     METHOD  F1      F2      NE      ND
EIGR    100     LAN     0.0     1000.0  10      0
$               NORM
+               MASS
```
