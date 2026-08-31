# CMB Lensing Visualizer

### → **[Open the visualizer](https://joannjones415.github.io/cmb-lensing-visualizer/)**

An interactive tool for seeing what gravitational lensing does to the cosmic
microwave background, separately from what cosmology does to it.

Lensing by intervening large-scale structure smooths the acoustic peaks of the
CMB temperature spectrum and transfers power into the damping tail. Here the
lensing potential is deformed independently of the cosmology that generated it,
so the two effects can be varied one at a time — something a normal Boltzmann
code will not let you do, because it always lenses a cosmology with its own
potential.

**Two parameterizations, one toggle apart:**

- **A<sub>L</sub>** — a single amplitude rescaling C<sub>L</sub><sup>φφ</sup> at
  every L equally. A<sub>L</sub> = 1 is the self-consistent prediction,
  A<sub>L</sub> = 0 the unlensed primary spectrum.
- **θ₁, θ₂** — the first two lensing principal components, which carry most of
  the peak-smoothing information and deform the potential with a *shape* rather
  than a constant.

A strip under the lensing panel plots the deformation actually being applied.
In A<sub>L</sub> mode it is a flat line; in PC mode it is a structured curve
peaking near L ≈ 86 and L ≈ 296. That difference is the reason a peak-smoothing
amplitude and a direct lensing reconstruction can disagree while describing the
same sky.

Seven cosmological parameters have their own sliders, and real bandpowers can be
overlaid.

---

Theory spectra computed with [CAMB](https://camb.info).
Bandpowers from Planck 2018 (plik-lite and lensing), ACT DR6, and SPT-3G D1.
Typeset in Computer Modern.

Joann Jones · [joannjones@uchicago.edu](mailto:joannjones@uchicago.edu)
