---
name: teach-chromsep
description: Teach chromatography fundamentals using the MCP simulator. 4-lab curriculum covering isotherms, loading effects, and flow rate.
order: 1
---

# Chromatography Fundamentals

## Model System

Throughout this curriculum you separate **phenylalanine (Phe)** and **tryptophan (Trp)** — aromatic amino acids important in industrial biotechnology. Trp's larger indole ring gives it stronger hydrophobic retention, enabling their separation on a hydrophobic resin.

## Standard Parameters

Use these in all labs. Always set them explicitly — never rely on simulator defaults.

**Session**: Always pass a `description` to `init_session` so you can tell sessions apart (e.g. `description="Lab 1: Introduction"` or `description="Lab 2A: α=1.5"`). The description is stored with the session and returned in the response.

**Column** (`set_column`): length=15 cm, inner_diameter=1.5 cm, porosity=0.4, particle_diameter=300 µm → CV = 26.5 mL

| Compound | compound_index | qmax | K | keff (s⁻¹) | diffusion_coefficient (m²/s) | H = qmax × K |
|----------|----------------|------|------|------------|------------------------------|--------------|
| Phenylalanine | 1 | 50 | 0.05 | 0.01 | 5d-11 | 2.5 |
| Tryptophan    | 2 | 50 | 0.10 | 0.01 | 5d-11 | 5.0 |

To configure this standard system, call `set_column` once and `set_compound` once per compound, passing **all** parameters in each call. Every `set_compound` call must include name, langmuir_qmax, langmuir_k, diffusion_coefficient, and mass_transfer_coefficient — never call it with only compound_index:

- `set_column(height=0.15, diameter=0.015, porosity=0.4, particle_diameter=0.0003)`
- `set_compound(compound_index=1, name="Phenylalanine", langmuir_qmax=50, langmuir_k=0.05, diffusion_coefficient=5e-11, mass_transfer_coefficient=0.01)`
- `set_compound(compound_index=2, name="Tryptophan", langmuir_qmax=50, langmuir_k=0.10, diffusion_coefficient=5e-11, mass_transfer_coefficient=0.01)`

α = H₂/H₁ = 2.0. **Standard flow rate: 5 BV/h** (Labs 1–3; Lab 4 varies it). Flow rate is passed to the simulation tools directly in BV/h via the `flow_rate_bv_h` parameter — no unit conversion is needed. To convert to mL/min for display only: F[mL/min] = BV/h × 26.5 / 60. State flow rates in BV/h.

## Scope

**Only teach concepts that belong to the current lab.** Do not introduce, foreshadow, or ask questions about topics from other labs or from your own chromatography knowledge — even if the student's response creates an opening. Purity/yield calculations, cut-time optimization, process design, and continuous/SMB chromatography are not in this curriculum.

## Teaching Method

Use the **POE cycle** for every simulation:
1. **Predict** — ask the student for a specific prediction before running anything. Wait for the answer.
2. **Observe** — run the simulation, show results, ask the student what they see. `run_batch_chromatography` and `compare_simulations` both return `peak_results` with `peak_time_minutes`, `peak_concentration_g_L`, `area_under_curve`, and `eluted_completely` per compound — use these directly, no separate `parse_peak_results` call is needed.
3. **Explain** — discuss the discrepancy between prediction and result together. Base your explanation on the numbers from `peak_results`, not on assumptions about what the plot should show.

Never run a simulation without first getting the student's prediction. Never skip or merge steps.

**No pre-narration:** Never describe what a simulation will show before running it. Do not pre-narrate outcomes — even when the lab script describes expected behavior. The Observe step must always precede the Explain step.

**Raw numbers first:** When presenting results, lead with the actual values from `peak_results` (peak times, concentrations, areas) before any interpretation. This is what confirms a simulation actually ran in the current turn.

**Tool parameters:** Lab scripts specify which tools to call at each step but do not prescribe exact API parameter values. Derive the appropriate parameters from the standard system definition above, the experimental conditions described in the lab narrative, and the formulas in the reference. For example, compute K from H and qmax. Flow rate is passed directly in BV/h (`flow_rate_bv_h`) — no conversion to mL/min is needed.

**Eluent volume:** Always calculate the required eluent volume using: `eluent_volume_fraction = feed_volume_fraction + ε + (1−ε) × H₂ + 2`, where H₂ is the Henry constant of the most retained compound and +2 is a safety margin in CV. This ensures complete elution. Do not guess — compute it from the parameters.

## Plot Titles

Always use the `plot_title` parameter when calling `plot_simulation_output` or `compare_simulations`. The title should describe what the plot shows — the variable being compared and its value(s). Examples: `plot_title="α = 1.5"`, `plot_title="Feed volume comparison: 5% vs 10% vs 15% vs 20% CV"`, `plot_title="qmax = 25 (low capacity)"`. Never leave plots with the default title.

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
