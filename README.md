<p align="center">
    <a>
	    <img src='https://img.shields.io/badge/python-3.10%2B-blueviolet' alt='Python' />
	</a>
    <a>
	    <img src='https://img.shields.io/badge/code%20style-black-black' />
	</a>
    <a href='https://opensource.org/license/lgpl-2-1'>
	    <img src='https://img.shields.io/badge/license-LGPLv2+-blue' />
	</a>
</p>

# 🌐 BiFIS

[**Signed Distance Function-biased flow importance sampling for implicit neural compression**]()<br/>
[Omar A. Mures](https://omaralv.com/), [Miguel Cid Montoya]()

## Usage

```python
from bifis.utils import Config, console
from bifis.sampling import Uniform, VariableDensity, BiFIS
from scipy.interpolate import griddata

# Load configuration
config = Config("config.json")

# Build toy deck
points, deck, path = build_toy_deck()

uniform = Uniform(config, args.resolution[0], args.resolution[1], np.arange(len(points)))
uniform.show(points[:, 0], points[:, 1], write=False)

variable_density = VariableDensity(config, args.resolution[0], args.resolution[1], np.arange(len(points)))
variable_density.show(points[:, 0], points[:, 1], write=False)

bifis = BiFIS(config, args.resolution[0], args.resolution[1], np.arange(len(points)), samples=points, surface=deck, surface_idx=np.arange(len(deck)))
bifis.show(points[:, 0], points[:, 1], write=False)

# Use resulting grid
interp_var_dens = griddata((points[:, 0], points[:, 1]), p_original, (variable_density.grid_x, variable_density.grid_y), method=interpolation_method)

# BiFIS uses cell ids instead of interpolation to get pixel data
fields_b_i = p_original[bifis.idx].reshape(bifis.img_shape)
```

## 📢 News

### May 2025

- Initial code version published.

### March 2025

- Code coming soon.

## 🎯 TODO

The repo is still under construction, thanks for your patience. 

- [ ] Release pip package.
- [x] Release of the sampling code.
