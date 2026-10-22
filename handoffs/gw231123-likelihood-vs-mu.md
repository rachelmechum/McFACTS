# Handoff: GW231123 log likelihood versus Gaussian IMF mean

Started 2026-10-21. Continues from the sessions of 2026-10-16 and 2026-10-19.

## Goal and completion criteria

The task, as the owner stated it: "Pick a Gaussian IMF (like Barry's 35 solar mass bump Gaussian), and let the mu parameter of that Gaussian be a free parameter. Show me the log likelihood of GW231123 as a function of mu."

Width is fixed at sigma = 4 Msun. Means run 20 to 80 Msun in steps of 5, the grid already simulated.

Done means one curve of log likelihood against mu, with four things attached: the estimator written out, the assumptions that make it valid checked rather than assumed, a Monte Carlo uncertainty from the existing simulation structure, and a statement of where the curve is not informative. A detection-weighted overlap does not satisfy this.

## Context and constraints

- Simulations are finished. Do not rerun McFACTS. The 13 means, 46 host/morphology cases and 100 galaxies per case are complete and audited.
- Prefer an existing tool. Basil, the event-fit catalog and the October estimators already exist. Ask before writing new analysis code.
- The owner has been through several detours on this task and wants the estimator justified before a curve is drawn.
- Cardassia is reachable as `ssh cardassia`. Issue read-only commands first and check before launching anything long.
- This task writes records and analysis products only. No change to `src/`, no new recipe. If code is needed, it belongs on a feature branch off `main-dev` and gets a PR to `McFACTS/McFACTS`.
- Repository left with `AGENTS.md` modified and `.cortexkit/` untracked. Preserve both. `NOTES.md` is the owner's curated file: propose, do not edit.
- No commit authorization for this task so far.

## Relevant files and sources

Paths on Cardassia are under `/home/rachel.mechum`.

| What | Where |
| --- | --- |
| Production simulations, 13 means | `scratch/gw231123_main100_production_20261017` |
| Analysis root (call it W) | `scratch/gw231123_current_basil_postproc_20261019` |
| Intrinsic population, one mean | `W/mu_N/stock_epochs/BumpInjection_sfr2.hdf5`, group `BumpInjection-sfr2/intrinsic` |
| Detection-weighted copy, one mean | same file, group `BumpInjection-sfr2/o3` |
| SNR model per mean | `W/mu_N/snr_extended_log201.hdf5` |
| mu=35 final interpolated population | `W/mu_35/interpolation_validation/full_log201.hdf5`; use this instead of its legacy `stock_epochs` file |
| Existing overlap curve and logs | `W/basil_overlap_results/` |
| Pipeline driver and manifests | `W/current_basil_pipeline_20261019.py`, `W/manifest.json`, `W/FULL_SCAN_COMMANDS.md` |
| Likelihood products and scripts | repo `analysis/gw231123-likelihood-vs-mu/`; Cardassia `W/mu_likelihood_20261021/` |
| Pinned clean Basil, commit 476120e151af93fce0a3df8b5b603ff2f25c21e2 | `scratch/basil_main_workflow_readonly_20261019` |
| Basil 0.1.0 event scorer | that checkout, `old/integrate_GW231123_likelihood.py` |
| Paper III likelihood-versus-lifetime plotter | `repos/basil/src/basil/DCO/plots/plot_mcfacts_lifetime.py` |
| 4D NAL event fit | `scratch/gw231123_clean_release_20261016/GW231123.nal.hdf5`, group `GW231123/mc_eta_chi1_chi2:NRSur7dq4:select` |
| Supported O4 event fit | `/Users/rachelmechum/Desktop/Projects/gw-event-likelihood-fits/editions/20261020-common-cosmo-o4-v4-supported/GW231123_135430.json` |
| Event-fit catalog API | same repo, `src/catalog.py`, documented in `VALIDATION.md` under "Catalog API and scope-aware access" |
| October likelihood scan, small fixed host | `~/.config/openchamber/chats/2026-10-16/session-2f62e4da-8961-478b-8cda-41d8f6477fa0/` |

