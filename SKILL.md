---
name: nastran-card-structure
description: |
  Reference for MSC Nastran Bulk Data card syntax (BDF/DAT files). Use this skill
  whenever the user is reading, writing, or debugging Nastran bulk data entries —
  even if they just mention a card name like GRID, CQUAD4, CTRIA3, CBAR, CBEAM,
  CBUSH, CGAP, CROD, PSHELL, PCOMP, PBAR, PBARL, PBEAM, PBUSH, PGAP, PROD,
  MAT1, MAT8, CORD1R, CORD2R, CORD1C, CORD2C, RBE1, RBE2, RBE3, ENDDATA,
  SET1, SET2, SET3, SET4, ITER, NLPARM, EIGR, EIGRL, LOAD, PLOAD, PLOAD1,
  PLOAD2, PLOAD4, FORCE, FORCE1, FORCE2, TEMP, TEMPD, MPC, SPC, SPC1,
  MPCADD, or SPCADD. Also trigger when the user asks about field format,
  continuation lines, element–property–material relationships, rigid elements,
  gap/contact elements, set definitions, load combinations, pressure loads,
  distributed loads, concentrated forces, thermal loading, temperature fields,
  single-point constraints, multipoint constraints, enforced displacement,
  or any .bdf or .dat file content.
  Also trigger for Case Control topics: SUBCASE, STATSUB, NONLINEAR, OUTPUT,
  ANALYSIS, METHOD, NLBUCK, NLOPRM, NLSTEP, STEP, TEMPERATURE,
  subcase scope, output requests, SET command, LOAD/SPC/MPC/METHOD/NLPARM/TEMP
  selection in Case Control, subcase delimiters, static load set selection,
  differential stiffness, buckling preload, nonlinear step control, analysis
  type selection, eigenvalue method, thermal loading, temperature-dependent
  material, or the structure of the input file (Executive Control / Case
  Control / Bulk Data) — even if they don't use the word "card" or "Nastran"
  explicitly.
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

## Case Control Format Rules

The Case Control section sits between the Executive Control deck and `BEGIN BULK`. It selects loads and constraints, requests output, and defines the subcase structure.

### Input file structure

```
$ Executive Control
SOL 101
CEND
$------- Case Control -----------------------------------------------
TITLE = My Analysis
SPC   = 1
LOAD  = 10
SUBCASE 1
  LOAD = 100
  DISPLACEMENT = ALL
SUBCASE 2
  LOAD = 200
  DISPLACEMENT = ALL
BEGIN BULK
$------- Bulk Data --------------------------------------------------
```

`CEND` ends Executive Control and starts Case Control. `BEGIN BULK` ends Case Control and starts Bulk Data.

### Format rules

