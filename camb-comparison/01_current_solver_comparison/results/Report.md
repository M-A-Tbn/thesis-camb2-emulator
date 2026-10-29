# Three thesis-informed cosmologies: comparison across nine redshifts

## Purpose and scope

Our question is: **when we describe the same universe to CAMB 1.6.6, CAMB 2.0.4 and CLASS 3.1.0, how closely do their predictions agree?**

These programs are called *solvers* because they solve equations describing how the universe and its small density fluctuations evolve. Here we ask each solver for the linear matter power spectrum, P(k). This describes the strength of matter-density fluctuations at different spatial scales.

“Linear” means that we use equations that treat the fluctuations as small. We compare those linear predictions even on scales where real matter clustering today would require a more complicated, nonlinear calculation.

We test three examples: a flat universe with a cosmological constant, a curved universe with a cosmological constant, and a flat universe whose dark energy changes with time. These are the three types of model shown in Emmanuel’s Figure 4.3 (printed page 62, PDF page 67).

There is an important difference between his figure and ours. **Emmanuel compares his emulator with CAMB. We compare three solvers with one another.** We have not yet tested or trained an emulator in this experiment.

We call our settings *thesis-informed*: we use his documented values where available and record our choices where his exact settings are missing. This is not an exact reproduction of his figure.

We have extended the same experiment rather than started a separate project. The earlier z=0 results are preserved and checked against a backup. We added eight earlier epochs for each model, with three fresh runs per solver and epoch. This gives 27 model–redshift combinations and 81 distinct spectra. There are 216 new solver runs, plus the 27 runs already completed at z=0.

## Inputs and method

### What stays the same?

We keep the ordinary-matter density, cold-dark-matter density, present expansion rate and initial fluctuation settings the same across the three examples. Within each example, all three solvers receive the same stated physical inputs.

We now compare **nine redshifts: z=0, 0.25, 0.5, 0.75, 1, 1.25, 1.5, 1.75 and 2**. Redshift tells us which stage of the universe's history we are looking at: z=0 is today, and larger z means earlier times. Changing redshift does not create a different cosmology; it asks for that cosmology's prediction at another time. All cases have **zero massive neutrinos**. Massless neutrinos still contribute radiation; “no massive neutrinos” does not mean we removed all neutrinos.

For reference, the shared inputs are:

- **H0=67.36 km/s/Mpc:** the present expansion rate.
- **omega_b=0.0224 and omega_cdm=0.120:** measures of the ordinary-matter and cold-dark-matter densities. These are the physical-density parameters omega=Omega × h², where h=H0/100.
- **As=2.1e-9 and ns=0.9649:** the initial strength of primordial curvature fluctuations and how that strength varies with scale.
- **tau=0.0544:** the optical depth associated with reionization, describing the chance that a CMB photon scatters during that later period.
- **TCMB=2.7255 K, Neff=3.044 and YHe=0.245:** the CMB temperature, the effective number describing neutrino radiation here, and the helium mass fraction.
- **pivot=0.05 Mpc^-1:** the reference wavenumber at which the primordial amplitude and tilt are specified.

These inputs are explicitly recorded so the comparison can be repeated. The [baseline source audit](../../02_shared_inputs_and_z0_reference/Source%20audit.md) distinguishes the values documented in the thesis from our own choices.

### What do we change?

Omega_k describes spatial curvature. It is different from the wavenumber k on the plots.

The parameters w0 and wa describe dark energy: w0 is its equation-of-state value today, and wa controls its evolution. The equation of state relates dark energy’s pressure to its energy density. A cosmological constant has w=-1 at all times.

| Model | Omega_k | w0 | wa |
|---|---:|---:|---:|
| Flat LambdaCDM | 0 | -1 | 0 |
| Curved LambdaCDM | 0.01 | -1 | 0 |
| Evolving dark energy | 0 | -0.99 | -0.02 |

