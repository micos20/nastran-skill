# PLOAD2 — Uniform Normal Pressure Load on a Surface Element

Defines a uniform static pressure load applied to CQUAD4, CSHEAR, or CTRIA3 two-dimensional elements identified by element ID.

## Format

Standard (explicit element list):

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PLOAD2 | SID | P | EID1 | EID2 | EID3 | EID4 | EID5 | EID6 | |
| | EID7 | EID8 | -etc.- | | | | | | |

Alternate (THRU range):

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PLOAD2 | SID | P | EID1 | "THRU" | EID2 | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| P | Real | Pressure value. Direction follows right-hand rule from the element's grid point sequence |
| EIDi | Integer ≥ 0 or blank | Element identification numbers. Must be CQUAD4, CSHEAR, or CTRIA3 elements |
| "THRU" | Character | Keyword selecting all elements with IDs from EID1 to EID2 (alternate format) |

## Key Remarks

- Selected by Case Control command `LOAD = SID` (directly or via a LOAD combination entry).
- **Direction:** the pressure acts in the direction of the element's positive normal, determined by applying the right-hand rule to the grid point connection sequence on the element entry (same convention as PLOAD).
- At least one positive EID must appear on each PLOAD2 entry.
- When using the THRU alternate format, all elements with IDs in the range EID1–EID2 must be two-dimensional elements.
- Applicable elements: CQUAD4, CSHEAR, CTRIA3. For higher-order shell elements or solid element faces, use PLOAD4.
- Element IDs may appear on multiple continuation lines in the explicit form.

## Example

```
$       SID     P       EID1    EID2    EID3    EID4    EID5    EID6
PLOAD2  21      -3.6            4               16              2

$       THRU range
PLOAD2  1       30.4    16      THRU    48
```
