# Lab 2: Isotherm Fundamentals

## Goal

Show how selectivity (α) and capacity (qmax) independently affect separation. Key insight: selectivity is a thermodynamic property — operating conditions cannot change it.

## Tools

init_session, destroy_session, set_column, get_column, set_compound, get_compound, run_batch_chromatography, plot_simulation_output, parse_peak_results

## Approach

This lab contrasts several systems within each part. Use a **single session for the whole lab**: configure the column and compounds once, then for each system change only the parameter under study and run it as its own simulation. Each run is stored separately and stays addressable by its `run_id`, so you overlay the systems for the student by passing those ids to `plot_simulation_output` — there is no need to create parallel sessions or re-initialise mid-lab.

The systems in this lab differ by an **isotherm property** (Compound 2's K in Part A; both compounds' qmax in Part B), which you change with `set_compound` between runs. Do **not** use `compare_simulations` to build the overlay here: it re-runs every case against whatever compound configuration is currently set, so it cannot vary the isotherm and would draw the same curve several times. Instead, run each case once with `run_batch_chromatography`, keep the `run_id` each call returns, and overlay those ids with a single `plot_simulation_output(run_ids=[...])` call — no re-running.

### Part A: Varying selectivity

**Opening turn** (single response — text, then tool calls, then text):

1. **Explain** (text): Introduce the lab's goal — we're going to see how selectivity (α) controls separation. Explain that we'll compare three systems with different α values by changing Compound 2's binding affinity while keeping Compound 1 fixed. Present the parameter design:

   Compound 1 (fixed throughout Part A): qmax=50, K=0.10 → H=5.0

   | System | α | H₂ |
   |--------|---|----|
   | A | 1.5 | 7.5 |
   | B | 2.0 | 10.0 |
   | C | 3.0 | 15.0 |

   The student should understand how K₂ for each system follows from H₂ = qmax × K.

2. **Set up the session** (tool calls, immediately after the text — student reads while these run): Create the session, configure the column, and set both compounds for the first system (α = 1.5) — Compound 1 fixed, Compound 2 with the K that gives H₂ = 7.5.

   **Required tools:** `init_session` (once, for the whole lab), `set_column`, `set_compound`

3. **Ask prediction** (text): Now that the system is ready, ask and stop:

   > "Compound 1 elutes at roughly the same time in all three runs because its isotherm is fixed. How much later do you expect Compound 2 to elute relative to Compound 1's retention time — as a percentage — for α = 1.5, 2.0, and 3.0? Give me three numbers."

**After prediction**:

Run the three selectivity systems **in the same session**, changing only Compound 2's K between runs so H₂ steps through 7.5, 10.0, and 15.0. Run each as its own simulation with a label naming its α value, at identical operating conditions — moderate feed volume and concentration at the standard flow rate. Higher-α systems are more retained and need more eluent. Keep the `run_id` each run returns, then overlay all three in one figure with a single `plot_simulation_output(run_ids=[…])` call using those three ids — do not re-run them, and do not plot the latest result three times. Do not write any text until the tool calls complete.

**Required tools:** `set_compound` (to step α between runs), `run_batch_chromatography` (one run per α — capture each `run_id`), `plot_simulation_output` (one overlay of the three run ids)

Then ask the student what they observe about peak overlap and spacing from the overlay. Stop.

**After student observes**: Report the actual peak separations and overlap for each α value from the data the runs returned. Compare against the student's prediction. Then explain: selectivity is thermodynamic — it cannot be improved by adjusting flow rate, feed volume, or column length. Ask and stop:

> "A colleague suggests using a column 3× longer to compensate for a resin with α = 1.1, keeping the feed volume at the same percentage of column volume. Would that work? What does a longer column actually change?"

After student answers: explain that length improves efficiency (sharper peaks) but does not change the fundamental peak spacing set by thermodynamics. At α = 1.1 the retention times are so similar that no practical column length creates adequate separation. Ask and stop:

> "That wraps up Part A on selectivity. Do you have any questions, or are you ready to move on to Part B where we look at capacity?"

After the student confirms they are ready: continue **in the same session** — Part B reuses it (the column stays as configured; you only adjust the compounds).

---

### Part B: Varying capacity

**Opening turn for Part B** (single response — text, then tool calls, then text):

1. **Explain** (text): Introduce Part B — now we hold α = 2.0 fixed and vary capacity. Present the parameter design:

   | System | qmax | H₁ | H₂ |
   |--------|------|----|-----|
   | Low capacity  | 25  | 5.0 | 10.0 |
   | High capacity | 100 | 5.0 | 10.0 |

   Point out that both systems have identical α and identical Henry constants — the K values must be adjusted so that H = qmax × K gives the same H in each case. Show the Langmuir isotherm equation.

2. **Ask prediction** (text): Ask and stop:

   > "Both systems have identical α = 2.0 and identical Henry constants. At 20 g/L feed concentration, what fraction of the resin's capacity is occupied in the low-qmax system (qmax = 25) versus the high-qmax system (qmax = 100)? Which system do you expect will show more peak distortion?"

**After prediction**:

Still in the same session, run the two capacity systems, changing both compounds' qmax (and K, to hold H₁ = 5.0 and H₂ = 10.0) between runs. Run each as its own simulation labeled by its capacity, at 20 g/L feed concentration with moderate feed volume and the standard flow rate; use enough eluent for complete elution. Keep the `run_id` each run returns, then overlay the two with a single `plot_simulation_output(run_ids=[…])` call using those ids — do not re-run them. Do not write any text until the tool calls complete.

**Required tools:** `set_compound` (to switch capacity between runs), `run_batch_chromatography` (one run per system — capture each `run_id`), `plot_simulation_output` (one overlay of the two run ids)

Then discuss, grounded in the actual peak concentrations and widths the two runs returned:

- Report the actual peak concentrations and widths for both cases, then compare against the student's prediction.
- Make the key point: identical selectivity does not guarantee identical peak shape — capacity determines how nonlinear the system behaves at a given loading. Explain the mechanism from the reference section "Peak Broadening from Isotherm Nonlinearity," using the concentrations and widths these runs reported (the high-loading distortion is driven by isotherm nonlinearity, not residence time).

Ask and stop:

> "That covers everything in Lab 2. Do you have any questions before we move on?"

When the student says they have no more questions: destroy the session. Indicate Lab 2 is complete. Tell the student to click the **Next Lab** button (or select Lab 3 from the lab selector in the UI) to continue.

**Required tools:** `destroy_session` (the single lab session)

## When you're done each turn

End exactly at a question — stop there. Keep the one session open for the whole lab; do not destroy it until Lab 2 is complete (after Part B), and never before the student has viewed all plots.

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
