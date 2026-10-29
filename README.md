# CAMB2 thesis — progress and results

This repository presents the current results and weekly updates for a thesis
on a CAMB2-based emulator of the linear matter power spectrum.

**Latest update: 29 October 2026.** The current stage is a comparison of the
underlying solvers. Emulator reproduction and training are the next stages.

## Review this week's work

1. [Weekly update: 26–29 October](meetings/2026-10-29.md) — completed work,
   principal findings, limitations and next steps.
2. [Scientific report](camb-comparison/01_current_solver_comparison/results/Report.md)
   — methods, figures and numerical results.
3. [Input assumptions and source audit](camb-comparison/02_shared_inputs_and_z0_reference/Source%20audit.md)
   — which settings come from Emmanuel's thesis and which were adopted here.

## Current finding

We compared CAMB 1.6.6, CAMB 2.0.4 and CBL-bundled CLASS 3.1.0 in three
representative cosmologies at nine redshifts from z=0 to z=2.

**CAMB2 has smaller maximum and RMS disagreement with CLASS in all 27 tested
model–redshift combinations**, within 0.02 < k < 0.3 Mpc^-1. RMS summarizes the
overall size of the sampled differences.

![Agreement with CLASS across nine redshifts](camb-comparison/01_current_solver_comparison/results/figures/01_solver_agreement_by_redshift.png)

This establishes closer agreement with CLASS under the tested settings.
It does not establish absolute accuracy or numerical convergence. The settings
are thesis-informed reconstructions rather than an exact reproduction of
Emmanuel's experiment.

## Next step and feedback

Obtain Emmanuel's implementation, trained weights and training/validation
scripts, then produce one traceable reference spectrum. The immediate
discussion points are access to those assets and the scope of the first
reproduction test.

Comments on the results, assumptions and proposed next steps can be recorded
in the repository's Issues or discussed alongside the weekly update.

## Supporting results

The [result index](camb-comparison/01_current_solver_comparison/README.md) links
the power-spectrum figures, saved comparison arrays and detailed numerical
summaries. Original run records, calculation code and installation records are
retained in the research workspace and are available for a detailed methods
review. This repository is the review copy of the results.
