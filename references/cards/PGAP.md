# PGAP — Gap Element Property

Defines the properties of a gap or friction element (CGAP entry).

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PGAP | PID | U0 | F0 | KA | KB | KT | MU1 | MU2 | |
| | TMAX | MAR | TRMIN | | | | | | |

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property identification number |
| U0 | Real; Default = 0.0 | Initial gap opening. Positive = gap open; negative = initial overlap (interference fit) |
| F0 | Real ≥ 0.0; Default = 0.0 | Preload force. Applied along the gap x-axis when the gap is closed |
| KA | Real > 0.0 | Axial stiffness when gap is **closed** (Ua − Ub ≥ U0). Required. |
| KB | Real ≥ 0.0; Default = 10⁻¹⁴ × KA | Axial stiffness when gap is **open** (Ua − Ub < U0). Default is near-zero. |
| KT | Real ≥ 0.0; Default = MU1 × KA | Transverse shear stiffness when the gap is closed |
| MU1 | Real ≥ 0.0; Default = 0.0 | Adaptive gap: static friction coefficient. Nonadaptive gap: friction coefficient in y-transverse direction (μy) |
| MU2 | Real ≥ 0.0; Default = MU1 | Adaptive gap: kinetic friction coefficient. Nonadaptive gap: friction coefficient in z-transverse direction (μz). MU2 ≤ MU1. |
| TMAX | Real; Default = 0.0 | Maximum allowable penetration for adaptive penalty adjustment. TMAX = 0.0 activates adaptive gap. TMAX = −1.0 selects the old nonadaptive gap. |
| MAR | 1.0 < Real < 10⁶; Default = 100.0 | Maximum allowable adjustment ratio for penalty values KA and KT (adaptive gap only) |
| TRMIN | 0.0 ≤ Real ≤ 1.0; Default = 0.001 | Fraction of TMAX defining the lower bound for allowable penetration |

## Key Remarks

- **KA sizing**: for contact problems, KA should be about three orders of magnitude larger than the adjacent structural stiffness. Too large → slow convergence or divergence; too small → inaccurate contact force.
- **Adaptive vs. nonadaptive gap**: TMAX ≥ 0.0 → adaptive (penalty values auto-adjusted to limit penetration). TMAX = −1.0 → nonadaptive (fixed penalty values, no adjustment). The adaptive gap is recommended for most nonlinear analyses.
- When the gap is open, there is no transverse stiffness. When closed with friction, the transverse stiffness is KT until the friction force limit MU1×Fₓ (or MU2×Fₓ) is exceeded and slip occurs.
- If U0 is negative (initial overlap) and GA ≠ GB, the gap closing direction must be controlled by the CID field on the CGAP entry.
- PGAP is a primary property entry — PID must be unique with respect to all other PGAP entries.
- Request element output with FORCE or NLSTRESS Case Control. NLSTRESS also outputs gap STATUS (open/closed/slipping).
