# Solver comparison — results and methods

The current comparison covers three cosmologies at nine redshifts from z=0
to z=2, using CAMB 1.6.6, CAMB 2.0.4 and CLASS 3.1.0 through CosmoBolognaLib.

## Reports and figures

- [Scientific report](results/Report.md): purpose, inputs, method, results and limits.
- [Agreement across redshifts](results/figures/01_solver_agreement_by_redshift.png):
  maximum and RMS disagreement with CLASS in each model.
- Actual power spectra at all nine redshifts:
  [flat LambdaCDM](results/figures/02_matter_power_flat_lcdm.png),
  [curved LambdaCDM](results/figures/03_matter_power_curved_lcdm.png), and
  [evolving dark energy](results/figures/04_matter_power_evolving_dark_energy.png).
- [Figure index](results/figures/README.md): PNG and scalable SVG versions.
- [Source audit](../02_shared_inputs_and_z0_reference/Source%20audit.md):
  documented thesis settings and adopted assumptions.

## Numerical records

The [statistics](results/statistics.json) give unrounded maxima, their signs and
locations, and RMS values. The [audit](results/audit.json) records the checks
performed during the calculation. The [provenance record](results/provenance.json)
identifies the saved files by SHA-256 fingerprints.

Comparison arrays are saved as NumPy NPZ files for
[flat](results/flat.npz), [curved](results/curved.npz), and
[evolving-dark-energy](results/evolving_de.npz) models. Each contains the
redshift list and the matched spectra at native CLASS output points. The
original keys refer to z=0; suffixes such as `_z025` identify z=0.25.

The [model configuration](config.json) gives the curvature, dark-energy and
redshift choices. The [shared configuration](../02_shared_inputs_and_z0_reference/config.json)
gives the common physical inputs. Historical paths in these records identify
the original research workspace.

Original raw runs, calculation scripts, software environments and recoverable
backups remain in that workspace. They support the recorded audit but are not
included in this results repository. The published files allow review of the
reported results; rerunning the full calculation requires those additional
materials.
