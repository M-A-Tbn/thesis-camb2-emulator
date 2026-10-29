# Emmanuel baseline: source audit, 22 October 2026

The comparison adopts an explicitly labelled thesis-informed baseline, with
the source checks and remaining uncertainties recorded below. See the
[current results](../01_current_solver_comparison/results/Report.md).
The exact input file behind Figure 4.3 has not been recovered.

## Verified sources

- Original PDF: `Resources/sources/emmanuel.pdf`, Table 4.1, printed p. 57
  (PDF page 62), visually verified against the Markdown conversion.
- Figure 4.3 and its caption, printed p. 62 (PDF page 67), visually verified.
- Section 1.3.1 in the full conversion gives rounded physical densities
  omega_b = 0.0224 and omega_cdm = 0.120 as observational values, not explicitly
  as the exact input pair used for Figure 4.3.
- Table 5.1, printed p. 64 (PDF page 69), gives a separate mock-data cosmology.
  It must not be substituted silently for Figure 4.3's Planck-reference cosmology.

## Table 4.1: documented training configuration

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

The table labels 9.26535e-5 as Omega_gamma and uses it in flat closure.
Its interpretation as photon-only versus total relativistic density must be
resolved before claiming exact solver-input matching; do not silently set it
as a photon density while also adding a separate neutrino radiation density.

Figure 4.3 names Planck best-fit parameters, z=0, Omega_k=0.01 for the curved
case, and w0=-0.99, wa=-0.02 for the evolving-dark-energy case. It uses
h-scaled axes, unlike the physical-k training table. It does not give a complete
input file or an explicit exact baryon/CDM pair in its caption.

## Why the installed CBL preset is insufficient evidence

The pinned CBL `Cosmology/Lib/Cosmology.cpp`, Planck18 branch around line 167,
sets Omega_m=0.3153, Omega_b=0.0486, h=0.6736, one massive neutrino with
sum m_nu=0.06 eV, and 2.04 massless species. This is not automatically the
configuration Emmanuel used, particularly after any massless-neutrino overrides.
Comments near these values are not substitutes for executable values or the
author's actual parameter file.

## Adopted reconstruction

Use a clearly labelled thesis-informed reconstruction of Figure 4.3's flat
baseline: Table 4.1's fixed parameters, rounded omega_b=0.0224 and
omega_cdm=0.120 from Section 1.3.1, zero massive neutrinos, and explicitly retain
the earlier comparison's common TCMB=2.7255 K, Neff=3.044, YHe=0.245 and
pivot=0.05 Mpc^-1 as our choices rather than documented Figure 4.3 inputs.
Compute radiation consistently from TCMB/Neff and close the flat background.
Keep native solver precision/recombination prescriptions and the current limited
comparison range initially; do not claim reproduction of the thesis's full k range.

This reconstruction was adopted on 22 October 2026. Three fresh runs per solver
passed repeatability and input/output
checks. CLASS was launched through CBL's Table_PkCodes interface without additional
CBL interpolation; comparisons use its native k points. Exact reproduction still
requires Emmanuel's input configuration or clarification of the missing settings.
No MCMC, curved case or dark-energy extension was run in this step.
