# SET1 — Set Definition

Defines a list of structural grid point or element identification numbers.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SET1 | SID | ID1 | ID2 | ID3 | ID4 | ID5 | ID6 | ID7 | |
| | ID8 | -etc.- | | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Unique set identification number |
| IDi | Integer > 0 or "THRU" or "SKIN" | Structural grid point or element IDs. For "THRU": ID1 < ID2. "SKIN" may appear in field 3 only. |

## Key Remarks

- The most widely used set card — referenced by output requests (DISPLACEMENT, STRESS, FORCE, etc.), SPLINEi, SPC, AECOMP, XYOUTPUT, and many others.
- **THRU option**: Use `ID1 THRU ID2` to specify a range. For SPLINEi and PANEL requests, all intermediate IDs must exist. For XYOUTPUT and AECOMP, missing points are ignored.
- **SKIN option**: Entering "SKIN" in field 3 generates a panel at the fluid-structural boundary. Requires all ACMODL fields to have their default values.
- THRU may not appear in field 3 or field 9 on the parent line, or in field 2 or field 9 on continuation lines.
- The same SID may not be used by both a SET1 and SET2 entry.

## Examples

```
$ List of discrete grid IDs
SET1    3       31      62      93      124     16      17      18
        19

$ Range using THRU
SET1    6       29      32      THRU    50      61      THRU    70
        17      57

$ SKIN option
SET1    7       SKIN
```