For the curved example, we change Omega_k from 0 to 0.01. For the evolving-dark-energy example, we keep space flat and change w0 and wa.

The redshifts are those listed in Emmanuel's Chapter 5, Tables 5.1–5.3. Combining them with these Figure-4.3-inspired examples is our comparison design. It is not an exact repetition of one experiment from his thesis.

The present dark-energy density is calculated so the model’s matter, radiation, dark energy and curvature satisfy the expansion equation. Therefore, when we introduce curvature while keeping H0 and the matter densities fixed, the dark-energy density adjusts. We do not hold every density fraction fixed independently.

### How do we handle evolving dark energy?

We use the time dependence w(a)=w0+wa(1-a), where the scale factor a equals 1 today and was smaller in the past.

For our chosen values, w passes through -1 at a=0.5, corresponding to z=1. The solvers calculate the evolution of the fluctuations through the relevant history, including this crossing when it lies before the requested output time.

Both solvers use a treatment called **PPF**, which handles dark-energy perturbations through this crossing. Matching that choice helps avoid comparing two different treatments without realizing it.

For technical reference, we use CAMB PPF and CLASS use_ppf=yes, with cs2=1 and c_gamma/c_s=0.4. CLASS calls the time-dependence option CLP; it is the CPL form written above. In this case we set its separate cosmological-constant component to zero and let its evolving dark-energy component supply the required density. We checked the saved CLASS history of w and the present-day densities.

### How do we make the outputs comparable?

**First, we use the same units and the same definition of matter fluctuations.**

Our matter spectrum includes ordinary matter plus cold dark matter. CAMB and CLASS can describe density fluctuations using different coordinate conventions, called *gauges*. Comparing different conventions directly could create a difference that is not the solver difference we want to study.

We therefore use CLASS’s saved component outputs to construct the same synchronous-gauge matter quantity used by CAMB. We also checked that those component outputs recover CLASS’s own original spectrum when combined according to its original convention. This checks the conversion and normalization; it is not a test of absolute physical accuracy. At each redshift, the check uses the expansion rate at that epoch rather than incorrectly reusing today's expansion rate.

**Second, we compare at the same k values.**

CosmoBolognaLib (CBL) launches CLASS and retrieves its original table through a method called Table_PkCodes. We do not use the higher-level Pk_matter method that adds CBL’s interpolation. We also do not rescale the spectrum afterward to match a chosen sigma8, a measure of fluctuation amplitude.

We keep CLASS’s original output points and ask CAMB for its predictions at those same points. CAMB uses its own interpolation to return those values. Thus we avoid *CBL’s additional interpolation*, not all interpolation inside the calculations.

### Which scales do we compare?

The wavenumber k labels spatial scale: smaller k means larger spatial scales, and larger k means smaller spatial scales.

We compare saved points within **1e-4 <= k <= 1 Mpc^-1**. Both solvers are asked to calculate through k=2 Mpc^-1, beyond the upper end of the comparison.

We also report a smaller **focus interval, 0.02 < k < 0.3 Mpc^-1**, which continues the interval used in our earlier comparisons. The main figure summarizes disagreements within this interval at each redshift. It is not the full range covered by Emmanuel’s thesis or a claim that all these scales can be treated linearly in observations.

We retain each solver’s native numerical settings and recombination treatment. Recombination is the early period when electrons and nuclei formed neutral atoms; its treatment can affect the predicted spectrum. Since these treatments differ between solvers and versions, this experiment measures their practical prediction differences, not only numerical calculation errors.

## Results: how agreement changes with redshift

Our new question is: **does the agreement we found today remain similar at earlier times?**

At each scale we calculate: **Difference (%) = 100 × (P_CAMB / P_CLASS − 1).** CLASS is the comparison reference, not a known exact answer. A positive value means CAMB predicts more power; a negative value means less.

