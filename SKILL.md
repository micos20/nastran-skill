---
name: nastran-card-structure
description: |
  Reference for MSC Nastran Bulk Data card syntax (BDF/DAT files). Use this skill
  whenever the user is reading, writing, or debugging Nastran bulk data entries —
  even if they just mention a card name like GRID, CQUAD4, CTRIA3, CBAR, CBEAM,
  CBUSH, CGAP, CROD, PSHELL, PCOMP, PBAR, PBEAM, PBUSH, PGAP, PROD, MAT1, MAT8,
  CORD1R, CORD2R, CORD1C, CORD2C, RBE1, RBE2, or RBE3. Also trigger when the
  user asks about field format, continuation lines, element–property–material
  relationships, rigid elements, gap/contact elements, or any .bdf or .dat file
  content — even if they don't use the word "card" or "Nastran" explicitly.
---

## Bulk Data Format Rules

MSC Nastran bulk data uses a 10-field record. Three formats are supported:

### Small field (default)
- Each record has 10 fields of 8 characters each (columns 1–80)
- Field 1: card name (left-justified)
- Fields 2–9: data values
- Field 10: optional continuation pointer (any non-blank character)
- Continuation line: field 1 must match field 10 of the preceding line (or be blank/`+`)

### Large field (double-precision)
- Card name in field 1 has an asterisk suffix, e.g. `GRID*`
- Fields 2–5 use 16 characters each (two small fields merged)
- Two physical lines required per logical record

### Free field
- Fields separated by commas (no fixed column widths)
- Card name first, then comma-separated values
- Continuation: trailing comma or `+` at end of line

### Field types
| Type | Notes |
|---|---|
| Integer | No decimal point; may not be blank unless explicitly allowed |
| Real | Decimal or scientific notation (e.g. `1.5`, `1.5E3`, `1.5+3`); trailing `+`/`-` shorthand is valid |
| Character | Left-justified string; case-insensitive unless noted |
| Blank | Means "use default" — only valid where the field description says "Default = ..." |

### Defaults and blanks
- A blank field uses the card's stated default value
- If no default is given, the field is required
- Integer fields: blank is only allowed when the description explicitly permits it

### Writing / generating cards

When writing or generating Nastran bulk data, always use **small-field format** unless the user explicitly requests large-field or free-field. The non-negotiable rule:

> **Every field is exactly 8 characters wide. Left-justify the value and pad with trailing spaces.**

Column layout (80 characters total):

```
Cols  1- 8  Field  1  card name (or blank/+ for continuation)
Cols  9-16  Field  2  first data field
Cols 17-24  Field  3
Cols 25-32  Field  4
Cols 33-40  Field  5
Cols 41-48  Field  6
Cols 49-56  Field  7
Cols 57-64  Field  8
Cols 65-72  Field  9
Cols 73-80  Field 10  continuation pointer (or blank)
```

For **comment header lines** above a card, place `$` in column 1 and align each label to the same 8-character grid:

```
$       EID     PID     G1      G2      G3      G4      THETA   ZOFFS
CQUAD4  1       10      101     102     103     104     45.0    0.0
$               TFLAG   T1      T2      T3      T4
+               0       0.25    0.25    0.25    0.25
```

Common mistakes to avoid:
- Using tab characters or variable-width spaces between values
- Forgetting that `CQUAD4` is 6 chars — it needs 2 trailing spaces to fill field 1: `CQUAD4  `
- Placing a value in column 9 when it should start at column 9 (i.e. field 2 starts at col 9, not col 10)

### Comments ($)
- A line beginning with `$` is a comment — ignored by the solver, appears only in the unsorted bulk data echo
- Format: `$` followed by any text up to column 80
- Example: `$ TEST FIXTURE-THIRD MODE`
- A `$` can also appear mid-line to terminate a fixed-field or free-field entry; all fields after the `$` are treated as blank (requires default IFPSTAR=YES)
- Example: `GRID, 101, , 1.0, 2.0, 0.0 $ node at (1,2,0)`

---

## Card Dependency Map

```
CBAR      → GA, GB: GRID          PID: PBAR
             orientation: X1/X2/X3 or G0 (GRID)

CBEAM     → GA, GB: GRID          PID: PBEAM
             orientation: X1/X2/X3 or G0 (GRID)

CBUSH     → GA, GB: GRID          PID: PBUSH
             orientation: X1/X2/X3 or G0 (GRID), or CID (CORDiR/C)

CQUAD4    → G1–G4: GRID           PID: PSHELL or PCOMP
             MCID: CORDiR or CORDiC (optional)

CTRIA3    → G1–G3: GRID           PID: PSHELL or PCOMP
             MCID: CORDiR or CORDiC (optional)

PSHELL    → MID1, MID2, MID3, MID4: MAT1 (or MAT2/MAT8)

PCOMP     → MIDi per ply: MAT1 or MAT8

PBAR      → MID: MAT1

PBEAM     → MID: MAT1

PBUSH     → no material reference (stiffness/damping entered directly)

GRID      → CP: CORDiR or CORDiC (input coord system, optional)
             CD: CORDiR or CORDiC (output coord system, optional)

CORD1R/C  → G1, G2, G3: GRID (defining grid points)
CORD2R/C  → no GRID references (defined by coordinates)

CGAP      → GA, GB: GRID          PID: PGAP
             orientation: X1/X2/X3 or G0 (GRID), or CID (CORDiR/C)
             CID required when GA and GB are coincident

CROD      → G1, G2: GRID          PID: PROD

RBE1      → GNi: GRID (independent grid points)
             GMj: GRID (dependent grid points)
             no property card (constraints defined directly on element)

RBE2      → GN: GRID (single independent hub grid)
             GMi: GRID (dependent grid points, any number)
             no property card (constraints defined directly on element)

RBE3      → REFGRID: GRID (reference/dependent node)
             Gi,j: GRID (contributing/independent grids)
             no property card (interpolation constraint, adds no stiffness)

PGAP      → no material reference (stiffness/friction entered directly)

PROD      → MID: MAT1
```

