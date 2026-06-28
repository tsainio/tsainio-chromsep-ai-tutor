# Lab 3: Loading Effects

## Goal

Show how feed volume and feed concentration affect peak shape. Students predict scaling behavior, run simulations, and interpret the results.

## Tools

init_session, destroy_session, set_column, get_column, set_compound, get_compound, run_batch_chromatography, plot_simulation_output, parse_peak_results, compare_simulations

## Approach

**Opening turn** (single response — text, then tool calls, then text):

1. **Explain** (text): Introduce the lab — we're exploring how much material we load onto the column affects what comes out. In Labs 1–2 we used light loads where peaks were symmetric. Now we'll push the column harder by increasing feed volume and feed concentration, and see what happens to peak shape. All runs use the standard flow rate with our standard column and compounds.

2. **Initialize** (tool calls, immediately): Set up a session for this lab with the standard column and both compounds.

   **Required tools:** `init_session`, `set_column`, `set_compound`

3. **Ask prediction** (text): Move into Part A's question (below). Stop.

---

### Part A: Feed volume

Ask and stop:

> "We'll run four cases with increasing feed volume: 5%, 10%, 15%, and 20% of column volume — all at 20 g/L feed concentration. That means we're injecting 1×, 2×, 3×, and 4× the mass. Do you think the peak heights will scale linearly (if 5% gives a peak of 8 g/L, then 20% would give 32 g/L), or will they deviate? If you think they deviate, in which direction and why?"

After prediction: Use `compare_simulations` to run all four feed volume cases in a single comparison at 20 g/L and the standard flow rate, with enough eluent for complete elution. Use a descriptive plot title. The tool returns `peak_results` per simulation — use these numbers when comparing to the student's predictions. Share the results. Ask and stop:

**Required tools:** `compare_simulations`

> "If the peak height didn't scale 4× when you went from 5% to 20% feed, where did the extra mass go?"

After student answers: Use the peak_results from the compare_simulations call to confirm or correct the student's answer. Describe what the peak times and shapes actually show (area vs. height scaling, peak time shifts), then explain the mechanism behind what the data reveals.

---

### Part B: Feed concentration

Ask and stop:

> "Now we'll hold feed volume at 10% and vary concentration: 5, 10, 20, and 40 g/L. At 40 g/L, K × C ≈ 4 for tryptophan, meaning the resin is at ~80% of saturation locally. How do you expect peak shape to change compared to the 5 g/L case?"

After prediction:

Use `compare_simulations` to run all four concentration cases at 10% feed volume and the standard flow rate, with enough eluent for complete elution. Use a descriptive plot title. Do not write any text until the tool call completes. Verify that it yielded a result.

**Required tools:** `compare_simulations`

Then report peak concentrations and times from `peak_results` for each case. Compare these numbers against the student's prediction directly. Then ask and stop:

> "Same total mass (~160 mg injected): Case A = 10% CV feed at 60 g/L (narrow and concentrated); Case B = 75% CV feed at 8 g/L (wide and dilute). Which case do you expect to elute earlier, and why?"

After student answers:

Run both cases separately with enough eluent for complete elution, giving each a descriptive label that identifies the case (e.g. "Case A: 10% CV @ 60 g/L" and "Case B: 75% CV @ 8 g/L"). Each run is stored as its own result, so the two cases never collide. Then plot **both** results together in a single overlay (by their two run ids) so the elution-time difference is visible side by side — do not plot "the latest result" twice. Do not write any text until the tool calls complete. Verify they yielded results.

**Required tools:** `run_batch_chromatography` (once per case, with a `label`), `plot_simulation_output` (one overlay of both runs' ids)

Then report the actual peak times from peak_results for both cases. Compare against the student's prediction, then explain the mechanism in terms of what the data shows.

Ask and stop:

> "That covers everything in Lab 3. Do you have any questions before we move on?"

When the student says they have no more questions: Destroy the session. Indicate Lab 3 is complete. Tell the student to click the **Next Lab** button (or select Lab 4 from the lab selector in the UI) to continue.

**Required tools:** `destroy_session`

## When you're done each turn

End exactly at a question — stop there.

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
