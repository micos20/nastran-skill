# OUTPUT (Case) — Case Control Section Delimiter

Delimits the various types of output control commands: structure plotter, curve plotter, and grid point stress.

## Format

```
OUTPUT [(PLOT | POST | XYOUT | XYPLOT)]
```

## Examples

```
OUTPUT(POST)
OUTPUT(PLOT)
OUTPUT(XYOUT)
```

## Describers

| Describer | Meaning |
|---|---|
| (blank) | Subsequent commands are standard Case Control commands. |
| PLOT | Beginning of structure plotter command block. Must precede all structure plotter control commands. |
| POST | Beginning of grid point stress commands. Must precede SURFACE and VOLUME commands. |
| XYOUT or XYPLOT | Beginning of curve plotter command block. XYOUT and XYPLOT are equivalent. |

## Key Remarks

- OUTPUT must appear at the **end of normal Case Control**, just above the BEGIN BULK statement.
- Any Case Control command that controls selection or analysis flow (e.g., `TEMP(LOAD)`, `LOAD`) that occurs *after* an OUTPUT entry will be **ignored**.
- `OUTPUT(PLOT)`, `OUTPUT(XYOUT/XYPLOT)`, and `OUTPUT(POST)` must all follow any standard Case Control commands.
- `OUTPUT(XYPLOT)` forces SORT2 output format and overrides SORT1 requests.
- Commands after `OUTPUT(POST)` are the SURFACE and VOLUME grid point stress commands.

## Typical Input File Placement

```
...                     ← standard Case Control commands
SUBCASE 1
  LOAD = 10
  DISP = ALL
  STRESS = ALL
OUTPUT(POST)            ← opens grid point stress block
  SURFACE 10 SET 5 QUAD4-...
BEGIN BULK
```