We summarize these differences in two ways. The **maximum** is the largest absolute difference among the points checked. **RMS** measures the overall size of the differences: square them, average them, then take the square root, so positive and negative values do not cancel. Both are percentages, not uncertainties.

### The main figure

Each column is one cosmology. The horizontal axis is now **redshift**, not k. The upper row shows the largest sampled disagreement inside our focus interval; the lower row shows RMS disagreement in the same interval. Orange is CAMB 1.6.6 and teal is CAMB 2.0.4. Each dot summarizes one redshift; connecting lines are visual guides, not extra calculations.

A lower value means closer agreement with CLASS. This figure summarizes the comparison; it does not show the power spectra themselves. All spectra remain saved in the same numerical results files.

![Three models](figures/01_solver_agreement_by_redshift.png)

### The actual P(k) spectra

The summary above tells us how far apart the predictions are. The next figures show **what the solvers actually predict**: the strength of matter fluctuations at each scale. There is one figure per cosmology and one panel per redshift, so all 27 cases are shown. Every panel contains the three solver predictions, using the same matched matter definition as the comparison.

Read left to right, then move down to the next row. The horizontal axis is **k**, not redshift: larger k means smaller spatial scales. The vertical axis is **P(k, z)** in Mpc³. Both axes are logarithmic, meaning equal distances represent equal multiplication factors rather than equal additions. All panels and figures use the same axis ranges, so their heights can be compared directly. The grey band marks our focus interval, not an uncertainty band.

The curves almost overlap because their differences are small compared with the range shown. Grey solid is CLASS, orange dashed is CAMB 1.6.6 and teal dotted is CAMB 2.0.4. An almost single-looking curve does not mean a solver is missing or the predictions are identical. Use the disagreement summary and tables for the small numerical differences. The lines join the saved comparison points; we have not run extra k values to draw them.

Within each cosmology, the spectrum is generally lower at higher redshift. Our physical interpretation is that matter fluctuations have had less time to grow at those earlier epochs. This is a statement about the evolution of the predicted power, not about solver error. The plots remain linear-theory predictions, including on scales where a description of real clustering would require nonlinear physics.

#### Flat LambdaCDM spectra

![Flat LambdaCDM power spectra at nine redshifts](figures/02_matter_power_flat_lcdm.png)

#### Curved LambdaCDM spectra

![Curved LambdaCDM power spectra at nine redshifts](figures/03_matter_power_curved_lcdm.png)

#### Evolving-dark-energy spectra

![Evolving-dark-energy power spectra at nine redshifts](figures/04_matter_power_evolving_dark_energy.png)

### Largest focus-interval differences across all nine redshifts

For each solver and model, the next table finds the largest sampled difference across both k and the nine redshifts. The redshift columns tell us when that largest disagreement occurs. The two versions can reach their maxima at different times and scales.

| Model | CAMB 1.6.6 maximum | z at maximum | CAMB 2.0.4 maximum | z at maximum |
|---|---:|---:|---:|---:|
| Flat LambdaCDM | 0.140297% | 2 | 0.069197% | 2 |
| Curved LambdaCDM | 0.159183% | 2 | 0.065888% | 2 |
| Evolving dark energy | 0.140659% | 2 | 0.068325% | 2 |

### Results at each redshift

These tables let you read the figure's values directly. For example, a difference of 0.10% means one tenth of one percent, not ten percent. The z=0 rows are unchanged from the earlier report. Maxima refer to 0.02 < k < 0.3 Mpc^-1.

#### Flat LambdaCDM

