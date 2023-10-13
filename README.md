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

### [`hierarchical_vorbin_example.ipynb`](hierarchical_vorbin_example.ipynb)

This is an example of hierarchical Voronoi binning, as described in the
[`vorbin` documentation](https://www-astro.physics.ox.ac.uk/~cappellari/software/vorbin_manual.pdf).
Note that you will need [vorbin](https://pypi.org/project/vorbin/) to run this notebook.
In most cases, I would imagine this to be more useful than my fast Voronoi binning code,
as my "fast" Voronoi binning code is only a few times faster than the standard `vorbin`
implementation, but this hierarchical method can be faster than standard Voronoi binning
by orders of magnitude. I may make this process into a proper Python function/class
sometime in the future, and maybe I will parallelize certain steps with
[Dask](https://docs.dask.org/en/stable/)\*.

\* I think parallelizing the hierarchical Voronoi binning code is only useful if there are
many disconnected regions in the final step where we Voronoi bin previously-unbinned
full-resolution data. We cannot split up a contiguous region and process them in parallel
because how we divide up the original data will affect the locations and shapes of the
Voronoi cells (i.e., no Voronoi cell can cross a boundary where we split the data). I
think having many disconnected full-resolution regions that we will need to Voronoi bin at
the last step will be rare. Recall that if we need to Voronoi bin any data in this last
step, it implies that the signal-to-noise ratio (SNR) of the original data were quite high
to begin with. The SNR usually peaks in the centre of the object and changes (relatively)
smoothly over the image, so having sudden pockets of high SNR seems unlikely (as opposed
to just one big region that we need to Voronoi bin, in which case we cannot split up the
region to process it in parallel\*\*). Hence I am not sure parallelizing the code with
Dask will actually be important in most cases, but please let me know if your use case
would benefit greatly from having this feature.

\*\* If most of your re-binned image ends up being untouched by Voronoi binning (meaning
you will need to Voronoi bin the full-resolution data in these regions), you may want to
decrease your bin size used in the regular binning step, which will yield a re-binned
image where the binned pixels/spaxels have a lower SNR than the initial re-binning. As
Voronoi binning tries to only touch data that have an SNR less than the chosen threshold,
more of the data will be processed in the initial Voronoi binning step, leaving fewer
full-resolution pixels/spaxels to Voronoi bin in the second step.

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
