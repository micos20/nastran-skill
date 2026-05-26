# SET3 — Defines a List of Grids, Elements, Points or Modules

Defines a typed list of grids, elements, scalar points, properties, or modules. More flexible than SET1 because the DES field controls what the IDs refer to.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| SET3 | SID | DES | ID1 | ID2 | ... | IDi | | | |
| | IDi+1 | -etc.- | | | | | | | |

Continuation lines repeat as needed.

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Unique set identification number |
| DES | Character | Set descriptor. Valid values: "GRID", "ELEM", "POINT", "PROP", "RBEin", "RBEex", "MODULE", "SEOVRD" |
| IDi | Integer ≥ 0 | Identifiers of the items named by DES. May be 0 when DES = "MODULE"; must be > 0 otherwise. Supports "THRU" ranges. |

## DES Values

| DES | Contents |
|---|---|
| "GRID" | Grid point IDs |
| "ELEM" | Element IDs |
| "POINT" | Scalar point IDs |
| "PROP" | Property IDs |
| "RBEin" | Rigid element IDs to include in MPC=sid selection |
| "RBEex" | Rigid element IDs to exclude from MPC=sid selection |
| "MODULE" | Module IDs |
| "SEOVRD" | Superelement IDs (SOL 101 INREL=−1 or −2 suspension) |

## Key Remarks

- SET3 is referenced by entries such as PBMSECT, PBRSECT (DES="POINT"), SOL 400 DEACTEL (DES="ELEM"), and FTGDEF (DES="PROP" or "ELEM"), among others.
- In SOL 400, only DES = "GRID" or "ELEM" may be used.
- "RBEin" and "RBEex" are mutually exclusive for a single SET3 entry — do not combine them.
- "RBEin": selects rigid elements (RBAR, RBE1, RBE2, RBE2GS, RBE3, RROD, RSPLINE, RSSCON, RTRPLT, RTRPLT1) to be included for MPC=sid. Without a SET3,SID,RBExx entry, all rigid elements in the deck are used.
- "RBEex": selects rigid elements to be excluded from MPC=sid.
- THRU may not appear in field 4 or field 9 on the parent line, or in field 2 or field 9 on continuation lines.

## Examples

```
$ Grid point list
SET3    1       POINT   11      12      13      15      18      21

$ Element range using THRU
SET3    33      POINT   20      THRU    60
```
