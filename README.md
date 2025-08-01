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
	<a href="https://colab.research.google.com/drive/1opg23nw1WNMYAnKL1w6t6oF8-xCHJcqr">
		<img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
	</a>
</p>

# 🌐 BiFIS

[**Signed Distance Function-biased flow importance sampling for implicit neural compression of flow fields**]()<br/>
[Omar A. Mures](https://omaralv.com/), [Miguel Cid Montoya](https://mcidmontoya.com/)

## Usage

### Installation

The `bifis` library is available using pip. We recommend using a virtual environment to avoid conflicts with other software on your machine.

``` bash
pip install bifis
```

To run the demo:

```bash
python main.py -c configs/toy_case.json
```

To create a conda env for development:

```bash
conda env create -f environment.yaml
```

### Example

Sample configuration:

```json
{
    "domain": [
        0.0,
        1.0,
        0.0,
        1.0
    ],
    "counts": [
        [
            4,
            72,
            4
        ],
        [
            8,
            24,
            8
        ]
    ],
    "roi": [
        0.15,
        0.85,
        0.25,
        0.75
    ],
    "preserve_surface": false
}
```

`domain` is an array containing `xmin`, `xmax`, `ymin`, `ymax` coordinates representing the limits of the simulation domain. `counts` contains the number of samples per section and `roi` represents the min and max coordinates of each dimension (it follows the `domain` convention) when using `VariableDensity` sampling. `preserve_surface` lets the user choose to always preserve samples that belong to the surface of interest and choose the rest using the SDF when using `BiFIS` sampling.

Sample run script:

```python
from bifis.utils import Config, console
from bifis.sampling import Uniform, VariableDensity, BiFIS
from scipy.interpolate import griddata

# Load configuration
config = Config("config.json")

# Get simulation data
cell_centers, surface = ...

uniform = Uniform(config, args.resolution[0], args.resolution[1], np.arange(len(cell_centers)))
uniform.show(cell_centers[:, 0], cell_centers[:, 1], write=False)

variable_density = VariableDensity(config, args.resolution[0], args.resolution[1], np.arange(len(cell_centers)))
variable_density.show(cell_centers[:, 0], cell_centers[:, 1], write=False)

bifis = BiFIS(config, args.resolution[0], args.resolution[1], np.arange(len(cell_centers)), samples=cell_centers, surface=surface, surface_idx=np.arange(len(surface)))
bifis.show(cell_centers[:, 0], cell_centers[:, 1], write=False)

# Use resulting grid
uni_pixel_data = griddata((cell_centers[:, 0], cell_centers[:, 1]), p_original, (uniform.grid_x, uniform.grid_y), method=interpolation_method)
var_dens_pixel_data = griddata((cell_centers[:, 0], cell_centers[:, 1]), p_original, (variable_density.grid_x, variable_density.grid_y), method=interpolation_method)

# BiFIS uses cell ids instead of interpolation to get pixel data
bifis_pixel_data = p_original[bifis.idx].reshape(bifis.img_shape)
```

## Google Colab Tutorial

1. Hello world! A simple sampling example <a href="https://colab.research.google.com/drive/1opg23nw1WNMYAnKL1w6t6oF8-xCHJcqr">
		<img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/>
	</a>
   - Learn how to use the different sampling classes.
   - Visualize their differences.

## 📢 News

### July 2025

- Colab example released.

### May 2025

- Initial code version published.
- Pip package released.

### March 2025

- Code coming soon.

## 🎯 TODO

The repo is still under construction, thanks for your patience. 

- [x] Release Colab example.
- [x] Release pip package.
- [x] Release of the sampling code.

## 📜 Citation

```
@Article{mures2025signed,
  author = {Mures, Omar A. and Cid Montoya, Miguel},
  title = {Signed distance function-biased flow importance sampling for implicit neural compression of flow fields},
  journal = {Computer-Aided Civil and Infrastructure Engineering},
  volume = {40},
  number = {17},
  pages = {2434-2463},
  year = {2025},
  doi = {10.1111/mice.13526},
  url = {https://doi.org/10.1111/mice.13526},
}
```
