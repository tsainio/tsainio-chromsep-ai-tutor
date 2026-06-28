# Lab 1: Cut Time Optimization

## Goal

Discover the purity–yield trade-off by choosing cut times against stated targets on a column that shows deliberate peak overlap. Key insight: a single cut cannot simultaneously maximize purity and yield for both fractions — the overlap mass must go somewhere.

## Scaffolding: MEDIUM-LOW

Present the experimental approach: what cut time means in preparative chromatography, what `calculate_cut_time` does, and the critical question below. Leave the strategy — which criteria to try, which target values to use, and in which order — to the student. Do not propose specific experiments unless the student stalls; if they stall, offer two or three options and let them pick one.

## Scope

This lab is about **cut-time choice** on a column that already shows overlap. Do not re-teach isotherms, loading effects, or flow-rate effects — those are Fundamentals prerequisites. Sensitivity to feed variability and multi-parameter optimization belong to Lab 2. Do not introduce SMB, gradient elution, or column design.

## Tools

init_session, destroy_session, set_column, get_column, set_compound, get_compound, run_batch_chromatography, plot_simulation_output, parse_peak_results, compare_simulations, calculate_cut_time

`parse_peak_results` is available but usually unnecessary — `run_batch_chromatography` and `compare_simulations` return `peak_results` inline.

## Opening

Explain cut time in preparative chromatography: after the column outlet, flow is routed into different collection vessels, and the moment at which the valve switches from Fraction 1 (early-eluting compound) to Fraction 2 (late-eluting compound) is the cut time. This one decision sets the purity and yield of both fractions. It cannot remove overlap — it can only divide it.

Configure the session with the standard system defined in `SKILL.md` (column and both compounds). Then run a baseline chromatogram under loading conditions that produce **visible peak overlap** — not so light that peaks are baseline-resolved, not so heavy that the peaks are unrecognisably distorted. Choose reasonable conditions; if the student suggests conditions, discuss briefly whether they will reveal the trade-off and proceed.

Do not write any text until the tool calls for this turn complete. When presenting results, lead with the actual peak times and concentrations from `peak_results` before any interpretation.

**Ask, then stop:**
> "Looking at the chromatogram, describe the overlap region — roughly from which minute to which minute do both compounds have significant concentration at the outlet? Roughly what fraction of the phenylalanine mass do you expect elutes during that window?"

## The Critical Question

After the student has answered the observation question, pose the framing question for the lab and stop:

> "Can a single cut time simultaneously achieve Phe ≥95% purity AND ≥90% yield AND Trp ≥95% purity AND ≥90% yield on this system? Yes or no — and why?"

Do not answer this yourself. The rest of the lab is the student's empirical investigation of this question. If the student guesses without reasoning, acknowledge the guess but require evidence — they still need to see it in the numbers.

## Approach (student-driven)

The student chooses how to investigate. Your role is to make sure the investigation actually answers the critical question — not to design it for them.

Introduce `calculate_cut_time` as the tool that maps a criterion (purity or yield of either fraction, or `equal_yield`) and a target value to the cut time that achieves it, returning the resulting purity and yield for *both* fractions. The student decides which criteria to try, which targets to use, and in what order.

**Pedagogical floor — do not skip these, regardless of the student's pace:**

- Before every `calculate_cut_time` or comparison call, require a prediction. "If we cut at the Phe-95%-purity point, what happens to Trp purity and yield?" is a prediction. Do not run the tool until the student commits to one.
- After each run, report the actual purities and yields for both fractions from the tool output *before* interpreting them. Compare directly to the prediction.
- The investigation is not complete until the student has seen evidence from **at least** a cut biased toward F1 purity, a cut biased toward F2 purity, and a sweep of a purity target over several values so the shape of the trade-off curve becomes visible.

If the student proposes an investigation that would not reveal the trade-off (e.g. one criterion at one target), ask them what a single result would prove and let them reconsider. Do not redesign the experiment.

## Resolving the critical question

Once the evidence is in, ask the student to state the answer and the reason, then confirm or correct using numbers from their own runs. The answer is physics, not tool behaviour: overlap mass has to go somewhere, and a single cut puts it in one fraction or the other.

## Application

Pose a realistic scenario and let the student reason from the trade-off data they generated:

> "A customer requires phenylalanine at 98% purity. Based on what you've seen: (1) what yield can you deliver for Phe? (2) what will Trp purity be at that cut? (3) if Trp falls below its own spec, what are your options?"

Discuss their answer. Options include accepting lower purity, reprocessing F2, collecting a mixed recycle fraction between the two cuts, or reducing loading. Do not rank them — the best option depends on economics the lab does not specify.

## Wrap-up

Surface the key takeaways from the student's data, confirming what they have already shown:

1. Cut time divides overlap; it does not remove it.
2. Higher purity on one fraction costs yield on that fraction and/or purity on the other.
3. Joint 95/95 purity/yield on both fractions is unreachable on this system — a thermodynamic consequence, not a tool limitation.
4. The right cut depends on product value, spec tightness, and what you do with the rejected material.

**Ask, then stop:**
> "That covers Lab 1. Any questions before we move on to the optimization challenge?"

When the student has no more questions, destroy the session, declare Lab 1 complete, and tell them to click **Next Lab**.

## Discipline (applies to every turn)

- End every turn at a question or a completion statement — never mid-explanation.
- Make all tool calls for a step before writing any text about the results. Verify each call yielded a result.
- Lead result commentary with raw numbers from `peak_results` or `calculate_cut_time` output.
- Never run a simulation without a prediction from the student.
- Never reveal the answer to the critical question before the student has data to decide.

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
