# Lab 4: Flow Rate Effects

## Goal

Show how flow rate affects column efficiency (NTP) and separation quality (overlap) through mass transfer limitations under linear conditions. Then demonstrate that under nonlinear conditions, the same flow rate changes have a smaller effect on overlap — because isotherm nonlinearity, not mass transfer, dominates peak broadening.

## Tools

init_session, destroy_session, set_column, get_column, set_compound, get_compound, run_batch_chromatography, plot_simulation_output, parse_peak_results, compare_simulations, calculate_plate_count, calculate_overlap

## NTP validity

NTP (number of theoretical plates) is only meaningful under **linear conditions** — when isotherm nonlinearity is negligible. In Part A we use very low loading to ensure this. **Do not call `calculate_plate_count` on Part B runs** — at higher loading, peak broadening is dominated by isotherm nonlinearity (Lab 3), and the plate count would be a meaningless composite number. If the student asks about NTP at high loading, explain that it is only valid in the linear regime.

## Approach

### Part A: Column efficiency at different flow rates

**Opening turn** (single response — text, then tool calls, then text):

1. **Explain** (text): Introduce the lab — we're investigating what happens when we change how fast liquid flows through the column. In Lab 1 we saw that flow rate shifts peak positions in time but doesn't change relative retention. Now we'll look at a second effect: how flow rate affects peak sharpness through mass transfer.

   Remind the student of **dead time** (t₀) from Lab 1 — the time a volume element of liquid actually spends inside the column: t₀ = CV × ε / F (void volume divided by flow rate). Present the three flow rates we'll test:

   | BV/h | t₀ |
   |------|----|
   | 2  | 12.0 min |
   | 5  | 4.8 min |
   | 10 | 2.4 min |

   Explain the physical picture: adsorption and desorption between the liquid and the resin take time. In our system, the mass transfer coefficient keff = 0.01 s⁻¹ sets an equilibration timescale of roughly 100 seconds. At 2 BV/h the liquid spends 12 minutes in the column — about 7× the equilibration time — so molecules have plenty of time to reach local equilibrium with the resin. At 10 BV/h, liquid transit time drops to 2.4 minutes — only 1.4× the equilibration time — so equilibration is much less complete.

2. **Initialize** (tool calls, immediately): Set up a session for this lab with the standard column and both compounds.

   **Required tools:** `init_session`, `set_column`, `set_compound`

3. **Ask prediction** (text): Ask and stop:

   > "We'll run at all three flow rates with 15% CV feed at 2 g/L — low concentration keeps us in the linear regime where isotherm nonlinearity is negligible, so any peak broadening comes mostly from mass transfer. How do you expect the **relative peak width** (peak width divided by retention time) to change as flow rate increases from 2 to 10 BV/h: stay the same, increase, or decrease? Why?"

**After prediction**:

For each of the three flow rates: run the batch simulation with the light-loading conditions from the question, then calculate the plate count and overlap for that run — before moving to the next flow rate. After all individual runs complete, generate a comparison overlay plot of all three flow rates. Do not write any text until all tool calls complete.

**Required tools:** `run_batch_chromatography`, `calculate_plate_count`, `calculate_overlap` (called for each of the three flow rates), then `compare_simulations` (once, for the overlay)

Then present the results in a single table (average NTP of the two compounds for each flow rate):

> | BV/h | NTP | Overlap |
> |------|-----|---------|

Compare the actual values against the student's prediction. Explain: NTP captures column efficiency — how many equilibrium stages the column provides. At low NTP (high flow), peaks are broader relative to their retention, and even at this light loading we start to see some overlap. Ask and stop:

> "Why does faster flow reduce the number of effective equilibrium stages?"

**After student answers**: Confirm: at higher flow the liquid passes through each section of the column faster, so molecules have less time to equilibrate between the liquid and the resin. Each "stage" is less efficient, which means more stages (more column length) would be needed to achieve the same peak sharpness. This is purely a kinetic effect — the thermodynamics (selectivity, Henry constants) are unchanged.

---

### Part B: Flow rate effects under nonlinear conditions

