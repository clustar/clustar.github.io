# About

### Clustar

Release: 1.1.10

Date: April 23, 2021

Addiitonal documentation is available at the following links:

* [Basic usage](https://clustar.github.io/basic_usage)

* [Examples](https://clustar.github.io/examples)

### Installation

Clustar is available on [PyPI](https://pypi.org/project/clustar/0.0.1/) and can be installed using pip.

```python
pip install clustar
```

### Overview

The motivation for using clustar is to identify prostars/ protoplanetary disks found from FITS files. These files are represented as 2d array's containing intensities at each point along with the header information about the telescope observational parameters. Clustar simplifies and expediates the identification pipeline of FITS files by automating the preprocessing, grouping, and fitting for a group of FITS files. Clustar is optimized in its codebase primarily in its efficacy in identifying non-bivariate Gaussian like substructures and is tested upon the Tobin dataset.


#### Example

![](readme_Images/image%201.png)

### Preprocessing

Clustar crops the input image from a square dimension to a circle. This is done to alleviate the higher noise around the edges of the image. After cropping, clustar utilizes a technique known as sigma clipping to filter out the data points that are 5 times the RMS statistic.

![](readme_Images/image%202.png)
### Grouping

![](readme_Images/image%203.png)

### Fitting

![](readme_Images/image%204.png)
### Summary

Clustar should be utilized to identify protostars and other potential celestial objects that are suspected to be non-bivariate Gaussian. The t-SNE clustering methods further identify images with substructures that may be of interest. Anyone that works with FITSits files can utilize the different methods independently or as a pipeline to preprocess, group, fit, and cluster their data.
