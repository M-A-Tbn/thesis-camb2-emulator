# CAMB2 thesis — progress and results

I'm working on reproducing Emmanuel's matter power-spectrum emulator with
CAMB2. Before starting that stage, I've been comparing CAMB 1.6.6, CAMB 2.0.4
and CLASS 3.1.0 to see how closely their predictions agree.

This repository brings together the results and weekly updates for discussion.
It was last updated on **29 October 2026**.

## This week's result

I extended the comparison from z=0 to nine redshifts between z=0 and z=2,
using flat LambdaCDM, curved LambdaCDM and evolving dark energy.

**CAMB2 has smaller maximum and RMS differences from CLASS in all 27 tested
model–redshift combinations**, within 0.02 < k < 0.3 Mpc^-1. RMS measures the
overall size of the sampled differences.

![Agreement with CLASS across nine redshifts](camb-comparison/01_current_solver_comparison/results/figures/01_solver_agreement_by_redshift.png)

The same pattern holds across the redshifts tested. CLASS is an independent
comparison, though, so closer agreement is not proof of absolute accuracy.
A numerical convergence test is also still needed.

## Where to start

- [This week's update](meetings/2026-10-29.md) gives the main result and next step.
- [The full report](camb-comparison/01_current_solver_comparison/results/Report.md)
  explains the setup, plots and limitations.
- [The input notes](camb-comparison/02_shared_inputs_and_z0_reference/Source%20audit.md)
  explain which settings come from Emmanuel's thesis and which I had to choose.
- [The result index](camb-comparison/01_current_solver_comparison/README.md)
  links the figures and numerical files.

## Next step

I need Emmanuel's implementation, trained weights and training/validation
scripts so I can reproduce one reference spectrum before moving on to CAMB2
training. Access to those files and the scope of that first test are the main
points I'd like to discuss next.

Feedback can be added in [Issues](https://github.com/M-A-Tbn/thesis-camb2-emulator/issues)
or discussed with the weekly update. The original runs, code and installation
records are kept separately and can be supplied if needed.
