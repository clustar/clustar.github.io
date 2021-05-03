# About

### Clustar

Release: 1.1.10

Date: April 23, 2021

Additional documentation is available at the following links:

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

Our Clustar package uses the following steps to automate the search for interesting celestial objects. 
From a raw FITS file, we apply the following steps:
1.	Denoise the image.
2.	Arrange intensities into groups. 
3.	Fit a model bivariate gaussian.
4.	Analyze the residuals to determine whether or not a group should be flagged for manual review.

On the left, we have a FITS file as our input. On the right, we have a depiction of the output, which appropriately labels the groups. The detection shown in the green box conforms to a bivariate gaussian, whereas the detection shown in the red box is flagged for manual review.

### Preprocessing

Clustar crops the input image from a square dimension to a circle. This is done to alleviate the higher noise around the edges of the image. After cropping, clustar utilizes a technique known as sigma clipping to filter out the data points that are 5 times the RMS statistic.

![](readme_Images/image%202.png)

### Grouping

![](readme_Images/image%203.png)

From the denoised image, the nonzero intensities are arranged into groups, based on their proximity. Here, we identify two detections labeled as group 1 and group 2. We compute the model bivariate gaussian by using the elementary statistics calculated from the extracted group data.

### Fitting

![](readme_Images/image%204.png)

The residuals are obtained by subtracting the fit from the group data. Next, we assess the absolute value of the residuals. Because we are not interested in the residuals outside of the general bivariate gaussian shape, we only consider the residuals inside of this default ellipse. Finally, we calculate the variance metric for each of these groups. Since the first group exceeds the default threshold of 0.025, we flag this group for manual review. We do not flag the second group since it does not exceed this threshold.

### Summary

Clustar should be utilized to identify protostars and other potential celestial objects that are suspected to be non-bivariate Gaussian. The t-SNE clustering methods further identify images with substructures that may be of interest; nonetheless, this package is open to anyone that works with FITS files. They can utilize our package methods independently or as a pipeline to preprocess, group, fit, and cluster their data.