- **Free format** — no fixed column widths (unlike bulk data's 8-character fields)
- **Case-insensitive** — `SPC`, `spc`, `Spc` are all equivalent
- **Comments** — `$` in column 1, same as bulk data
- **Abbreviation** — commands may be shortened to the first 4 characters if that abbreviation is unique among all commands (e.g., `DISP` for `DISPLACEMENT`); if not unique, specify the full name or at least the first 8 characters
- **Continuation** — if a command exceeds 72 columns, end the line with a comma and continue on the next line:
  ```
  SET 1 = 5, 6, 7, 8, 9,
          10 THRU 55
  ```

### Command syntax forms

| Pattern | Example | Notes |
|---|---|---|
| `CMD = value` | `LOAD = 10` | Most data-selection commands |
| `CMD = ALL` | `DISPLACEMENT = ALL` | All applicable entities |
| `CMD = NONE` | `STRESS = NONE` | Suppress output |
| `CMD = n` | `DISPLACEMENT = 1` | Entities in SET n |
| `CMD(opt) = value` | `STRESS(SORT1,PRINT) = ALL` | Options in parentheses |
| `CMD n` (no `=`) | `SUBCASE 10` | Subcase delimiters |

The `=` sign is optional for some commands (`SUBCASE 10` and `SUBCASE = 10` are equivalent).

### Notation in QRG format descriptions

| Symbol | Meaning |
|---|---|
| `UPPERCASE` | Keyword — must be entered exactly as shown |
| `lowercase` | Variable — user supplies the value; type and range given in parentheses |
| `{ }` (braces) | Mandatory choice — pick exactly one of the stacked options |
| `[ ]` (brackets) | Optional group — may be omitted entirely; if options are stacked, pick one |
| Shaded option | Default — used when the optional group is omitted |

### Scope: global vs. subcase-local

Commands before the first `SUBCASE` are **global** — they apply to all subcases unless overridden. Commands inside a `SUBCASE` block are **local** — they apply only to that subcase and override any matching global command.

```
SPC  = 1          $ global: all subcases use SPC set 1 unless overridden
LOAD = 10         $ global default load
SUBCASE 1
  LOAD = 100      $ local: overrides global LOAD for subcase 1
SUBCASE 2
  LOAD = 200
  SPC  = 2        $ local: overrides global SPC = 1 for subcase 2 only
```

### SET command

`SET` defines a list of IDs that output-request commands reference by number:

```
SET 1 = 101, 102, 103, 110 THRU 120
SET 2 = ALL
DISPLACEMENT = 1   $ output only the grid points in SET 1
STRESS = ALL       $ output for all elements
```

- `ALL` — all applicable entities
- `NONE` — suppress output
- `n` — entities in SET n (integers, THRU ranges, or a mix)

### Subcase delimiter commands

| Command | Purpose |
|---|---|
| `SUBCASE n` | Standard subcase |
| `SUBCOM n` | Combination subcase (linear combination of previous subcases) |
| `SYM n` | Symmetry subcase |
| `SYMCOM n` | Symmetry combination subcase |
| `REPCASE n` | Repeated output request subcase |

### Command categories (Chapter 5 overview)

Case Control commands fall into three broad groups:

**Subcase definition** — delimiters (`SUBCASE`, `SYM`, `SUBCOM`), subcase control (`MASTER`, `MODES`), combination coefficients (`SUBSEQ`, `SYMSEQ`)

**Data selection** — selects which Bulk Data entries are active for each subcase:
- Static loads: `LOAD`, `DEFORM`
- Dynamic loads: `DLOAD`, `LOADSET`, `NONLINEAR`
- Constraints: `SPC`, `MPC`, `AUTOSPC`, `BC`
- Temperatures: `TEMPERATURE`
- Eigenvalue / solver: `METHOD`, `SMETHOD`, `CMETHOD`
- Nonlinear: `NLPARM`, `NLSTEP`

**Output selection** — controls what is written to the .f06/.punch/.op2:
- `DISPLACEMENT`, `VELOCITY`, `ACCELERATION`
- `STRESS`, `STRAIN`, `FORCE`, `SPCFORCES`, `MPCFORCES`
- `OLOAD`, `ESE` (element strain energy)
- `ECHO`, `LABEL`, `TITLE`, `SUBTITLE`

Individual command reference files will be added to `references/cards/` as they are extracted.

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

PBARL     → MID: MAT1
             used with CBAR (same PID as PBAR; alternative dimensional input)

ENDDATA   → no dependencies (bulk data section terminator, optional)

SET1      → IDi: GRID or element IDs (integer lists or THRU ranges)

SET2      → MACRO: aerodynamic macro element (aeroelastic models only)

SET3      → IDi: GRID, element, or property IDs depending on DES field

SET4      → IDi: property IDs of TYPE (PSOLID/PSHELL/PSHEAR/PBAR/PBEAM/PWELD)

EIGR      → no element/property/material refs
             selected by Case Control METHOD = SID
             EIGRL takes precedence when same SID exists

EIGRL     → no element/property/material refs
             selected by Case Control METHOD = SID
             takes precedence over EIGR when same SID

ITER      → no element/property/material refs
             selected by Case Control SMETHOD = SID
             key=value syntax on continuation lines (no spaces within each token)

NLPARM    → no element/property/material refs
             selected by Case Control NLPARM = ID (required per nonlinear subcase)

LOAD      → Li: any static load SID (FORCE, MOMENT, PLOAD, PLOAD1, PLOAD2, PLOAD4, GRAV, etc.)
             selected by Case Control LOAD = SID
             required when combining GRAV with any other load type

PLOAD     → G1–G4: GRID (grid points defining loaded surface)
             no element reference (load applied directly to grid point geometry)

PLOAD1    → EID: CBAR, CBEAM, or CBEND element
             no property/material reference

PLOAD2    → EIDi: CQUAD4, CSHEAR, or CTRIA3 elements
             no property/material reference

PLOAD4    → EID: CHEXA, CPENTA, CPYRAM, CTETRA, CTRIA3, CTRIA6, CTRIAR, CQUAD4, CQUAD8, CQUADR
             G1, G3/G4: GRID (for solid element face identification)
             no property/material reference

FORCE     → G: GRID (where force is applied)
             CID: CORDiR or CORDiC (optional, direction coordinate system)
             no property/material reference

FORCE1    → G: GRID (where force is applied)
             G1, G2: GRID (define direction vector)
             no property/material reference

FORCE2    → G: GRID (where force is applied)
             G1, G2, G3, G4: GRID (define direction via cross product)
             no property/material reference

TEMP      → Gi: GRID (grid points receiving temperatures)
             no property/material reference
             selected by Case Control TEMP = SID

TEMPD     → no GRID/property/material references
             applies default temperature to all grids not listed on TEMP or TEMPN1
             selected by Case Control TEMP = SID

MPC       → Gj: GRID (constrained grid points)
             no property/material reference
             selected by Case Control MPC = SID
             first DOF (G1,C1) is the dependent DOF

MPCADD    → Sj: MPC set IDs (union of MPC entries)
             no GRID/property/material reference
             selected by Case Control MPC = SID
             takes precedence over MPC with same SID

SPC       → Gi: GRID (constrained grid points)
             no property/material reference
             selected by Case Control SPC = SID

SPC1      → Gi: GRID (constrained grid points, list or THRU range)
             no property/material reference
             selected by Case Control SPC = SID

SPCADD    → Si: SPC/SPC1 set IDs (union)
             no GRID/property/material reference
             selected by Case Control SPC = SID
             takes precedence over SPC/SPC1 with same SID

LOAD (Case) → n: FORCE/MOMENT/PLOAD/PLOAD4/GRAV/LOAD Bulk Data SID
               used in Case Control: LOAD = n
               GRAV must be combined via LOAD Bulk Data when mixed with other loads

LOADSET     → n: LSEQ Bulk Data entry SID
               OBSOLETE — documented for legacy reference only
               used in Case Control: LOADSET = n

NONLINEAR   → n: NOLINi, NLRGAP, or NLRSFD Bulk Data SID
               used in Case Control: NONLINEAR = n
               no element/property/material reference

OUTPUT      → no Bulk Data references
               Case Control section delimiter — must appear at end of Case Control,
               just above BEGIN BULK
               sub-type (PLOT/POST/XYOUT) opens a specialized command block

STATSUB     → n: subcase ID of a prior static SUBCASE
               used in same subcase as METHOD/CMETHOD/TSTEP/FREQ
               BUCKLING option: varying load subcase
               PRELOAD option: constant preload subcase

SUBCASE     → n: integer subcase ID (must be increasing)
               delimits all data/output selection commands for one load case
               in SOL 106/129: chains solutions (end state = next initial state)

ANALYSIS    → no Bulk Data references
               specifies analysis type per SUBCASE/STEP/SUBSTEP
               SOL 400: one per STEP (single physics) or per SUBSTEP (coupled)
               SOL 200: required in every SUBCASE

METHOD      → n: EIGR, EIGRL, or EIGB Bulk Data SID
               EIGRL takes precedence over EIGR when same SID

MPC (Case)  → n: MPC or MPCADD Bulk Data SID
               may also select rigid elements via SET3 (RBEin/RBEex)
               in SOL 400: RIGID=LINEAR required to use rigid element set selection

NLBUCK      → no Bulk Data references (uses existing NLSTEP for load increments)
               SOL 400 only; must be in same STEP/SUBCASE as NLSTEP
               NLPARM is NOT allowed with NLBUCK

NLOPRM      → no Bulk Data references
               keyword=value syntax; SOL 400 output/debug control only

NLSTEP (Case) → n: NLSTEP Bulk Data entry SID
                 if present anywhere in SUBCASE, all NLPARM in that SUBCASE are ignored
                 SOL 400 and SOL 101 linear contact only

SPC (Case)  → n: SPC, SPC1, SPCADD Bulk Data SID

STEP        → n: integer step ID (increasing within SUBCASE)
               SOL 400 only; continuation of previous STEP within same SUBCASE
               solutions chain (end state = next initial state)

TEMPERATURE → n: TEMP, TEMPD, TEMPP1, TEMPB3, TEMPRB, or TEMPAX Bulk Data SID
               LOAD/MATERIAL/INITIAL/BOTH options control usage
               HSUBCASE/HSTEP/HTIME keywords for multi-physics thermal coupling (SOL 400)
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
| PBARL | 1D Property | references/cards/PBARL.md |
| ENDDATA | Delimiter | references/cards/ENDDATA.md |
| SET1 | Set Definition | references/cards/SET1.md |
| SET2 | Set Definition | references/cards/SET2.md |
| SET3 | Set Definition | references/cards/SET3.md |
| SET4 | Set Definition | references/cards/SET4.md |
| EIGR | Normal Modes Control | references/cards/EIGR.md |
| EIGRL | Normal Modes Control | references/cards/EIGRL.md |
| ITER | Solver Control | references/cards/ITER.md |
| NLPARM | Nonlinear Control | references/cards/NLPARM.md |
| LOAD | Load Combination | references/cards/LOAD.md |
| PLOAD | Static Load | references/cards/PLOAD.md |
| PLOAD1 | Static Load | references/cards/PLOAD1.md |
| PLOAD2 | Static Load | references/cards/PLOAD2.md |
| PLOAD4 | Static Load | references/cards/PLOAD4.md |
| FORCE | Static Load | references/cards/FORCE.md |
| FORCE1 | Static Load | references/cards/FORCE1.md |
| FORCE2 | Static Load | references/cards/FORCE2.md |
| TEMP | Temperature Load | references/cards/TEMP.md |
| TEMPD | Temperature Load | references/cards/TEMPD.md |
| MPC | Multipoint Constraint | references/cards/MPC.md |
| MPCADD | MP Constraint Combo | references/cards/MPCADD.md |
| SPC | SP Constraint | references/cards/SPC.md |
| SPC1 | SP Constraint | references/cards/SPC1.md |
| SPCADD | SP Constraint Combo | references/cards/SPCADD.md |
| LOAD (Case) | CC Data Selection | references/cards/LOAD_Case.md |
| LOADSET | CC Data Selection | references/cards/LOADSET.md |
| NONLINEAR | CC Data Selection | references/cards/NONLINEAR.md |
| OUTPUT | CC Section Delimiter | references/cards/OUTPUT.md |
| STATSUB | CC Data Selection | references/cards/STATSUB.md |
| SUBCASE | CC Subcase Delimiter | references/cards/SUBCASE.md |
| ANALYSIS | CC Analysis Type | references/cards/ANALYSIS.md |
| METHOD | CC Data Selection | references/cards/METHOD.md |
| MPC (Case) | CC Constraint Selection | references/cards/MPC_Case.md |
| NLBUCK | CC NL Analysis | references/cards/NLBUCK.md |
| NLOPRM | CC NL Options | references/cards/NLOPRM.md |
| NLSTEP (Case) | CC NL Step Selection | references/cards/NLSTEP_Case.md |
| SPC (Case) | CC Constraint Selection | references/cards/SPC_Case.md |
| STEP | CC Step Delimiter | references/cards/STEP.md |
| TEMPERATURE | CC Data Selection | references/cards/TEMPERATURE.md |

---

## Loading Instructions

### Reference documents (MSC Nastran 2025.1)

| File | Contents | Size |
|---|---|---|
| `references/cards/*.md` | 64 pre-extracted card/command summaries | minimal |
| `references/MSC_Nastran_2025.1_Quick_Reference_Guide.pdf` | Complete QRG — all chapters (local dev only, not in git) | 34.8 MB |

**Always cite MSC Nastran version 2025.1 when referencing QRG content.**

### Loading priority

1. Identify which cards are relevant to the task (element + its grids, property, and material form the dependency chain).
2. Read only the needed files from `references/cards/`. Do not load all files at once — load on demand.
3. For cards **not** in the index, fall back to `references/MSC_Nastran_2025.1_Quick_Reference_Guide.pdf`. This file is large (34.8 MB). **Always ask for explicit user permission before opening it**, e.g.: *"The card XYZ is not in the quick-reference index. Looking it up in the full QRG will use a significant number of tokens. Do you want me to proceed?"* Only read the PDF after the user confirms. Use the page formula: `PDF_page = QRG_page + 32`. The Bulk Data chapter (Chapter 9) starts at QRG page 1019 (PDF page 1051).
4. For Case Control commands not yet in the index, the same file and the same permission rule apply. Chapter 5 starts at QRG page 175 (PDF page 207). The page formula is identical: `PDF_page = QRG_page + 32`. Case Control command files will live in `references/cards/` with a `(Case)` suffix in their title when added.

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
| Bar property (dimensional input) | PBARL.md, MAT1.md, CBAR.md |
| Set of grid/element IDs | SET1.md |
| Typed set (grid, elem, prop) | SET3.md |
| End of bulk data section | ENDDATA.md |
| Normal modes extraction (EIGR) | EIGR.md |
| Normal modes extraction (EIGRL, preferred) | EIGRL.md |
| Iterative solver settings | ITER.md |
| Nonlinear analysis parameters | NLPARM.md |
| Combining load sets (incl. gravity) | LOAD.md |
| Pressure on surface defined by grids | PLOAD.md |
| Distributed/concentrated load on bar or beam | PLOAD1.md, CBAR.md or CBEAM.md |
| Uniform pressure on shell elements | PLOAD2.md |
| Pressure on shells or solid element faces | PLOAD4.md |
| Concentrated force by direction vector | FORCE.md, GRID.md |
| Concentrated force between two grids | FORCE1.md, GRID.md |
| Concentrated force via cross product | FORCE2.md, GRID.md |
| Grid point temperatures (thermal load) | TEMP.md, GRID.md |
| Default temperature for all grids | TEMPD.md |
| Multipoint constraint equation | MPC.md, GRID.md |
| Combining MPC sets | MPCADD.md |
| Single-point constraint (with enforced displacement) | SPC.md, GRID.md |
| Single-point constraint on a list of grids | SPC1.md, GRID.md |
| Combining SPC/SPC1 sets | SPCADD.md |
| Select static load set in a subcase (Case Control) | LOAD_Case.md |
| Link static loads to dynamic excitation (legacy) | LOADSET.md |
| Select nonlinear dynamic forces (transient/harmonic) | NONLINEAR.md |
| Open structure/curve plotter command block | OUTPUT.md |
| Differential stiffness / buckling preload reference | STATSUB.md |
| Define a new subcase | SUBCASE.md |
| Specify analysis type per subcase/step (SOL 400/200) | ANALYSIS.md |
| Select eigenvalue extraction method | METHOD.md, EIGR.md or EIGRL.md |
| Select multipoint constraint set | MPC_Case.md |
| Nonlinear buckling prediction (SOL 400) | NLBUCK.md, NLSTEP_Case.md |
| SOL 400 nonlinear output/debug options | NLOPRM.md |
| Select nonlinear step control (SOL 400) | NLSTEP_Case.md |
| Select single-point constraint set | SPC_Case.md |
| Define a nonlinear step in SOL 400 | STEP.md, ANALYSIS.md |
| Thermal load or temperature-dependent material | TEMPERATURE.md, TEMP.md, TEMPD.md |
