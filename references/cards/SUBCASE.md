# SUBCASE (Case) — Subcase Delimiter

Delimits and identifies a subcase within the Case Control Section.

## Format

```
SUBCASE n
```

(Alternate form `SUBCASE = n` is also accepted.)

## Example

```
SUBCASE 101
```

## Describers

| Describer | Meaning |
|---|---|
| n | Subcase identification number. (Integer, 0 < n < 9999999) |

## Key Remarks

- The subcase identification number must be **greater than all previous** subcase identification numbers.
- All Case Control commands that appear after a `SUBCASE n` line and before the next `SUBCASE` (or `BEGIN BULK`) apply only to that subcase, overriding any global (above-first-subcase) defaults.
- Plot requests and RANDPS requests reference the subcase by its ID n.
- If a `$` comment follows n on the same line, the first few characters of the comment appear as the subcase label in the upper-right corner of the printed output.
- In **nonlinear statics** (SOL 106 and SOL 129), subcases are **not independent** — they act as a load progression. The ending conditions of one SUBCASE become the initial conditions of the next SUBCASE.
- See the `MODES` Case Control command for use of SUBCASE in normal modes analysis.
- For SOL 400: use `STEP` commands within a SUBCASE to further subdivide the analysis.

## Typical Usage (SOL 101 — Multiple Load Cases)

```
TITLE = MY MODEL
DISP = ALL
SPC = 1

SUBCASE 1             $ Gravity load
  LOAD = 10

SUBCASE 2             $ Pressure load
  LOAD = 20

SUBCASE 3             $ Combined
  LOAD = 30
```

## Typical Usage (SOL 400 — Nonlinear with STEP)

```
SUBCASE 1
  STEP 1
    LOAD = 1
    NLPARM = 10
  STEP 2
    LOAD = 2
    NLPARM = 10
```
