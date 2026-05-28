# LOADSET (Case) — Static Load Set Selection

Selects a sequence of static load sets to be applied to the model; loads may be referenced by dynamic load commands via LSEQ Bulk Data entries.

> **Obsolete.** The QRG explicitly states: *"Use of the LOADSET/LSEQ should be avoided as it is an obsolete entry and is never needed and is only documented for legacy code. Some new features such as SUBSTEP do not support it and will issue a fatal message."* Use direct LOAD Case Control commands referencing EXCITEID-compatible Bulk Data entries instead.

## Format

```
LOADSET = n
```

## Example

```
LOADSET = 100
```

## Describers

| Describer | Meaning |
|---|---|
| n | Set identification number of at least one LSEQ Bulk Data entry. (Integer > 0) |

## Key Remarks

- In dynamic solution sequences (SOLs 108, 109, 111, 112, 118, 146): LOADSET must appear in the first subcase or above all subcases.
- In nonlinear statics (SOLs 106, 153): must appear above all subcases; only one LOADSET allowed per run.
- In superelement analysis: must be used for all superelements and specified in each superelement's first subcase.
- When LOADSET is present in a static analysis, any LOAD commands within subcases are ignored.
- LOADSET/LSEQ is not needed when RLOAD1/2, TLOAD1/2, ACSRCE entries reference static load SIDs directly (modern practice).
