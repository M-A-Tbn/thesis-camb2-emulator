# Thesis progress

This repository follows the thesis from the initial solver comparisons through
to emulator reproduction, training and validation. Reports, plots and weekly
updates are collected here for discussion with Professor Marulli.

**Last updated: 30 October 2026.**

## Aim of the thesis

The aim is to reproduce Emmanuel's emulator of the linear matter power spectrum
and build a version based on CAMB2. An emulator provides a fast approximation
to a solver's predictions, so its accuracy needs to be checked against the
solver on cases that were not used for training.

The CAMB2 emulator will also be compared with Emmanuel's emulator over their
shared parameter range. Once that baseline works, and if time allows, a
massive-neutrino extension will be investigated following Pezzotta et al.

The CAMB comparison is the first practical step in this path. It helps check
the predictions that will later be used for the emulator work.

## Project path

| Stage | Work | Current status |
|---|---|---|
| 1. Solver study and comparison | Study CAMB2 and compare its predictions with CAMB, using CLASS as an independent reference. | Three models have been compared at nine redshifts. Numerical convergence remains to be checked. |
| 2. Reproduce Emmanuel's emulator | Obtain the implementation and trained weights, then reproduce the main results and plots. | The next milestone is one reference-spectrum prediction with its settings clearly recorded. The emulator has not yet been reproduced. |
| 3. Train and validate a CAMB2 emulator | Generate the training data, train the emulator and test it against CAMB2 on an independent test set. | Not started. |
| 4. Compare the emulators | Compare the CAMB2 and Emmanuel emulators over their shared parameter range. | Planned after reproduction and training. |
| 5. Investigate massive neutrinos | Study an extension following Pezzotta et al. and agree on a suitable test and validation range. | Conditional on completing the baseline and having enough time. |

## Current step: solver comparison

The current results compare CAMB 1.6.6, CAMB 2.0.4 and CLASS 3.1.0 for flat
LambdaCDM, curved LambdaCDM and evolving dark energy. Each model was tested at
nine redshifts from z=0 to z=2.

**CAMB2 had smaller maximum and RMS differences from CLASS in all 27 tested
model–redshift combinations**, within 0.02 < k < 0.3 Mpc^-1. RMS measures the
overall size of the sampled differences. This shows closer agreement with
CLASS under the tested settings, rather than establishing absolute accuracy.

- [Latest weekly update](meetings/2026-10-29.md): what was done, the main findings and the remaining work.
- [Comparison report](camb-comparison/01_current_solver_comparison/results/Report.md): setup, results and limitations.
- [Comparison plot](camb-comparison/01_current_solver_comparison/results/figures/01_solver_agreement_by_redshift.png): agreement across the nine redshifts.
- [Input notes](camb-comparison/02_shared_inputs_and_z0_reference/Source%20audit.md): documented thesis settings and choices made for this comparison.

The next small step is to obtain Emmanuel's implementation, trained weights
and training/validation scripts, then reproduce one reference spectrum before
moving on to training.

## Repository structure

The tree below shows all files currently shared in this repository.

```text
thesis-progress/
├── .gitattributes
├── .gitignore
├── README.md
├── camb-comparison/
│   ├── 01_current_solver_comparison/
│   │   ├── Solver comparison guide.md
│   │   └── results/
│   │       ├── Report.md
│   │       └── figures/
│   │           ├── 01_solver_agreement_by_redshift.png
│   │           ├── 02_matter_power_flat_lcdm.png
│   │           ├── 03_matter_power_curved_lcdm.png
│   │           ├── 04_matter_power_evolving_dark_energy.png
│   │           └── Figure index.md
│   └── 02_shared_inputs_and_z0_reference/
│       └── Source audit.md
└── meetings/
    └── 2026-10-29.md
```

The `meetings/` folder holds the weekly updates across the thesis. The
`camb-comparison/` folder contains the reports and plots for the solver-comparison
stage. Within it, `01_current_solver_comparison/` holds the current results,
while `02_shared_inputs_and_z0_reference/` records how the baseline inputs
were chosen. Those numbers identify folders within this stage; they are not
the stages of the whole thesis.

Reports and plots for reproduction, training, validation and any neutrino
extension will be added in their own folders as the work is carried out.
Those stages are planned above; they do not have published results yet.

## Discussion and feedback

Feedback and questions can be added in
[Issues](https://github.com/M-A-Tbn/Thesis-Progress/issues) or discussed alongside
the weekly updates.

This repository will continue to track the whole thesis through written
reports and PNG plots. Numerical data, calculation code, raw runs and SVG
originals remain in the local research workspace. A separate repository for
the complete code and calculation files is planned for later.
