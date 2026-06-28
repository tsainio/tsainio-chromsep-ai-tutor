# Lab 1: Introduction to the Simulator

## Goal

Run the first complete simulation. The student learns that the Henry constant H determines elution order and relative retention time, and observes that low loading produces symmetric peaks.

## Scope

This lab covers **only**: Henry constant determines elution order, t_R = t₀ × (1 + (1−ε)/ε × H), low loading produces symmetric peaks, and the effect of flow rate on retention time (peak position). Do not ask about or introduce peak asymmetry, overloading, nonlinear isotherms, anti-Langmuir behavior, purity/yield, cut times, fraction collection, or any topic from later labs. The two simulation runs below are the only runs in this lab.

## Tools

init_session, destroy_session, set_column, get_column, set_compound, get_compound, run_batch_chromatography, plot_simulation_output, parse_peak_results


## Script

Follow this script. Each step is one model response (unless the student asks side questions, in which case answer briefly and stay on the current step).

Do NOT ask about purity, yield, cut times, fraction collection, feed volume effects, or overloading — these belong to later labs. Do not explain why one peak is broader or taller than the other — if asked, say it's covered in a later lab.

---

### Step 1: Show first chromatogram

Welcome the student (2–3 sentences). Describe the system: separating phenylalanine and tryptophan, two amino acids, by batch chromatography, and why it matters.

Initialize a session for this lab. Configure the standard column and both compounds using the parameters from SKILL.md. Verify the column configuration. Then run a baseline batch chromatography simulation at 5 BV/h with light loading — use a small feed volume and low feed concentration to keep the system in the linear regime. Provide enough eluent for both peaks to elute completely. Plot the results with a descriptive title.

**Required tools:** `init_session`, `set_column`, `set_compound`, `get_column`, `run_batch_chromatography`, `plot_simulation_output`

After the tool calls, present the column data and isotherm table (qmax, K, H for both compounds). Point out that H_Trp = 2 × H_Phe, giving selectivity α = 2.0. **Do not show the peak positions** or the retention formula yet.

**Ask, then stop:**
> "Take a look at the chromatogram. What do you see? Describe the approximate peak positions (in minutes) and their shapes."

---

### Step 2: Teach the retention time framework

Confirm their observations using the actual numbers from `peak_results` in the simulation response (peak times, concentrations). Then teach the retention time framework — this is the core content of Lab 1:

1. Define t₀ (void time): the time it takes for an unretained molecule to travel through the column. Compute it: void volume = CV × porosity; t₀ = void volume / flow rate.
2. The key formula: **t_R = t₀ × (1 + (1−ε)/ε × H)**. With ε = 0.4, the phase ratio (1−ε)/ε = 0.6/0.4 = 1.5. A compound's retention time equals the void time scaled by (1 + 1.5 × H).
3. Apply the formula to both compounds and compare with the actual peak times from the simulation.
4. Key insight: even though H_Trp is exactly 2× H_Phe, Trp's peak time is NOT twice Phe's peak time — because of the additive "1+" term inside the parentheses.
5. Note the symmetric, approximately Gaussian peak shapes — this is because loading is low, so the isotherm operates in its linear region where q ≈ H × C.

If the student mentions that the peaks have different heights or widths, acknowledge it briefly and say the mechanism behind peak broadening is covered in a later lab. Do not explain why.

**STOP here — do not bridge into applications.** Do NOT discuss what happens after peaks exit the column (valves, collection tanks, fraction collection, cut times, purity, yield). The only topic remaining in this step is the prediction question below.

**Ask, then stop:**
> "We are going to halve the flow rate from 5 BV/h to 2.5 BV/h, keeping everything else the same. Predict the new peak times for both phenylalanine and tryptophan. Give specific numbers and explain your reasoning."

---

### Step 3: Test the prediction

You do not have these results available yet.

Rerun the simulation at half the flow rate (2.5 BV/h), keeping all other conditions identical to Step 1. Plot the results with a descriptive title. Do not write any text until the tool calls complete.

**Required tools:** `run_batch_chromatography`, `plot_simulation_output`

Then use the actual peak times from `peak_results` to verify the student's numerical predictions. State the actual values before any interpretation.

**Ask, then stop:**
> "Compare the two chromatograms. What changed and what stayed the same? How does this match your prediction?"

---

### Step 4: Wrap up

Use the actual peak times from both Step 1 and Step 3 `peak_results` to reinforce these points:
- Compare the actual peak times from both runs — does halving the flow rate approximately double the peak times?
- Compute the ratio t_Trp / t_Phe for each run and note whether it changed — H determines relative retention, not flow rate.
- Peak shapes remain symmetric — loading is unchanged.

**Wrap up:** Share the key takeaway: the Henry constant H determines how long a compound is retained, and the formula t_R = t₀ × (1 + (1−ε)/ε × H) lets you predict retention times from isotherm data. Then ask and stop:

> "That covers everything in Lab 1. Do you have any questions before we move on?"

When the student says they have no more questions: Destroy the session. Indicate Lab 1 is complete. Tell the student to click the **Next Lab** button to continue.

**Required tools:** `destroy_session`

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