| z | Original CLASS focus points | CAMB 1.6.6 maximum | CAMB 2.0.4 maximum | CAMB 1.6.6 RMS | CAMB 2.0.4 RMS |
|---|---:|---:|---:|---:|---:|
| 0 | 63 | 0.136461% | 0.065369% | 0.036083% | 0.029118% |
| 0.25 | 63 | 0.135668% | 0.065362% | 0.036108% | 0.029121% |
| 0.5 | 63 | 0.135881% | 0.065301% | 0.036263% | 0.029097% |
| 0.75 | 63 | 0.136555% | 0.065470% | 0.036412% | 0.029053% |
| 1 | 63 | 0.137009% | 0.065884% | 0.036460% | 0.029038% |
| 1.25 | 63 | 0.138093% | 0.066986% | 0.036541% | 0.029092% |
| 1.5 | 63 | 0.138943% | 0.067842% | 0.036602% | 0.029235% |
| 1.75 | 63 | 0.140043% | 0.068963% | 0.036682% | 0.029369% |
| 2 | 63 | 0.140297% | 0.069197% | 0.036707% | 0.029448% |

#### Curved LambdaCDM

| z | Original CLASS focus points | CAMB 1.6.6 maximum | CAMB 2.0.4 maximum | CAMB 1.6.6 RMS | CAMB 2.0.4 RMS |
|---|---:|---:|---:|---:|---:|
| 0 | 62 | 0.153655% | 0.062971% | 0.038343% | 0.028997% |
| 0.25 | 62 | 0.153999% | 0.062613% | 0.038398% | 0.028999% |
| 0.5 | 62 | 0.155157% | 0.062627% | 0.038508% | 0.028921% |
| 0.75 | 62 | 0.157154% | 0.063181% | 0.038669% | 0.028830% |
| 1 | 62 | 0.157835% | 0.063805% | 0.038712% | 0.028813% |
| 1.25 | 62 | 0.158323% | 0.064794% | 0.038790% | 0.028883% |
| 1.5 | 62 | 0.158834% | 0.065509% | 0.038858% | 0.028947% |
| 1.75 | 62 | 0.159101% | 0.065716% | 0.038880% | 0.028990% |
| 2 | 62 | 0.159183% | 0.065888% | 0.038863% | 0.029007% |

#### Evolving dark energy

| z | Original CLASS focus points | CAMB 1.6.6 maximum | CAMB 2.0.4 maximum | CAMB 1.6.6 RMS | CAMB 2.0.4 RMS |
|---|---:|---:|---:|---:|---:|
| 0 | 63 | 0.138969% | 0.066643% | 0.036508% | 0.029578% |
| 0.25 | 63 | 0.138484% | 0.066152% | 0.036439% | 0.029441% |
| 0.5 | 63 | 0.138027% | 0.065697% | 0.036449% | 0.029376% |
| 0.75 | 63 | 0.138097% | 0.065775% | 0.036447% | 0.029302% |
| 1 | 63 | 0.138043% | 0.065706% | 0.036433% | 0.029265% |
| 1.25 | 63 | 0.138533% | 0.066193% | 0.036406% | 0.029328% |
| 1.5 | 63 | 0.139336% | 0.067002% | 0.036462% | 0.029356% |
| 1.75 | 63 | 0.139772% | 0.067420% | 0.036520% | 0.029368% |
| 2 | 63 | 0.140659% | 0.068325% | 0.036590% | 0.029376% |

## Full sampled range

The following table includes all checked points in 1e-4 <= k <= 1 Mpc^-1, and all nine redshifts. It therefore answers a broader question than the focus-interval tables. Each k is in Mpc^-1.

| Model | CAMB 1.6.6 maximum | z | k | CAMB 2.0.4 maximum | z | k |
|---|---:|---:|---:|---:|---:|---:|
| Flat LambdaCDM | 0.140297% | 2 | 0.0383146 | 0.069197% | 2 | 0.0383146 |
| Curved LambdaCDM | 0.159183% | 2 | 0.038894 | 0.066727% | 0 | 0.00936425 |
| Evolving dark energy | 0.140659% | 2 | 0.0383188 | 0.068325% | 2 | 0.0383188 |

## Interpretation

CAMB2 has the smaller focus-interval maximum in **27 of 27** model–redshift combinations. It has the smaller focus-interval RMS in **27 of 27** combinations.

