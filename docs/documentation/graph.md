# Graph

General module for graphing-related methods.

---

## `critial_points`

Returns the number of smoothed critical points along the specified axis.

### Parameters

- `image` : (`ndarray`) Data from the FITS image.
- `angle` : (`float`, optional) Degree of rotation used to specify the axis of differentiation.
- `smoothing` : (`int`, optional) Size of window used in the moving average smoothing process for peak evaluation.
- `clip` : (`float`, optional) Determines the percentage of tail values that are trimmed for peak evaluation.
- `center` : (`int`, optional) Defines the row-wise axis of differentiation for peak evaluation.

### Returns

`list`

---

## `identify_groups`

Displays the FITS image and identifies the groups in green, orange, or red rectangles, which are defined as: <br>
1. 'Green' denotes that the group is not flagged for manual review
2. 'Orange' denotes that the group is not flagged for manual review, 
   but the group is smaller than the beam size.
3. 'Red' denotes that the group is flagged for manual review.<br>
Beam size is the white oval shown on the bottom right corner of 
the FITS image.

### Parameters

- `cd` : (`ClustarData`) `ClustarData` object required for processing.
- `vmin` : (`float`, optional) Lower bound for the shown intensities. 
- `vmax` : (`float`, optional) Upper bound for the shown intensities.
- `show` : (`bool`, optional) Determines whether the groups should be identified. If `False`, the rectangles identifying the groups are not drawn.
- `dpi` : (`int`, optional) Dots per inch.
