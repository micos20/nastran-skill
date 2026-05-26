# TEMPD — Grid Point Temperature Field Default

Defines a default temperature value for all grid points of the structural model that have not been given a temperature on a TEMP or TEMPN1 entry.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| TEMPD | SID1 | T1 | SID2 | T2 | SID3 | T3 | SID4 | T4 | |

## Fields

| Name | Type | Description |
|---|---|---|
| SIDi | Integer > 0 | Temperature set identification number |
| Ti | Real | Default temperature value for all grid points not explicitly listed on a TEMP or TEMPN1 entry |

## Key Remarks

- One to four SID–temperature pairs may be defined on a single entry.
- Selected by Case Control `TEMP = SID` (static sequences) or `TEMP(INIT) = SID` (nonlinear heat transfer initialization). Same selection rules as TEMP.
- SID must be unique with respect to all other LOAD type entries if `TEMP(LOAD)` is specified in Case Control.
- The default temperature applies only to grid points **not** listed on any TEMP or TEMPN1 entry with the same SID. Per-grid TEMP values always take precedence.
- **Element temperature averaging:** element temperatures are computed as the average of connected grid point temperatures (those from TEMP entries, or the TEMPD default if no TEMP entry covers that grid). Directly defined element temperatures from TEMPP1, TEMPRB, or TEMPB3 always take precedence.
- For nonlinear heat transfer initialization (SOLs 153, 159), used together with TEMP and TEMPN1 entries.
- In partitioned Bulk Data (superelement) models, TEMPD must be specified in **all** partitioned Bulk Data Sections. If Modules are present, TEMPD may only appear in the main Bulk Data section.

## Example

```
$       SID1    T1
TEMPD   1       216.3
```

All grid points not explicitly listed on a TEMP entry for SID=1 receive a temperature of 216.3.
