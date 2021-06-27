# Denoise

Clustar module for denoising-related methods.

This module is designed for the `ClustarData` object. All listed methods take an input parameter of a `ClustarData` object and return a `ClustarData` object after processing the method. As a result, all changes are localized within the `ClustarData` object.

---

## `crop`

Crops the square FITS image into a circle.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.
- `radius_factor` : (`float`, optional) Factor multiplied to radius to determine cropping circle in the denoising process; must be within the range [0, 1].
- `apply_gradient` : (`bool`, optional) Determine if the FITS image should be multiplied by a gradient in order to elevate central points; similar to multiplying the FITS image by the associated 'pb' data.

### Returns

`ClustarData`

---

## `compute_noise`

Computes the noise level by evaluating the root mean square (RMS) metric at the specified quantile on the composed grid.

- `cd` : (`ClustarData`) `ClustarData` object required for processing.
- `n` : (`int`, optional) Number of chunks to use in a grid; must be an odd number.
- `quantile` : (`float`, optional) Quantile of RMS to determine the noise level; must be within the range [0, 1].

### Returns

`ClustarData`

---

## `resolve`

Performs the complete denoising operation on the FITS image.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`
