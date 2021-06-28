# Getting Data from ALMA

While we used an existing [dataset](https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/JETGZC) from a study by Tobin et al. for our paper, most other researchers interested in our package will want to download data from the [ALMA Science Archive](https://almascience.nrao.edu/aq/). Here we provide some guidance on how to do so programmatically.

The following packages will be used:


```python
import pandas as pd
import tarfile
import shutil

import astroquery
from astroquery.alma import Alma
from astropy.table import Table
alma = Alma()
alma.archive_url = 'https://almascience.nrao.edu'
```

There are three main steps to the process:

1. Get a list of all MOUS IDs for a given science keyword

2. Get a list of file links associated with those MOUS IDs

3. Download the FITS file from each link

We wrote functions for each step with options based on our desired data, but some slight modifications to these functions may be needed depending on intended usage.

### Get MOUS IDs for a Science Keyword

ALMA data is identified by a member ous ID (MOUS ID). We can search the archive by science keyword to get a list of the MOUS IDs associated with that keyword. Note that the science keywords must exactly match one of the keywords listed in the archive's dropdown menu. For example "Outflows, jets and ionized winds" is a valid science keyword, but "outflows" alone will not work. In our code, we also include an option to return only data after a specified year in the results.


```python
def get_mous(science_keyword, save_file_path = None, min_year = None):
    """Get all mous IDs for a given science keyword
    Returns list of mous IDs as strings
    science_keyword: string search keyword
    save_file_path: optional path to save csv of results
    min_year: optional param to filter results to only those after a certain year;
        can be string or int;
        current min year in archive is 2011
    """

    #query alma
    full_query = "select * from ivoa.obscore where science_keyword = '{}'".format(science_keyword)
    query_results = Alma.query_tap(full_query)

    #convert results to df and clean up
    result_df = query_results.to_table().to_pandas()
    result_df.loc[:, result_df.dtypes == object] = result_df.loc[:, result_df.dtypes == object].apply(lambda x: x.str.decode('utf-8'))

    #filter results if desired
    if min_year is not None:
        if type(min_year) != int:
            min_year = int(min_year)
        result_df = result_df[result_df['proposal_id'].str[0:4].astype(int) >= min_year]

    #save results if desired
    if save_file_path is not None:
        result_df.to_csv(save_file_path, index = False)

    return(result_df['member_ous_uid'].unique())
```


```python
mous = get_mous('Outflows, jets and ionized winds', FILE_PATH, min_year = 2018)
```

### Get FITS Links

Next, we gather the FITS file links associated with each MOUS ID. Observations less that one year old are not available to the public and will return an error. Additionally, we were interested in only continuum FITS images and filtered the links accordingly. However, other researchers may need to customize this trimming/filtering process according to their specific needs.


```python
def get_fits_links(mous_list, trim = True):
    """Get file links from mous IDs
    Returns list of links to fits files
    mous_list: list of mous IDs (strings)
    trim: filter to just continuum fits files or not
    """

    all_links = pd.DataFrame()
    error_ids = []
    for mous_id in mous_list:
        try:
            mous_links = alma.stage_data([mous_id], expand_tarfiles=True)['URL']
            all_links = all_links.append(pd.DataFrame(mous_links))
        except:
            error_ids.append(mous_id)

    if trim:
        trimmed_links = all_links[all_links['URL'].str.contains('cont') & all_links['URL'].str.contains('fits.tar')]['URL']

        return trimmed_links, error_ids
    else:
        return all_links, error_ids
```


```python
links, _ = get_fits_links(mous)
```

### Download FITS Files from Links

Finally, we download the FITS files. The files initially are downloaded as zipped tarballs. When unzipped, the actual .fits file is buried a couple levels down in a nested folder structure. We added some clean up functionality to optionally pull the .fits files out of the subfolders into the root of the specified directory and to delete the .tar files after unzipping. If using this functionality, be very careful about your current working directory (or specified cache location), as this will impact all .tar files in the given location.


```python
def download_all_fits(fits_links, cache_location = None, unzip=True, unnest=True, del_tar=False):
    """Download and optionally unzip all FITS files from a list of mous IDs
    mous_list_loc: path to csv with mous IDs for a given search term
    cache_location: path to save downloaded files to (default current working directory)
    unzip: binary whether or not to unzip tar files
    unnest: if true, move fits files out of nested subpath
    del_tar: if true, remove tar files after unzipping
    """

    #set location to download to
    if cache_location is None:
        cache_location = os.getcwd()
    alma.cache_location = cache_location # --> if you want to download to a specific directory

    error_links = []
    #download fits files to that location
    for link in fits_links:
        try:
            alma.download_files([link])
        except:
            error_links.append(link)

    #unzip files
    if unzip:
        #get list of tar files in directory
        dir_files = os.listdir()
        tar_files = [s for s in dir_files if s.endswith('.tar')]

        for tar_file in tar_files:
            #unzip file
            tar = tarfile.open(tar_file)
            tar_names = tar.getnames()

            tar.extractall(cache_location)
            tar.close()

            if unnest:
                for name in tar_names:
                    #move fits file out of subfolder
                    shutil.move(name, name.split('/')[-1])
                    #delete now-empty subfolder
                    shutil.rmtree(name.split('/', 1)[0] + '/')
            if del_tar:
                #delete tar file
                os.remove(tar_file)

    return error_links
```


```python
download_all_fits(links)
```
