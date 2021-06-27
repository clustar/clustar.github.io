# Fit

Clustar module for fitting-related methods.

This module is designed for the `ClustarData` object. All listed methods take an input parameter of a `ClustarData` object and return a `ClustarData` object after processing the method. As a result, all changes are localized within the `ClustarData` object.

---

## `compute_fit`

Computes the normalized bivariate gaussian fit for the `Group` objects.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `compute_ellipse`

Computes the ellipse parameters and localized residuals for the `Group` objects.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `compute_metrics`

Computes the evaluation metrics for the `Group` objects.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `compute_peaks`

Computes the number of peaks along the major and minor axes for the `Group` objects.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `validate`

Determines which `Group` objects are flagged for manual review by using the specified validation parameters.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`
