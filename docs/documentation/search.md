# Search

Contains the `Clustar` hierarchical class, which is responsible for transforming all available FITS images in a specified directory into their 
respective `ClustarData` objects. `Clustar` class uses multithreading to expedite the processing of the FITS images.

---

## `Clustar`

A class for executing the entire pipline for detecting groups from a directory of FITS files and for storing all relevant data associated with each file.

### Parameters

`**kwargs` : (optional) Same as `~clustar.core.ClustarData.params` arguments. Copy of that documentation is listed below for convenience.

### Keywords

- `radius_factor` : (`float`) Factor mulitplied to radius to determine cropping circle in the denoising process; must be within the range [0, 1].
- `chunks` : (`int`) Number of chunks to use in a grid; must be an odd number.
- `quantile` : (`float`) Quantile of RMS to determine the noise level; must be within the range [0, 1].
- `apply_gradient` : (`bool`) Determine if the FITS image should be multiplied by a gradient in order to elevate central points; similar to multiplying the FITS image by the associated 'pb' data.
- `sigma` : (`float`) Factor multiplied to noise level to determine the cutoff point, where values less than this threshold are set to zero.
- `alpha` : (`float`) Determines the size of the ellipse in relation to the chi-squared distribution; must be within the range (0, 1).
- `buffer_size` : (`int`) Number of points considered outside of the group range. For instance, given a 1-d group range of [10, 20], the algorithm checks for nonzero points within the range [5, 25] when the 'buffer_size' is 5.
- `group_size` : (`int`) Minimum number of nonzero points that determines a group.
- `group_factor` : (`float`) Ratio between [0, 1] that specifies the minimum number of nonzero points that determines a group in relation to the number of nonzero points in the largest group.
- `metric` : (`str`) Method used for evaluating the groups; must be one of the following: `"standard_deviation"`, `"variance"`, `"average"`, or `"weighted_average"`.
- `threshold` : (`float`) Cutoff point that determines which groups are flagged for manual review, given the specified metric. 
- `split_binary` : (`bool`) Experimental; determine whether binary subgroups identified within a group should be split into individual groups.
- `subgroup_factor` : (`float`) Experimental; ratio between [0, 1] that specifies the subgroup range in terms of the absolute maximum intensity.
- `evaluate_peaks` : (`bool`) Experimental; determine whether the peaks of the output residuals should be taken into consideration in the flagging process.
- `smoothing` : (`int`) Experimental; size of window used in the moving average smoothing process for peak evaluation.
- `clip` : (`float`) Experimental; determines the percentage of tail values that are trimmed for peak evaluation.

### Attributes

- `data` : (`list`) List of `ClustarData` objects corresponding to the FITS file in the directory.
- `errors` : (`list`) List of FITS files that raised an error in the processing.


### Examples

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

---

## `run`

Main `Clustar` method for executing the pipeline for group detection.

### Parameters

- `directory` : (`str`) Path to folder containing FITS files.

### Raises

`AssertionError` : If there are no FITS files at the specified directory.

---

## `display`

Generates a Pandas data frame object containing the specified variables.

### Parameters

- `category` : (`str`, optional) If category is `"summary"`, then a subset of `ClustarData` variables are returned in the data frame. If category is `"all"`, then all  singular `ClustarData` variables are returned in the data frame.
        
### Returns

`DataFrame`
