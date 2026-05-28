# NONLINEAR (Case) — Nonlinear Dynamic Load Set Selection

Selects the nonlinear dynamic load set for transient response or nonlinear harmonic response problems.

## Format

```
NONLINEAR = n
```

## Example

```
NONLINEAR = 75
```

## Describers

| Describer | Meaning |
|---|---|
| n | Set identification number of a NOLINi, NLRGAP, or NLRSFD Bulk Data entry. (Integer > 0) |

## Key Remarks

- NOLINi (NOLIN1, NOLIN2, NOLIN3, NOLIN4), NLBSH3D, NLRGAP, and NLRSFD Bulk Data entries are ignored unless selected by this command.
- At least one DOF must be defined on the nonlinear force entry. In nonlinear harmonic response, the NONLINEAR command calls up the nonlinear force entry.
- This command is unrelated to the NLPARM or NLSTEP commands used for nonlinear static/transient control parameters.

## See Also

- NLPARM Bulk Data entry — parameters for nonlinear static analysis (SOL 101/400)
- NLSTEP Bulk Data entry — time step control for SOL 400
