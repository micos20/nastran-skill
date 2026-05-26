# SET4 — Property Set Definition

Defines a list of property IDs of a specified type. Used in fatigue analysis (FTGDEF entry).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SET4 | ID | CLASS | TYPE | ID1 | ID2 | ID3 | ID4 | ID5 | |
| | ID6 | ID7 | ID8 | -etc.- | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| ID | Integer > 0 | Unique identification number |
| CLASS | Character | Must be "PROP". No default — must be specified. |
| TYPE | Character | Property type. Valid values: PSOLID, PSHELL, PSHEAR, PBAR, PBEAM, PWELD |
| IDi | Integer > 0 or "THRU" | Property IDs of the specified TYPE |

## Key Remarks

- SET4 is currently referenced only from the FTGDEF entry (fatigue definition).
- CLASS must always be "PROP" — no other value is valid.
- TYPE filters which property card type the listed IDs belong to.
- THRU option may not appear in field 5 or field 9 on the first line, or in field 2 or field 9 on continuation lines.

## Example

```
SET4    22      PROP    PSOLID  1       THRU    20
```
