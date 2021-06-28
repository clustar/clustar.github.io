# Usage

## FITS Files from ALMA Science Archive

Visit this [page](../examples/alma.html) to learn more on how to download FITS files from the [ALMA Science Archive](https://almascience.nrao.edu/aq/).

Click [here](../examples/alma.ipynb) to download this notebook.

## Singular FITS File

Detect celestial objects in a singular FITS image by creating a `ClustarData`
object.

```
from clustar.core import ClustarData

# Create the 'ClustarData' object by specifying the path to FITS file.
cd = ClustarData(path='~/data/example.fits', threshold=0.025)

# Visualize the detected groups.
cd.identify()

# Access individual 'Group' objects.
cd.groups
```

## Multiple FITS Files

Detect celestial objects in a directory containing multiple FITS images by
creating a `Clustar` object.

```
from clustar.search import Clustar

# Setup 'Clustar' object.
cs = Clustar(radius_factor=0.95, threshold=0.025)

# Execute pipeline on directory containing FITS files.
cs.run(directory='~/data/')

# Access individual 'ClustarData' objects.
cs.data

# Check which FITS files raised an error.
cs.errors

# Inspect 'ClustarData' variables for all groups in each FITS file.
cs.display(category='all')
```

## t-SNE on Clustar Output

Visit this notebook to learn more on how to apply t-SNE on the extracted groups from the Clustar package.
