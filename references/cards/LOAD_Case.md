# LOAD (Case Control) — External Static Load Set Selection

Selects an external static load set to apply in a subcase.

## Format

```
LOAD = n
```

## Example

```
LOAD = 15
```

## Describers

| Describer | Meaning |
|---|---|
| n | Set identification number of at least one external load Bulk Data entry. Accepted entry types: ACCEL, ACCEL1, FORCE, FORCE1, FORCE2, FORCEAX, GRAV, LOAD, MOMAX, MOMENT, MOMENT1, MOMENT2, MPCD, PLOAD, PLOAD1, PLOAD2, PLOAD4, PLOADB3, PLOADX1, QVOL, QVECT, QHBDY, QBDY1, QBDY2, QBDY3, PRESAX, RFORCE, SPCD, or SLOAD. (Integer > 0) |

## Key Remarks

- A GRAV entry cannot share a set ID with other load types. To apply gravity together with other static loads, use a LOAD Bulk Data entry to superpose them under a common SID.
- LOAD is applicable in linear/nonlinear statics, inertia relief, differential stiffness, buckling, and heat transfer analyses.
- Total applied load = external (LOAD) + thermal (TEMP(LOAD)) + element deformation (DEFORM) + enforced displacement (SPC/SPCD).
- Static, thermal, and element deformation loads should each have unique set IDs.

## See Also

- LOAD Bulk Data entry — combines multiple load sets with scale factors (Chapter 9)
- FORCE, FORCE1, FORCE2, MOMENT, PLOAD, PLOAD1, PLOAD2, PLOAD4, GRAV Bulk Data entries
