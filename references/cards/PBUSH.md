# PBUSH — Generalized Spring-and-Damper Property

Defines the nominal property values for a generalized spring-and-damper structural element.

## Format

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
|---|---|---|---|---|---|---|---|---|---|
| PBUSH | PID | "K" | K1 | K2 | K3 | K4 | K5 | K6 | |
| | | "B" | B1 | B2 | B3 | B4 | B5 | B6 | |
| | | "GE" | GE1 | GE2 | GE3 | GE4 | GE5 | GE6 | |
| | | "RCV" | SA | ST | EA | ET | | | |
| | | "M" | M | | | | | | |
| | | "T" | ALPHA | TREF | COINL | | | | |

Lines with "K", "B", "GE", "RCV", "M", "T" may appear in any order and are all optional.

## Fields

| Name | Type | Description |
|---|---|---|
| PID | Integer > 0 | Property identification number |
| Ki | Real; Default = 0.0 | Stiffness values in element directions 1–6 |
| Bi | Real; Default = 0.0 | Damping (force per velocity) in directions 1–6 |
| GEi | Real; GE1 default = 0.0 | Structural damping constants in directions 1–6 |
| SA, ST | Real; Default = 1.0 | Stress recovery coefficients: translational (SA) and rotational (ST) |
| EA, ET | Real; Default = 1.0 | Strain recovery coefficients: translational (EA) and rotational (ET) |
| M | Real ≥ 0.0; Default = 0.0 | Lumped mass of the CBUSH element |
| ALPHA | Real; Default = 0.0 | Thermal expansion coefficient |
| TREF | Real; Default = 0.0 | Reference temperature for thermal loads |
| COINL | Real ≥ 0.0; Default = 0.0 | Length of a CBUSH with coincident grids (used for thermal expansion) |

## Key Remarks

- Directions 1–3 are translational, 4–6 are rotational in the element coordinate system
- Ki/Bi/GEi can be made frequency-dependent via a PBUSHT entry (used by CBUSH in SOL 108/111)
- Nominal values are used for all analysis types except frequency response and nonlinear analysis
- If only GE1 is specified (GE2–GE6 blank): GE1 is applied to all Ki — this is a special flag, not GE = 0.0
- Element stresses: σ_i = F_i × SA (translational), σ_i = M_i × ST (rotational)
- Element strains: ε_i = U_i × EA (translational), ε_i = θ_i × ET (rotational)
