# Status

## Current work

GW231123 log likelihood as a function of the mean of a Gaussian initial-mass function, width fixed at 4 Msun. See [the handoff](handoffs/gw231123-likelihood-vs-mu.md).

The conditional likelihood curve is computed over all 13 simulated means. The maximum is at mu=65. A secondary shoulder sits at mu=20. Products are in `analysis/gw231123-likelihood-vs-mu/`.

## Where work stopped

2026-10-21. Curve computed, checked, and plotted with event ESS, contributing-merger counts, a one-sided selection-truncation bound, and a paired host-case bootstrap. The maximum is at mu=65 in 400 of 400 bootstrap resamples. No simulation was rerun.

## Next action

Owner decision on the selection observing run. The stored detection weights are O3-labelled while GW231123 is O4a. Accept the curve with that caveat, or commission an O4a selection. The handoff carries the estimator, the checks, and the remaining open items.

## Blockers

The selection model is O3-labelled (optimal-SNR threshold 8, O3 PSD, no spin dependence). Reproducing it at O4a needs the O4 detection machinery, which is uncommitted and covers O4a plus O4b jointly.

The selection denominator is a lower bound: Basil discards rows below optimal SNR 8 before evaluating detection probability. The largest possible correction is bounded and shown on the figure.

## Active handoffs

- [GW231123 likelihood versus Gaussian IMF mean](handoffs/gw231123-likelihood-vs-mu.md)

## History

- 2026-10-21: project documentation set up. Shared structure added, owner notes populated, no simulations or tests run for that migration.
- 2026-10-21: conditional likelihood curve for GW231123 versus Gaussian IMF mean computed from finished simulations.
