# How I chose the baseline inputs

These notes record the settings used for the first flat-model comparison on
22 October 2026. I wanted to follow Emmanuel's Figure 4.3, but the thesis does
not give its complete input file. I therefore used the values it documents
and recorded the remaining choices here.

The [current report](../01_current_solver_comparison/results/Report.md)
extends that baseline to the other models and redshifts.

## What the thesis gives us

The source checks used the original PDF, stored locally as
`Resources/sources/emmanuel.pdf`, alongside its Markdown conversion:

- Table 4.1 is on printed page 57, PDF page 62. It gives the training settings
  listed below, but ranges rather than a single value for the matter densities.
- Figure 4.3 and its caption are on printed page 62, PDF page 67. They identify
  Planck best-fit parameters, z=0, Omega_k=0.01 for the curved model, and
  w0=-0.99, wa=-0.02 for evolving dark energy. The axes use h-scaled units,
  unlike the physical-k training table. The caption does not give the exact
  baryon and cold-dark-matter inputs.
- Section 1.3.1 gives omega_b=0.0224 and omega_cdm=0.120 as rounded
  observational values. It does not explicitly identify them as the inputs
  used in Figure 4.3.
- Table 5.1, on printed page 64, PDF page 69, gives a separate mock-data
  cosmology. It cannot be assumed to be the Figure 4.3 reference cosmology.

### Settings in Table 4.1

| Quantity | Value |
|---|---:|
| z | 0 |
| h | 0.6736 |
| A_s | 2.1e-9 |
| n_s | 0.9649 |
| tau | 0.0544 |
| Omega_b | range [0.03, 0.06], not one fiducial value |
| Omega_cdm | range [0.07, 0.70], not one fiducial value |
| Omega_nu | 0 |
| Omega_k | 0 |
| w0, wa | -1, 0 |
| k, physical Mpc^-1 | [1.5e-4, 75] |

There is also an unresolved detail in the radiation density. Table 4.1 labels
9.26535e-5 as Omega_gamma and uses it in flat closure. It is not clear whether
this is meant to describe photons alone or the total relativistic density.
Using it as a photon density and then adding neutrino radiation could count
part of the radiation twice. This needs clarification before claiming an
exact match to the original inputs.

## Why I did not use the CBL preset directly

The installed CBL Planck18 preset is not the same as a confirmed input file
from Emmanuel. In the pinned `Cosmology/Lib/Cosmology.cpp`, around line 167,
it sets Omega_m=0.3153, Omega_b=0.0486, h=0.6736, one massive neutrino with
sum m_nu=0.06 eV, and 2.04 massless species.

Those values cannot simply be taken as his settings, especially for a
comparison without massive neutrinos. I used the executable preset values
for this check rather than relying on nearby comments.

## Settings used here

I used the fixed parameters from Table 4.1 and the rounded physical densities
omega_b=0.0224 and omega_cdm=0.120 from Section 1.3.1, with zero massive
neutrinos. I kept TCMB=2.7255 K, Neff=3.044, YHe=0.245 and pivot=0.05 Mpc^-1
from the earlier comparison. These last choices are recorded assumptions;
they are not documented inputs for Figure 4.3.

Radiation was calculated consistently from TCMB and Neff, and the dark-energy
density was set to close the flat background. Each solver kept its native
precision and recombination settings. The k range was also kept narrower
than the thesis's full range.

This baseline was adopted on 22 October 2026. Three fresh runs per solver
passed the repeatability and input/output checks. CLASS was called through
CBL's Table_PkCodes interface, using its original k points without extra CBL
interpolation. That first step covered the flat model at z=0; curvature,
evolving dark energy and MCMC were not included in it.

An exact reproduction still needs Emmanuel's input file or clarification of
the settings missing from the thesis.
