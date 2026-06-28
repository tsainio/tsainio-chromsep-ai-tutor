# Theory Reference

Consult as needed during labs. Not meant to be read in full.

---

## Core Concepts

### Linear vs. Nonlinear Chromatography

| Aspect | Linear (Analytical) | Nonlinear (Preparative) |
|--------|---------------------|-------------------------|
| Concentration | Dilute | High (overloaded) |
| Isotherm | q = H × C | q = qmax × K × C / (1 + K × C) |
| Peak shape | Symmetric | Asymmetric (tailing) |
| Goal | Detection | Purification |

### The Langmuir Isotherm

```
q = (qmax × K × C) / (1 + K × C)

qmax = saturation capacity (g/L)
K    = binding affinity (L/g)
H    = qmax × K  (Henry constant, slope at low C)
```

At low C: linear (q ≈ H × C). At high C: saturation (q → qmax).

### Key Relationships

- **Henry constant:** H = qmax × K
- **Selectivity:** α = H₂/H₁ (ratio of retention)
- **Rule of thumb:** α > 1.5 for preparative separation

### Peak Broadening from Isotherm Nonlinearity

A compound with higher H operates further from the linear region of the Langmuir isotherm at any given feed concentration. This means there is a greater difference between the propagation velocities of its high-concentration and low-concentration zones. The high-concentration front moves faster (less retained because the resin is locally closer to saturation), while the low-concentration tail moves slower (fully retained in the linear regime). Axial dispersion and mass transfer resistance also contribute to broadening, and both effects act simultaneously, but under preparative loading this concentration-dependent velocity spread is the dominant cause of the wide, asymmetric peaks seen for more strongly retained compounds. Dispersion and mass transfer affect all compounds roughly equally, so they cannot explain why one compound's peak is broader than another's — isotherm nonlinearity can.

---

## Model Limitations

The simulator assumes:
- Uniform packing (reality: wall effects, heterogeneity)
- No extra-column effects that affect separation (reality: tubing, fittings add dispersion)
- Langmuir isotherm (reality: may deviate)
- Isothermal (reality: heat of adsorption)
- Lumped kinetics (reality: multiple mass transfer resistances)

---

## Parameter Ranges

| Parameter | Typical Range | Units |
|-----------|---------------|-------|
| Flow rate | 1-20 | BV/h |
| Feed volume | 1-50 | % CV |
| Feed concentration | 1-100 | g/L |
| Langmuir qmax | 10-100 | g/L |
| Langmuir K | 0.01-1.0 | L/g |

---

## Formulas and notation

| Symbol | Meaning | Units |
|--------|---------|-------|
| F | Flow rate | mL/min |
| CV | Column volume | mL |
| d | Column inner diameter | cm |
| L | Column length | cm |
| C | Mobile phase concentration | g/L |
| q | Stationary phase loading | g/L |
| qmax | Maximum stationary phase capacity | g/L |
| K | Langmuir binding affinity constant | L/g |
| H | Henry constant (linear retention) | — (dimensionless) |
| α | Selectivity between two compounds | — |
| ε | Column porosity (void fraction) | — (dimensionless) |
| t₀ | Dead time (CV × ε / F) | min |
| τ | Residence time (CV/F) | min |
| BV/h | Bed volumes per hour — the unit flow rate is specified in (`flow_rate_bv_h`) | h⁻¹ |
| NTP | Number of theoretical plates (column efficiency) | — |
| HETP | Height equivalent to a theoretical plate (L/NTP) | cm |


```
Column volume:   CV = π × d² × L / 4
Henry constant:  H = qmax × K
Selectivity:     α = H₂/H₁
Dead time:       t₀ = CV × ε / F
Residence time:  τ = CV/F
BV/h:            (F in mL/min × 60) / CV in mL
NTP:             μ₁² / μ₂  (statistical moments method, linear conditions only)
HETP:            L / NTP
Elution volume:  V_EL = V_FF + ε + (1−ε) × H₂ + 2
                 (in CV; V_FF = feed volume in CV; H₂ = Henry constant of most retained compound; +2 CV safety margin)
```


---

## Troubleshooting

| Problem | Solution |
|---------|----------|
| Peaks don't elute completely | Recalculate eluent using V_EL formula above |
| Complete overlap | Reduce feed volume; check α |
| Simulation fails | Check parameter values are reasonable |
| Lost track of which run is which | `list_runs` returns every stored run in the session — its `run_id`, label, parameters, and peak summary — so you can re-plot or re-analyze a previous run by `run_id` instead of re-running it. (Requires the session_id.) |
| Lost the session_id itself | `list_runs` cannot recover it — it needs the session_id as input, and there is no tool that enumerates sessions. The only remedy is to start over: call `init_session` for a fresh session, then re-set **all** parameters (column and both compounds) before running any simulation — a new session starts empty, so a run before reconfiguring will use defaults, not your intended setup. |

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
