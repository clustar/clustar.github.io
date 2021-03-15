# Clustar

**Release:** 0.0.1

**Date:** March 15, 2021 


## Import

-- How to install clustar --

```{python}
pip install clustar
```

## Usage

-- How to run clustar --

```{python}
from clustar.search import Clustar
import os

# Change directory to folder containing FITS files.
fits_path = r"C:\Users\pavan\Projects\Capstone\Tobin_Data"
os.chdir(fits_path)
files = os.listdir()

# Run clustar algorithm to extract groups.
cs = Clustar()
output = cs.run(files)

```

## Output

-- Visualize output --

```{python}
from matplotlib import pyplot as plt
from astropy.io import fits

# Plot each FITS file that contains abnormal groups.
for file_path in output:
    file = fits.open(file_path)
    image = file[0].data[0,0,:,:]
    file.info()
    plt.imshow(image, origin='lower')
    plt.colorbar()
    plt.show()
```