The October session directory holds `RESULTS_01.md`, `scan_results.csv`, `score_scan.py`, `plot_scan.py`, `physical_selection.npz` and `review.json`. Its estimator was the conditional form; its population was a single 10^8 Msun host over 1 Myr with three 10-galaxy batches per mean.

## Progress and stopping point

Complete and checked: the simulations (598 of 598 cases valid, 59,800 galaxies, 7,883,571 mergers, McFACTS `7c3d6d6` with `flag_use_surrogate=0`), the per-mean Basil conversions, the per-mean detection weights, and one plotted curve.

The plotted curve is `W/basil_overlap_results/gw231123_basil_overlap_vs_mu.png`. Its values run from -2.060 at mu=35 to -0.453 at mu=65, with mu=80 at -5.889. Its ordinate is log[sum(w_det K) / sum(w_det)], so the detection weight sits inside the numerator and the event likelihood is never evaluated against the intrinsic population. It is a detection-weighted overlap and is labelled as exploratory in its own files.

2026-10-21, second pass: the conditional likelihood curve is computed and checked. The owner authorized the recommended route (single-event conditioning, published NAL event fit after a measure check) and the run went ahead. No simulation was rerun.

Products are in `analysis/gw231123-likelihood-vs-mu/`: the working figure, a top-panel-only figure for sharing, its caption, a per-mean CSV, the four scripts, and a README with the estimator. The shared figure carries the O3-selection provisional label in its subtitle. Cardassia copy is `W/mu_likelihood_20261021/`.

Result, relative to the maximum:

| mu | 20 | 25 | 30 | 35 | 40 | 45 | 50 | 55 | 60 | 65 | 70 | 75 | 80 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| dlnL | -0.59 | -1.08 | -1.49 | -1.62 | -1.31 | -1.28 | -1.43 | -1.57 | -0.67 | 0.00 | -0.73 | -2.91 | -5.51 |
| ESS | 220 | 184 | 166 | 319 | 606 | 897 | 1002 | 1082 | 2188 | 3036 | 2246 | 902 | 265 |
| den. bound | 0.46 | 0.36 | 0.30 | 0.28 | 0.24 | 0.23 | 0.23 | 0.22 | 0.21 | 0.23 | 0.22 | 0.22 | 0.22 |

The maximum sits at mu=65 in 400 of 400 host-case bootstrap resamples. There is a secondary shoulder at mu=20, about 0.59 below the peak and 5 sigma from it. Means 30 to 55 are disfavoured. Means 75 and 80 fall away steeply.

## Checks and findings

- Basil's 0.1.0 scorer and the Paper III plotter both place `weight_detection` in the numerator. The Paper III figure (`<model>_<universe>_GW231123_delay_time.png`) is the cumulative per-generation likelihood against AGN lifetime for one model, normalised so the total reaches one. It has no mu axis.
- GW231123_135430 is in the event-fit catalog: run O4a, aliases `GW231123_135430` and `S231123cg`, coordinates `mc_source, eta, chi_eff`, reference ESS 12492, nuisance measure `dVc/dz/(1+z)` with FlatLambdaCDM H0=67.9, Om0=0.3065.
- That fit's status in `provenance/current_catalog_index.json` is `accepted_for_declared_target`, with `fit_screen_status` = `candidate_training_screen_pass`. Its validation file passes for the declared flat and Gaussian-spin prior views. Earlier notes called its acceptance pending; that was stale metadata. It is still not a claim of uniform accuracy across arbitrary priors.
- The catalog README states the fits are joint likelihood shapes with arbitrary event-wise multiplicative constants, and that no redshift likelihood or evidence normalization is claimed. Any absolute offset in the curve is therefore arbitrary.
- The fit's analysis support begins at `mc_source` = 28.8 Msun, with eta from 0 to 0.25 and chi_eff within +/-0.99. Low-mu populations may sit largely outside support. That is the same pattern as the October note that means 20 to 30 had no sampled overlap, and it is a real support cut rather than a plotting artifact.
- The per-mean HDF5 carries `chirp_mass` (source frame) alongside `chirp_mass_det`, plus `weight_intrinsic`. The previously quoted 12,326,928 selected rows belong to the legacy direct mu=35 file. The final interpolated file has the counts below.
- October's own review found the source-3D fit peaking at mu=60 while the dedicated 4D fit was flat between 35 and 45. That disagreement is why the October result was never reported as a measurement.

