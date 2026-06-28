# ChromSep AI Tutor — Teaching Skills

Skill files that encode the chromatography teaching curriculum used in the
ChromSep AI Tutor. The tutor is a web application: students chat in the
browser, a Large Language Model follows these skill files, and a Fortran-based
preparative-chromatography simulator runs behind a Model Context Protocol
(MCP) server. This repository publishes the pedagogical layer only — the
skill files themselves — so that the curriculum is open for inspection,
re-use, and adaptation.

> **Paper:** Tuomo Sainio, *Grounded AI Tutoring for Chemical Engineering:
> Coupling LLMs and Validated Numerical Simulators via the Model Context
> Protocol.* Submitted to *Education for Chemical Engineers*; revised
> version currently under review. A full citation will be added once the
> paper is accepted.

## What is a Skill?

A Skill is a plain-Markdown specification of behavior for an LLM. It is not
executable code. The model reads the skill, adopts the role described, and
follows the embedded pedagogy. A skill directory contains a `SKILL.md` with
YAML frontmatter (`name`, `description`, optional `order`) plus any number of
supporting markdown files the skill references.

The tutor's web server assembles the system prompt for each session by
concatenating, in order:

1. `TUTOR.md` — global persona and technical rules (always included)
2. `GUIDED.md` — guided-mode constraints (included for the two guided
   curricula; omitted for Free Exploration)
3. the curriculum's module `SKILL.md`
4. the current lab's `lab*.md` file (for the guided curricula)
5. the curriculum's `reference.md` (when present)

Only the current lab is loaded; future labs are not revealed. Tool
availability is filtered per lab so that early labs expose only basic
simulation tools and later labs unlock optimization tools.

## Repository layout

```
.
├── TUTOR.md                            # Global persona and technical rules
├── GUIDED.md                           # Guided-mode overlay (not used in Free Exploration)
│
├── teach-chromsep/                     # Curriculum 1 — Chromatography Fundamentals
│   ├── SKILL.md                        # Module skill: model system, parameters, POE cycle
│   ├── lab1-intro.md                   # Lab 1 — Introduction to the simulator
│   ├── lab2-isotherms.md               # Lab 2 — Isotherm fundamentals (selectivity, capacity)
│   ├── lab3-loading.md                 # Lab 3 — Loading effects (feed volume, concentration)
│   ├── lab4-flow-rate.md               # Lab 4 — Flow rate (mass transfer, efficiency)
│   └── reference.md                    # Theory reference appended to every lab prompt
│
├── teach-process-design/               # Curriculum 2 — Process Design
│   ├── SKILL.md                        # Module skill: process design framework
│   ├── lab1-cut-time.md                # Lab 1 — Purity-yield trade-off via cut time
│   └── lab2-optimization.md            # Lab 2 — Multi-constraint optimization challenge
│
└── free-exploration/                   # Curriculum 3 — Open-ended assistant
    └── SKILL.md                        # No fixed script; student drives the dialogue
```

## The three curricula

### 1. `teach-chromsep` — Chromatography Fundamentals

Four labs that develop intuition about the underlying physics of preparative
chromatography: how the Henry constant controls retention, how selectivity
and capacity combine in the competitive Langmuir isotherm, what feed volume
and concentration do to peak shape, and how flow rate affects efficiency
through mass transfer. Scaffolding is high throughout — the tutor walks
through each step and requires a prediction before every simulation.

### 2. `teach-process-design` — Process Design

Two labs that shift the student from "understand the physics" to "design a
separation against a specification." Lab 1 surfaces the purity-yield
trade-off through cut-time choice; Lab 2 presents an open optimization
challenge with multiple constraints. Scaffolding steps down from
medium to low — the student makes more of the decisions.

### 3. `free-exploration` — Open-ended assistant

No script, no POE cycle, no hidden lesson plan. The LLM acts as a
knowledgeable chromatography assistant, answers questions directly, runs
simulations on request, displays plots, and suggests related things the student could try
next. Intended for after the guided curricula, for curious students who
want to explore beyond the scripted labs.

## Pedagogical design

### The POE cycle

Every simulation in the guided curricula follows **Predict → Observe →
Explain**:

1. **Predict.** The tutor asks what the student expects, then stops. No
   hints, no preview of the answer.
2. **Observe.** The student's prediction is recorded, the simulator runs,
   and the actual data is presented.
3. **Explain.** The tutor compares prediction to outcome. If the student
   was wrong, the tutor says so directly and explains why — this is the
   moment in which learning happens.

### Scaffolding

Labs progress from high to low scaffolding as the student's mental model
matures:

| Level  | Tutor's role                                   | Student's role                      |
| ------ | ---------------------------------------------- | ----------------------------------- |
| HIGH   | Explain each step, model the reasoning aloud   | Follow along, make predictions      |
| MEDIUM | Guide experiments, prompt interpretation       | Analyze results, refine predictions |
| LOW    | Provide tools and constraints only             | Design the approach, synthesize     |

### Misconception challenges

Each lab targets a specific intuition that is usually wrong. The student
commits to a prediction *before* seeing data so that the correction is
memorable rather than passively absorbed. Examples:

- "Doubling feed volume doubles peak height." (Lab 3)
- "Faster flow rate is better, same as in most unit operations." (Lab 4)
- "A single cut time can hit ≥95 % purity and ≥90 % yield for both
  fractions simultaneously." (Process Design Lab 1)

## The model system

All labs use the same separation problem so that effects across labs can be
compared directly: **phenylalanine / tryptophan** on a hydrophobic
preparative resin.

| Compound      | qmax | K    | H = qmax·K |
| ------------- | ---- | ---- | ---------- |
| Phenylalanine | 50   | 0.05 | 2.5        |
| Tryptophan    | 50   | 0.10 | 5.0        |

Selectivity α = H₂ / H₁ = 2.0. Column: 15 cm × 1.5 cm, porosity 0.4, 300 µm
particles, CV ≈ 26.5 mL. Mass transfer: k_eff = 1 × 10⁻² s⁻¹,
D = 5 × 10⁻¹¹ m² s⁻¹.

The system is deliberately mid-range: selective enough for clean separations
at low loading, overlapped enough to show dramatic broadening under
realistic preparative conditions.

## Adapting for another system

The skill files reference MCP tool names (`init_session`, `set_column`,
`run_batch_chromatography`, `calculate_cut_time`, `compare_simulations`,
etc.) used by the ChromSep MCP server. To re-use the curriculum with a
different simulator:

1. **Tool names.** Replace the tool names in each SKILL.md and lab file
   with your simulator's equivalents. The per-lab `## Tools` lists are
   parsed at runtime to filter which tools the model sees.
2. **Default parameters.** Update the "Standard Parameters" block in each
   SKILL.md and the "Default System" block in `free-exploration/SKILL.md`
   with your compounds and column.
3. **Session management.** The skills assume a stateful simulator with
   session IDs. Adapt the init/destroy pattern to your transport.
4. **POE structure.** Preserve the prediction-before-simulation pattern;
   it is the core of the pedagogy and is largely domain-independent.

## License

Copyright 2026 Tuomo Sainio.

Licensed under the Apache License, Version 2.0. See `LICENSE` for the full
text. Individual files carry SPDX headers
(`SPDX-License-Identifier: Apache-2.0`).