**Opening for Part B** (single response — text, then tool calls, then text):

1. **Explain** (text): In Part A we used very light loading to isolate the mass transfer effect. We saw that increasing flow rate reduces column efficiency and increases overlap. Now we'll see what happens under heavy loading — same 15% CV feed volume, but at 20 g/L instead of 2 g/L, where isotherm nonlinearity strongly affects peak shape (as we saw in Lab 3).

   Note: we cannot calculate NTP here because the peaks are no longer gaussian under nonlinear conditions. NTP is only meaningful in the linear regime. Instead, we'll use overlap as our measure of separation quality, so we can directly compare Parts A and B.

   Let's start by running just the middle flow rate — 5 BV/h — to see what heavy loading does.

2. **Run 5 BV/h** (tool calls, immediately): Run the simulation at 5 BV/h with 15% CV feed at 20 g/L, plot the results, and calculate the overlap. Do not write any text until the tool calls complete.

   **Required tools:** `run_batch_chromatography`, `plot_simulation_output`, `calculate_overlap`

3. **Present and ask prediction** (text): Report the overlap at 5 BV/h under heavy loading, and remind the student what it was at 5 BV/h in Part A (linear). The jump is dramatic. Then ask and stop:

   > "At 5 BV/h, overlap jumped from (Part A value) under linear conditions to (Part B value) under heavy loading. Now we'll run the other two flow rates — 2 BV/h and 10 BV/h — at the same heavy loading. In Part A, slowing to 2 BV/h eliminated the overlap entirely. Do you expect 2 BV/h to eliminate overlap here too? And how much worse do you expect 10 BV/h to be? Make a prediction for both."

**After prediction**:

For each of the two remaining flow rates (2 BV/h and 10 BV/h): run the simulation at heavy loading (same conditions as the 5 BV/h run, just different flow rate) and calculate the overlap — before moving to the next flow rate. Then generate a comparison overlay of all three heavy-loading runs. Do not write any text until all tool calls complete.

**Required tools:** `run_batch_chromatography`, `calculate_overlap` (called for each of the two remaining flow rates), then `compare_simulations` (once, for the overlay)

Then present the full comparison table:

> | BV/h | Overlap (Part A — linear) | Overlap (Part B — nonlinear) |
> |------|---------------------------|------------------------------|
> | 2    | (Part A value)            | (actual %)                   |
> | 5    | (Part A value)            | (actual %)                   |
> | 10   | (Part A value)            | (actual %)                   |

Compare against the student's prediction. Highlight two key observations: (1) at 2 BV/h, overlap didn't disappear — unlike in the linear case, slowing down can't eliminate the broadening from isotherm nonlinearity; (2) at 10 BV/h, the gap between linear and nonlinear narrows because both are already heavily overlapped. The underlying reason: under nonlinear conditions, the dominant source of peak broadening is the isotherm itself (the "shark fin" shapes from Lab 3), not mass transfer. Improving mass transfer by slowing the flow still helps, but it's addressing a smaller fraction of the total broadening. Ask and stop:

> "Imagine you're running a production process and you see significant overlap. Based on what we've learned in Labs 3 and 4, how would you decide whether to reduce the flow rate or reduce the loading to fix it?"

**After student answers**: Confirm: the answer depends on what's causing the broadening. If peaks are symmetric and gaussian (linear regime), the overlap is likely from poor column efficiency — reducing flow rate should help. If peaks have the characteristic shark-fin shape (nonlinear regime), the broadening is dominated by isotherm effects — reducing loading (feed volume or concentration) will be more effective than slowing the flow. In practice, you'd look at the peak shapes first to diagnose which regime you're in, then choose your lever accordingly.

Ask and stop:

> "That covers everything in Lab 4 and the full curriculum. Do you have any questions before we finish?"

When the student says they have no more questions: Destroy the session. Indicate Lab 4 is complete and the full curriculum is finished. Congratulate the student on completing all four labs.

**Required tools:** `destroy_session`

## When you're done each turn

End exactly at a question — stop there. Do not call `destroy_session` until the student confirms they have no more questions.

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
