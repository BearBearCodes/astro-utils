# astro-utils

Various utilities for doing astrophysics research. Originally compiled for my 2B co-op
term with Drs. Toby Brown, Joel Roediger, and Matt Taylor.

Note that some functions are not fully documented, but hopefully the code has sufficient
comments to be understandable. If there is demand for it, I can add `setup.py` files to
make using these packages easier.

If you use any of this code, please reference this GitHub repo. Thanks!

P.S. my fast(er) Voronoi binning code is super messy at the moment, hence I haven't
uploaded it to this repo yet. Let me know if you need it and I'll clean up the code ASAP.

## Quick Start

Simply download the repository:

```bash
git clone https://github.com/BearBearCodes/astro-utils.git
```

Or you can use [`svn`](https://subversion.apache.org/) to download just the folder(s) or
file(s) you need. For example:

```bash
svn export https://github.com/BearBearCodes/astro-utils/trunk/fits_utils.py
svn export https://github.com/BearBearCodes/astro-utils/trunk/radial_profile/
```

Notice the `tree/master` portion of the url has been replaced with `trunk`.

If you require version control, you can `svn checkout` instead of `svn export`.

## Highlights

### [`radial_profile` folder](radial_profile/)

See the [`README`](radial_profile/README_radial_profile.md) in the `radial_profile` folder
for more information. Briefly, this is a class for generating ellipses/rectangles for
radial profile calculations including support for high-inclination galaxies, inclination
corrections, and directional radial profiles.

### [`fits_utils.py`](fits_utils.py)

Contains several functions for processing astronomical data. Despite its name, most of
these functions do not require the data be in a FITS file. As long as the data are in
numpy arrays and you have `astropy` installed, most of this code should work fine.

Some included functions:

- `calc_pc_per_px()`: calculates the physical size of each pixel in parsecs per pixel
  dimension
- `cutout_to_target()`: cuts out an array to the extent of a target array and updates the
  array's WCS object
- `line_profile()`: e.g., for generating a major-axis profile of a galaxy. Uses
  nearest-neighbout sampling (no interpolation)
- `bin_sn_arrs_to_target()`: regularly bins a pair of signal and noise arrays to the exact
  extent + resolution of a target array. Requires the `reproject` package

### [`plot_utils.py`](plot_utils.py)

Contains several convenience functions for plotting with `matplotlib`. Some functions may
require the `seaborn` package.

Some included functions:

- `add_scalebar()`: adds a 1 kpc scalebar (by default) to the given axis. Credit goes to
  Dr. Toby Brown for making this function.
- `add_scalebeam()`: adds an ellipse representing an image's beam size to the given axis.
  Requires my [`radial_profiles_utils.py`](radial_profile/radial_profile_utils.py) file
- `add_annuli_RadialProfile()`: draws annuli on an given axis from a `RadialProfile`
  object
- `lognorm_median()`: for making median-normalized, log-scaled, RGB images. Default
  parameters should yield decent results (tested with NGVS data only)
- `joint_contour_plot()`: for making a snazzy contour plot with marginal plots showing the
  histograms and KDEs of the axes' quantities. Optionally superimposes a line of best fit
  onto the contour plot (intended for a least trimmed squares line, but any line in
  point-slope form will do)
- `set_aspect()`: robustly changes a subplot's aspect ratio (very useful for radial
  profile plots and mosaics)
