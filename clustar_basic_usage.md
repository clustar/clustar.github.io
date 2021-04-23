# Clustar Basic Usage

The main clustar functionality is in the `search` module `Clustar` class, so first you must import that.


```python
from clustar.search import Clustar
```

Next, you need to set list the FITS files on which you'd like to perform the search. A simple way to do this is to store all FITS files of interest in a folder and use the `os` package to list all files in that directory. Python list comprehensions can be used to filter to only FITS files if there are other files included in the same folder.


```python
import os

fits_path = PATH_TO_FOLDER #update to where you have images saved
os.chdir(fits_path)
files = os.listdir()

#make sure only fits in list
files = [f for f in files if '.fits' in f]
```

Now, you're ready to run `Clustar`.


```python
cs = Clustar()
output = cs.run(files)
```

    File: 40/40 | Flagged: 21 

The output will update as the code runs, showing how many of the files have been processed and how many have been flagged as deviating from a bivariate Gaussian.

The output from Clustar is a list of dictionaries. Each element in the output contains the info on a single group detected within an image. The attributes are: 

* `'file'`: the name of the FITS file that the group came from.

* `'group'`: the group number (0-indexed) of the group within the image. So if an image has multiple groups, there will be multiple entries in the output associated with that file, and the groups will be differentiated by their group number (0, 1, 2, ...)

* `'bounds'`: the pixel location of the group within the original image.

* `'data'`: the pixel intensity values of the group extracted from the image.

* `'residuals'`: the residuals of each pixel location of the group with the bivariate Gaussian fit subtracted.

These attributes can be accessed with normal Python indexing, as shown below.


```python
output[0]['file']
```




    'HOPS-7_cont_robust2.pbcor.fits'




```python
output[0]['bounds']
```




    [477, 525, 470, 518]