---

## Card Index

| Card | Category | File |
|---|---|---|
| GRID | Geometry | references/cards/GRID.md |
| CORD1R | Coordinate System | references/cards/CORD1R.md |
| CORD2R | Coordinate System | references/cards/CORD2R.md |
| CORD1C | Coordinate System | references/cards/CORD1C.md |
| CORD2C | Coordinate System | references/cards/CORD2C.md |
| CBAR | 1D Element | references/cards/CBAR.md |
| CBEAM | 1D Element | references/cards/CBEAM.md |
| CBUSH | 1D Element | references/cards/CBUSH.md |
| CQUAD4 | 2D Element | references/cards/CQUAD4.md |
| CTRIA3 | 2D Element | references/cards/CTRIA3.md |
| PBAR | 1D Property | references/cards/PBAR.md |
| PBEAM | 1D Property | references/cards/PBEAM.md |
| PBUSH | 1D Property | references/cards/PBUSH.md |
| PSHELL | 2D Property | references/cards/PSHELL.md |
| PCOMP | 2D Property | references/cards/PCOMP.md |
| MAT1 | Material | references/cards/MAT1.md |
| MAT8 | Material | references/cards/MAT8.md |
| CGAP | 1D Element | references/cards/CGAP.md |
| CROD | 1D Element | references/cards/CROD.md |
| RBE1 | Rigid Element | references/cards/RBE1.md |
| RBE2 | Rigid Element | references/cards/RBE2.md |
| RBE3 | Rigid Element | references/cards/RBE3.md |
| PGAP | 1D Property | references/cards/PGAP.md |
| PROD | 1D Property | references/cards/PROD.md |

---

## Loading Instructions

### Reference documents (MSC Nastran 2025.1)

| File | Contents | Size |
|---|---|---|
| `references/cards/*.md` | 24 pre-extracted card summaries | minimal |
| `references/QRG-BULKDATA.pdf` | Bulk Data Entries chapter only (Ch. 9) | 27.8 MB, ~700 pp |
| `references/MSC_Nastran_2025.1_Quick_Reference_Guide.pdf` | Complete QRG — all chapters | extremely large |

**Always cite MSC Nastran version 2025.1 when referencing QRG content.**

### Loading priority

1. Identify which cards are relevant to the task (element + its grids, property, and material form the dependency chain).
2. Read only the needed files from `references/cards/`. Do not load all files at once — load on demand.
3. For cards **not** in the index, the first fallback is `references/QRG-BULKDATA.pdf` (Bulk Data chapter only). **Before reading it, explicitly ask the user for permission**, e.g.: *"The card XYZ is not in the quick-reference index. Looking it up in the QRG Bulk Data PDF will use a significant number of tokens. Do you want me to proceed?"* Only read the PDF after the user confirms. Use the approximate page formula: `PDF_page ≈ QRG_page − 1018`. The bulk data chapter (Chapter 9) starts around PDF page 230.
4. For topics outside the Bulk Data chapter (Case Control, Executive Control, solution sequences, etc.), the fallback is `references/MSC_Nastran_2025.1_Quick_Reference_Guide.pdf`. This file is extremely large. **Always ask for explicit user permission before opening it**, with a clear warning, e.g.: *"This topic requires the full QRG (very large file — high token cost). Do you want me to look it up?"*

### Typical loading patterns

| User asks about... | Load these files |
|---|---|
| Defining a CQUAD4 | CQUAD4.md, GRID.md, PSHELL.md (or PCOMP.md) |
| Composite laminate setup | PCOMP.md, MAT8.md, CQUAD4.md or CTRIA3.md |
| CBEAM from grid to grid | CBEAM.md, PBEAM.md, GRID.md |
| CBAR with property | CBAR.md, PBAR.md, GRID.md, MAT1.md |
| Spring/bushing element | CBUSH.md, PBUSH.md |
| Coordinate system | CORD2R.md (most common) or CORD1R/CORD1C/CORD2C.md |
| Material properties | MAT1.md (isotropic) or MAT8.md (orthotropic/composite) |
| Gap/contact element | CGAP.md, PGAP.md, GRID.md |
| Rod element | CROD.md, PROD.md, GRID.md, MAT1.md |
| Rigid body element (RBE2) | RBE2.md, GRID.md |
| Interpolation constraint (RBE3) | RBE3.md, GRID.md |
| General rigid body (RBE1) | RBE1.md, GRID.md |