### Read-only checks in this session, 2026-10-21

All 13 final files contain `BumpInjection-sfr2/intrinsic` and `BumpInjection-sfr2/o3`, the expected weight fields, and selection attributes `observing_run=o3`, `snr_resolution=201`. Checks used existing source/log inspection, `h5ls`, and inline HDF5 diagnostics opened with mode `r`. No analysis script was saved.

| Final mu=35 check | Result |
| --- | ---: |
| Intrinsic rows | 16,115,740 |
| Eligible before SNR (`will_merge` and `zmer < 2`) | 16,107,730 |
| Excluded before SNR | 8,010 |
| Dropped by optimal-SNR cut | 6,173,749 |
| Stored selected rows | 9,933,981 |
| Both stored weights finite and nonnegative | True |
| Zero intrinsic weights among selected rows | 0 |
| Recovered alpha range | 0.01922662607160152 to 1 |
| `sum(weight_detection)`, per year | 20.550377727545367 |
| `sum(w * (weight_detection / w))`, per year | 20.550377727545367 |
| Maximum absolute reconstruction difference | 0 |
| Stored selected SNR minimum | 8.000002881382938 |
| Intrinsic SNR array saved | No |

The sum identity checks stored-weight algebra, not completeness. `LVK_Population.py:74-84,423-443` removes nonmerging/high-redshift rows, evaluates eligible SNRs, then discards SNR <= 8 before evaluating detection probabilities. The final log confirms these counts: `W/mu_35/interpolation_validation/full_log201_detection.log`.

Missing eligible rows had SNR evaluated, but their noisy detection probabilities were never evaluated or saved. They are assigned zero only by the hard-cut implementation. `LIGO_noise.py:92-156` permits nonzero noisy probabilities below optimal SNR 8; its interpolation zeroes values only below 2. Thus the saved sum supports a truncated selection model, not the full noisy model. The discarded SNRs and trained probability model are not stored in these population groups.

The final mu=35 log specifies `IMRPhenomPv2`, PSD `SimNoisePSDaLIGOaLIGO140MpcT1800545`, and optimal-SNR threshold 8. These are O3-labelled settings, not an O4a selection calibration.

### Checks behind the curve, 2026-10-21

