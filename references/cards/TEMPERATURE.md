# TEMPERATURE (Case) — Temperature Set Selection

Selects the temperature set used for thermal loading, temperature-dependent material properties, or initial temperature distribution.

## Format

```
TEMPERATURE [(INITIAL | MATERIAL | LOAD | BOTH)]
            [, HSUBCASE = i, HSTEP = j, HTIME = t | ALL]
            [, VERIFY = (GRID | ELEMENT | BOTH | NONE)]
            = n
```

## Examples

```
TEMPERATURE = 50
TEMPERATURE(LOAD) = 100
TEMPERATURE(MATERIAL) = 100
TEMPERATURE(INITIAL) = 5
TEMPERATURE(LOAD, HSUBCASE=1, HSTEP=2, HTIME=1.0) = 10
```

## Describers

| Describer | Meaning |
|---|---|
| BOTH | Selects the temperature set for both material property evaluation and equivalent static thermal load computation. **Default when no option is specified.** |
| LOAD | Computes an equivalent static thermal load from the temperature field; also updates temperature-dependent material properties in nonlinear analysis. |
| MATERIAL | Uses the temperature set for temperature-dependent material property evaluation only (no thermal load). |
| INITIAL | Specifies the initial temperature distribution for nonlinear static analysis. Must appear above or in the first SUBCASE/STEP. |
| HSUBCASE = i | SOL 400 multi-physics: subcase ID in the thermal database from which temperatures are retrieved (Integer > 0). |
| HSTEP = j | SOL 400 multi-physics: step ID in the thermal database (Integer > 0). |
| HTIME = t \| ALL | SOL 400 multi-physics: time value or ALL (retrieves all times) from the thermal database. At least one of HSUBCASE, HSTEP, HTIME must be present to activate multi-physics retrieval. |
| VERIFY | Requests verification output of temperature data: GRID, ELEMENT, BOTH, or NONE (default). |
| n | Set identification number of TEMP, TEMPD, TEMPP1, TEMPB3, TEMPRB, or TEMPAX Bulk Data entries. (Integer > 0) |

## Key Remarks

- In **linear analysis**: only one `TEMPERATURE(MATERIAL)` command is allowed per problem. If placed in multiple subcases, only the last occurrence is used; preceding ones are ignored with a warning.
- Total applied load vector: {P} = external `LOAD` + `TEMP(LOAD)` + `DEFORM` + SPC enforced displacement.
- `TEMPERATURE(INITIAL)` must be placed **above subcase level** (or in the first SUBCASE/STEP); `TEMPERATURE(LOAD)` must be within the subcase.
- `TEMPERATURE(MATERIAL)` is **not supported** for hyperelastic elements using the MATHP material entry in linear analysis.
- For **layered composites** (PCOMP/PCOMPG): the `TREF` field on the property entry governs the ply reference temperature, not `TEMPERATURE(INITIAL)`.
- The multi-physics coupling option (HSUBCASE/HSTEP/HTIME) requires a thermal solution database from a prior heat transfer analysis. At least one of these keywords must be specified to activate the retrieval.

## Typical Usage (SOL 101 — Thermal + Mechanical Load)

```
TEMPERATURE(LOAD) = 50
SPC = 1

SUBCASE 1
  LOAD = 10
  DISP = ALL
  STRESS = ALL
```

## Typical Usage (SOL 400 — Multi-Physics Thermal Coupling)

```
SUBCASE 1
  STEP 1
    ANALYSIS = NLSTATICS
    LOAD = 10
    TEMPERATURE(LOAD, HSUBCASE=1, HSTEP=1, HTIME=ALL) = 200
    NLSTEP = 100
    SPC = 1
```

## See Also

- TEMP Bulk Data entry — defines temperatures at individual grid points
- TEMPD Bulk Data entry — defines a default temperature for all grids
- TEMPP1 Bulk Data entry — temperatures for 2D (plate/shell) elements
- TEMPB3 Bulk Data entry — temperatures for beam elements
- LOAD Case Control command — selects external static load set
