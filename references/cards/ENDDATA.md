# ENDDATA — Bulk Data Delimiter

Designates the end of the Bulk Data Section.

## Format

```
ENDDATA
```

No fields — ENDDATA is a single keyword with no data values.

## Key Remarks

- ENDDATA is optional. The solver will process all bulk data entries preceding it.
- Placing ENDDATA explicitly is good practice to clearly mark the end of the deck and prevent accidental inclusion of trailing lines.
