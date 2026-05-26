# LOAD — Static Load Combination (Superposition)

Defines a static load as a linear combination of load sets. Used to scale and sum multiple individual load entries into a single applied load set.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| LOAD | SID | S | S1 | L1 | S2 | L2 | S3 | L3 | |
| | S4 | L4 | -etc.- | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| SID | Integer > 0 | Load set identification number. Selected by Case Control `LOAD = SID` |
| S | Real | Overall scale factor applied to the entire combination |
| Si | Real | Scale factor on load set Li |
| Li | Integer > 0 | Load set ID. May reference FORCE, MOMENT, FORCE1, FORCE2, MOMENT1, MOMENT2, DAREA (converted), PLOAD, PLOAD1, PLOAD2, PLOADB3, PLOAD4, PLOADX1, SLOAD, RFORCE, GRAV, ACCEL, ACCEL1 entries |

The combined load vector is: {P} = S · Σ Si · {P_Li}

## Key Remarks

- Selected by Case Control command `LOAD = SID`.
- **Must be used** when combining GRAV (gravity) with any other load type — GRAV cannot be combined by referencing two separate SIDs in Case Control.
- Si and Li pairs may continue on as many continuation lines as needed (one Si/Li pair per two fields).
- Load set IDs (Li) must be unique (no two Li may reference the same SID on the same LOAD entry).
- Nested references are permitted: a LOAD entry may reference the SID of another LOAD entry; a LOAD entry may not reference itself.
- LOAD does **not** combine SPCD (enforced displacement) entries — SPCD must be referenced directly via the LOAD Case Control command using its own SID.
- In dynamic analyses, the EXCITEID field of RLOADi or TLOADi may reference a LOAD entry SID.

## Example

```
$       SID     S       S1      L1      S2      L2      S3      L3
LOAD    101     -0.5    1.0     3       6.2     4
```

This creates load set 101 = −0.5 × (1.0 × {P_3} + 6.2 × {P_4}).
