# Solver comparison — figures and data

This folder contains the comparison of CAMB 1.6.6, CAMB 2.0.4 and CLASS 3.1.0
for three cosmologies at nine redshifts from z=0 to z=2.

Start with the [report](results/Report.md) for the setup and results, or open
the [redshift comparison](results/figures/01_solver_agreement_by_redshift.png)
for the main finding.

## Figures

The power spectra are shown separately for
[flat LambdaCDM](results/figures/02_matter_power_flat_lcdm.png),
[curved LambdaCDM](results/figures/03_matter_power_curved_lcdm.png), and
[evolving dark energy](results/figures/04_matter_power_evolving_dark_energy.png).
Each figure has nine panels, one per redshift. The
[figure index](results/figures/README.md) lists the PNG and SVG versions.

## Inputs and numerical results

The [input notes](../02_shared_inputs_and_z0_reference/Source%20audit.md) explain
which settings were taken from Emmanuel's thesis and where choices were needed.
The values are recorded in the [shared configuration](../02_shared_inputs_and_z0_reference/config.json)
and [model configuration](config.json).

The [statistics file](results/statistics.json) gives the unrounded differences,
their signs and locations, and RMS values. The [audit](results/audit.json)
records the checks made during the calculation. File fingerprints are saved
in the [provenance record](results/provenance.json).

The comparison arrays are available as NumPy NPZ files for the
[flat](results/flat.npz), [curved](results/curved.npz), and
[evolving-dark-energy](results/evolving_de.npz) models. Keys without a suffix
refer to z=0; a suffix such as `_z025` means z=0.25. Each file also contains
the redshift list and matched spectra at CLASS's original output points.

The original raw runs, calculation code and software are kept in the research
workspace. They can be supplied for a closer review or a rerun; the historical
paths in the saved records refer to that workspace.
