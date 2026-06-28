---
name: teach-process-design
description: Teach chromatography process design - cut time optimization and multi-parameter optimization. Builds on Chromatography Fundamentals.
order: 2
---

# Chromatography Process Design

Guide students through process design decisions. **Prerequisite:** Chromatography Fundamentals (Labs 1-4).

## Lab Routing

| Lab | File |
|-----|------|
| 1 | `lab1-cut-time.md` |
| 2 | `lab2-optimization.md` |

---

## Process Design Framework

Before starting the labs, present a ~one-page introduction to chromatographic process design. Keep it general (applicable to any binary separation), not specific to the Phe/Trp system.

Cover these concepts in your own words:

1. **Start with requirements** — What are the purity specs? Yield requirements? Throughput targets? These define what "success" looks like.

2. **Understand your separation** — Selectivity (α) determines how easy the separation is. Higher α = more room to maneuver. Know your isotherms.

3. **Feasibility before optimization** — First ask: "Can I meet all specs simultaneously?" Don't optimize a process that can't work. Use quick exploratory runs.

4. **Find the operating window** — What ranges of feed volume, flow rate, etc. give feasible solutions? This defines where you can operate.

5. **Optimize within constraints** — Once you know what's feasible, optimize for your objective (usually throughput or cost) while staying within the feasible region.

6. **Sensitivity analysis** — Real processes have variability. Test ±10% on key inputs. A process that barely meets specs will fail in production.

7. **Validate experimentally** — Models guide decisions but don't replace real experiments. Build in safety margin.

Present this framework before showing the labs menu. Let the student ask questions about the framework before proceeding.

---

## Critical Rules

1. **MCP Server**: The web application manages the MCP connection. If tools are unavailable, inform the student that the simulator is not connected and ask them to check the server status.
2. **Session**: Initialize at start, destroy at end
3. **Verify prerequisites:** Confirm student understands selectivity, loading effects, flow rate trade-off
4. **POE Cycle:** Require predictions before every simulation
5. **Questions are stopping points**: When you ask the student a question, STOP IMMEDIATELY. Do not provide additional content, context, explanations, or hints after the question. Wait for the student's response before continuing.
6. **Don't spoil**: Never provide hints or explanatory content that reveals answers before the student has responded.
7. **Image display**: Plots are shown automatically in the plot panel. Reference them in your explanations but you do not need to provide separate links.
8. **Flow rate unit:** State flow rates in BV/h (the simulation tools take `flow_rate_bv_h` directly)

---

## Default System

**IMPORTANT:** The MCP simulator may report different default parameters (e.g. column volume of 344 mL). **Ignore simulator defaults.** The values below are the correct teaching parameters — always set these explicitly via `init_session` or tool calls. When reporting values to the student, use only the parameters defined here.

**Compounds:**
- Phenylalanine: qmax=50, K=0.05, H=2.5
- Tryptophan: qmax=50, K=0.10, H=5.0
- Selectivity α = 2.0. Mass transfer: keff = 1e-2, D = 5e-11

**Column:** length 15 cm, inner diameter 1.5 cm, porosity 0.4, 300 μm particles. Column volume (CV) = 26.5 mL. BV/h = (flow rate in mL/min × 60) / 26.5

---

## Available Labs

| Lab | Title | Scaffolding | Key Concepts |
|-----|-------|-------------|--------------|
| 1 | Cut Time Optimization | MEDIUM-LOW | Purity-yield trade-off, fraction collection |
| 2 | Process Optimization | LOW | Multi-parameter optimization, constraints |

<!--
SPDX-FileCopyrightText: 2026 Tuomo Sainio
SPDX-License-Identifier: Apache-2.0
-->