This tells us whether the earlier z=0 pattern persists at the other epochs. It does not mean CAMB2 is closer at every individual k. Differences in the summaries can also reflect which k points the solvers sample, not just changes in the physical model.

The result is evidence about agreement between these implementations, at these settings. It is not proof that CLASS is exact, or that one CAMB version is universally more accurate. The solvers keep different native recombination prescriptions, so we have not isolated pure numerical error.

For the thesis, we now have a systematic check across three representative models and nine times in their evolution. It is still preliminary solver validation, not emulator validation or complete coverage of Emmanuel's parameter and scale ranges.

## Verification and limits

### What did we check?

We checked that the intended inputs were actually read, that the units and density definitions matched, and that the saved CLASS table was retrieved without CBL’s additional interpolation. We also checked the background densities, the reconstruction of the power spectrum from component outputs, and the records identifying the files and solver versions.

Each new solver/model/redshift combination was run three times, and its saved spectra were identical across repeats. The expanded analysis also reproduces every original z=0 array and statistic exactly. This shows **repeatability**: the same setup gives the same answer.

It does not show **convergence**. A convergence test asks whether the answer remains stable when numerical settings are made more demanding. We did not perform that test in this experiment.

One small detail matters when checking the earlier epochs. CLASS can provide the power directly, or we can reconstruct it from its component fluctuations. These two routes agree to nearly rounding precision at z=0, but differ by up to **0.000192%** at the added redshifts, measured across the native output points. Our earlier-epoch consistency check allows up to **0.001%**; the original, much tighter z=0 check is unchanged. This threshold is a diagnostic choice, not a claimed accuracy of CLASS.

Why can the two routes differ slightly? Inside CLASS, the power and the component fluctuations are interpolated separately in time. Interpolation means estimating an output between computed time steps. Interpolating a fluctuation and then squaring it need not give exactly the same answer as interpolating its power directly. We verified these separate paths in CLASS's fourier.c (fourier_pk_at_z) and perturbations.c (perturbations_sources_at_tau). Our interpretation is that this tiny mismatch is consistent with those different output paths; it is not a measurement of the solver's physical error. The main comparison consistently uses the reconstructed synchronous matter power.

The detailed check results are in [audit.json](audit.json). Exact numerical differences, including their signs and locations, are in [statistics.json](statistics.json). These provide the unrounded results and detailed verification records.

### Scope of the conclusions

We checked a finite set of k values, so a larger difference could exist between them. CLASS also chooses slightly different points for different cosmologies. Therefore, the reported maxima are the largest differences *at the points we checked*, not guaranteed bounds over every possible k.

RMS gives equal weight to each saved point. Those points are not evenly spaced in log k, so this RMS is not the same statistic as one calculated on an evenly spaced logarithmic grid.

We have not tested every cosmology in Emmanuel’s training range, redshifts outside the nine listed here, or his full k range. We have not trained an emulator, run an MCMC inference analysis, or shown how these differences would affect parameter estimates.

We also have not identified the cause of every feature in the difference plots. This experiment does not support a three-solver speed ranking, because we did not perform a fair timing comparison here.

## Data and reproducibility

The numerical comparison arrays are supplied for the [flat](flat.npz),
[curved](curved.npz), and [evolving-dark-energy](evolving_de.npz) cases.
Each contains the nine redshifts and spectra at matched native CLASS points.
The original keys refer to z=0; keys such as k_z025 and class_z025 identify
z=0.25. Different models can have different sampled k grids.

The [statistics](statistics.json), [audit](audit.json), and
[provenance](provenance.json) retain the results, verification outcomes and
original file fingerprints. Historical paths refer to the calculation workspace.
The raw outputs, calculation scripts, native software and recovery snapshots
are retained separately and are available for a detailed methods review.
This repository contains the results for review; it is not a standalone
installation for rerunning the calculation.