- The event term reproduces the recorded overlap curve at mu=35 when the detection weight is put back in the numerator (-2.06024 against -2.060). Event evaluation and row handling match the earlier pipeline.
- Stored `chirp_mass` equals `(m1 m2)^(3/5)/(m1+m2)^(1/5)` from `mass_1` and `mass_2` to 1.5e-11. The event term is evaluated in the source frame on both sides.
- `symmetric_mass_ratio` equals `m1 m2/(m1+m2)^2` to 1.7e-16, and `chi_1`/`chi_2` are non-negative magnitudes in (0, 0.98), matching the NAL fit's two spin coordinates.
- The NAL fit centres at `mc_source` = 103.63. The independent O4 fit's leading component sits at 103.33. Two separately built fits agree on the mass scale, which supports the frame reading.
- Six of the 46 host/morphology cases produced no mergers: early bins 0 and 1, late bins 0 to 3. Their `output_mergers_population.dat` files are header-only and their `nsystems` is 0. Early bin 2 holds 43 mergers but contributes no samples to the converted population either. The population therefore carries 39 subgroups, all at the low host-mass end of the gap.
- Each unique McFACTS merger appears about 30 times across galaxy and epoch reweightings (532,878 unique mergers against 16.1M rows at mu=35). Effective sample sizes are counted per unique `(subgroup_id, binary_id)` merger.
- The numerator is dominated by whichever hierarchical generation lands near the event's mass. At mu=20 the event term is 100% generation 4 and above; at mu=60 to 70 it is 98 to 99.99% generation 2-2; at mu=80 it is 69% first generation and 30% generation 2-2. This drives the bumps rather than Monte Carlo noise.
- The event sits at the 91st percentile of the mu=20 population's chirp mass, the 54th at mu=65, and the 44th at mu=80, so it is not an extreme outlier for mid and high means.
- At mu=80, 18% of the event term lies within 5 Msun of the fit's upper mass edge (4.8% at mu=75, below 0.2% everywhere else). Part of the decline at 75 and 80 is support truncation.
- Paired host-case bootstrap, 400 resamples: the argmax is mu=65 in every resample. Paired standard deviations of differences against the best mean run 0.03 to 0.12, so differences are better determined than the pointwise bars suggest.

## Blockers and open questions

Settled this session:

1. **Conditioning.** Single event, Poisson factor dropped. Authorized by the owner on 2026-10-21. The estimator is the one above. A rate likelihood would need an absolute-rate model and a defined catalog, and answers a different question.
2. **Event fit.** The published NAL fit in `(chirp_mass, symmetric_mass_ratio, chi_1, chi_2)`. It was already the fit Basil loads, its frame and coordinate conventions check out, and its mass scale agrees with the independent O4 fit to 0.3%. The O4 fit remains available as a cross-check; it is not used here because its coordinates and nuisance marginalization differ.
3. **Measure.** Checked. Source-frame on both sides, no detector-frame column, no missing (1+z).
4. **Support.** Event ESS and contributing-merger counts are reported per mean in the CSV and the figure, and mu=75 and 80 are marked as reaching the fit's mass edge.

Still open:

1. **Selection observing run.** The weights are O3-labelled: `IMRPhenomPv2`, the O3 PSD, optimal-SNR threshold 8, no spin dependence. GW231123 is an O4a event. Reproducing the selection at O4a needs the O4 detection machinery, and the copy in `repos/basil` is uncommitted, combines O4a and O4b, marginalises over an injection spin distribution, and clamps component masses at 120 Msun. That is a separate piece of work.
2. **Denominator completeness.** The stored sum is a lower bound. Tightening it needs optimal SNR for the discarded rows, which means evaluating the stored SNR interpolator and Basil's detection-probability model again. The model trains from random draws with no seed, so it would need to be pinned. Not done.
3. **Spin pairing.** `mass_1` is not always the larger mass. The event term is symmetric in the two spins and `eta` is symmetric, so the effect is small, but the pairing should be fixed if `chi_eff` is ever used as a coordinate.
4. **Realization variance.** Each mean rests on one McFACTS seed per host case. The paired bootstrap covers host sampling only. Repeat seeds would separate the 60 to 70 structure from realization noise.
5. **Lowest-mass hosts.** Six cases produced no mergers and one more contributes no samples, so the population integral omits the lowest host masses entirely.

## Next action

1. Owner decision on the selection observing run: accept the O3-labelled selection with that caveat stated, or commission an O4a selection. This is the largest remaining systematic.
2. If accepted, the curve is ready to report as relative likelihood with the stated scope. Optionally add the O4 event fit as a sensitivity check in the same figure.
3. Optional hardening: pin a seed and fill in the sub-threshold detection probabilities to replace the one-sided denominator bound with a computed value.
4. Do not rerun McFACTS. Do not relabel the existing overlap curve.
