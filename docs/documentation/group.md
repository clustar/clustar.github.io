# Group

Clustar module for grouping-related methods.

This module is designed for the `ClustarData` object. All listed methods take an input parameter of a `ClustarData` object and return a `ClustarData` object after processing the method. As a result, all changes are localized within the `ClustarData` object.

---

## `arrange`

Arrange nonzero points from the denoised FITS image into `Group` objects by populating the 'bounds' parameter.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `rectify`

Convert the dimensions of the `bounds` into a square.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `merge`

Remove nested `bounds` from the `Group` objects.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `refine`

Delete `Group` objects that do not meet the specified thresholds.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `extract`

Extract FITS data using the `bounds` variable for each `Group` object.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `screen`

Screen for `Group` objects that appear to be outliers; remove `Group` objects that are dim and distant from the center of the FITS image.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `calculate`

Calculate statistics using the FITS data for each `Group` object.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`

---

## `detect`

Split binary groups into separate `Group` objects.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.

### Returns

`ClustarData`
