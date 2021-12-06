# Radio Profile Utilities

Generate ellipses/annuli to create radial profile plots of radio or other wavelength data.
Includes support for high-inclination galaxies and directional radial profiles.

There may be bugs or other unexpected behaviour, in which case just ping me on Slack, send
me an email, open a GitHub issue, etc. Also let me know if there are typos in any
documentation.

Currently, the documentation for directional radial profiles is really sparse. I will
finish the docstrings eventually. Until then, just ping me if you have difficulty
using or understanding the directional radial profiles.

## Requirements

The following packages are required:

1. `astropy`
2. `matplotlib`
3. `numpy`
4. `photutils`
5. `radio_beam`
6. `copy` (a Python standard library)

## Quick Start

1. Download the repository

   ```bash
   git clone https://github.com/BearBearCodes/astro-utils.git
   ```

2. Look at [`radial_profile_example.ipynb`](radial_profile_example.ipynb)

3. Profit?

As always, feel free to reach out if you need any help or clarification using this
package. In particular, the documentation for how we treat high-inclination galaxies is
unbelievably word-vomity, but I should be able to resolve any confusion if you ask on
Slack, GitHub, email, etc.

## Roadmap

- Implement an option to not save a copy of the area masks in radial profile object,
  thereby decreasing memory usage
