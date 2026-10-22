# Project notes

The owner's curated overview of major findings, decisions, and important project knowledge. Agents suggest additions and wait for approval before editing. Routine progress belongs in handoffs.

## Overview

McFACTS simulates compact-object evolution and binary mergers in AGN disks. This repository owns the simulation code; population-likelihood integration is in the sibling hyperpipeline project.

## Important findings

- `recipes/model_choice.ini` supplies default inputs. Set a seed for reproducible runs.
- `runs/` holds outputs. Population merger summaries and per-timestep snapshots serve different analysis needs; enable snapshots when tracking individual objects.
- [README.md](README.md) describes installation and outputs. `IOdocumentation.txt` documents input/output fields.

## Major decisions

Follow the contributor and citation guidance linked in the README. Keep analysis evidence separate from simulation implementation.

## Open questions

No current simulation task or stopping point was established from the documentation inspected. Confirm the intended task before launching runs.
