# CMB Lensing Visualizer

An interactive tool showing how gravitational lensing distorts the CMB
temperature power spectrum, and how that distortion changes with cosmology.

Two panels side by side: the lensed TT spectrum, and the lensing potential
$C_L^{\phi\phi}$ that lensed it. Sliders for the lensing amplitude $A_L$ and for
seven cosmological parameters, with real bandpowers available as an overlay.

**Live page:** `https://<your-username>.github.io/<repo-name>/`

---

## Publishing this

The page is a single self-contained HTML file — embedded fonts, embedded data,
no CDN, no build step, no JavaScript dependencies. Any static host will serve
it. For GitHub Pages:

```bash
# 1. make a new repo (a fresh one is easiest -- Pages is configured per repo)
mkdir cmb-lensing-visualizer && cd cmb-lensing-visualizer
git init -b main

# 2. copy this folder in
cp -r /path/to/lens_vis/docs .

# 3. commit
git add docs && git commit -m "CMB lensing visualizer"

# 4. create the repo on GitHub and push
gh repo create cmb-lensing-visualizer --public --source=. --push
#    (or make it on github.com and: git remote add origin <url> && git push -u origin main)
```

Then on GitHub: **Settings → Pages → Source: Deploy from a branch →
Branch: `main`, folder: `/docs` → Save.** It goes live in a minute or two at
`https://<username>.github.io/cmb-lensing-visualizer/`.

A new repo is worth it here rather than adding to an existing one: Pages is
configured per repository, so a dedicated repo keeps the URL clean and cannot
disturb anything else.

### What is in this folder

| file | |
|---|---|
| `index.html` | the visualizer |
| `.nojekyll` | stops GitHub's Jekyll step from touching the files |

All three TT datasets (Planck 2018, ACT DR6, SPT-3G D1) and the Planck lensing
bandpowers are public data releases, embedded in the file with attribution
below.

---

## Credits

Please keep these if you republish.

**Data**

- Planck 2018 `plik_lite` TT bandpowers and lensing reconstruction —
  Planck Collaboration, *Planck 2018 results. V. CMB power spectra and
  likelihoods*, A&A 641, A5 (2020), and *VIII. Gravitational lensing*,
  A&A 641, A8 (2020).
- ACT DR6 foreground-marginalised TT bandpowers — ACT Collaboration DR6.
- SPT-3G D1 TT bandpowers — SPT-3G Collaboration, via the `candl` likelihood
  framework.

**Software**

- Theory spectra from [CAMB](https://camb.info) (Lewis & Challinor).
- Bandpowers read through `planck_lite_py`, `sacc`, and `candl`.

**Typography**

- CMU Serif (Computer Modern Unicode), SIL Open Font License.
- `cmmi10` and `cmsy10`, Computer Modern math fonts after D. E. Knuth,
  as distributed with matplotlib. Subset and remapped to Unicode codepoints
  for this page.

---

## Rebuilding

Requires the `clusterarc-tools` environment and the analysis directory:

```bash
conda activate clusterarc-tools
cd project2/lens_vis

python warp_precompute.py                    # A_L grid          (~5 s)
python warp_cosmo.py --validate 10           # cosmology grid    (~1 min)
python warp_viz.py                           # -> lens_warp.html
cp lens_warp.html docs/index.html

# --exclude drops a dataset from the file entirely, should one ever need it:
#   python warp_viz.py --exclude spt --out docs/index.html

python -m pytest test_warp.py -q             # 37 tests
```

### How the cosmology sliders work

A browser cannot run CAMB, and a full seven-dimensional grid is impossible. So
each parameter is precomputed on its own grid with the others held fixed, and
the deviations are superposed:

$$D_\ell(\theta) \approx D_\ell(\text{fid}) + \sum_p \Delta_p(\theta_p)$$

Each parameter's own non-linearity is captured **exactly**; only cross-terms
between parameters are dropped. Measured against exact CAMB at random points,
the cost of that approximation is a median of 1.1% of the TT peak (worst 5.2%)
with all seven sliders moved simultaneously, and zero when only one is moved.

Because cosmology changes the lensing potential as well as the primary
spectrum, the deviations are stored at several values of $A_L$ and interpolated
between them.
