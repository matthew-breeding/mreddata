# mreddata

Version 0.1.0:

To install: `pip install mreddata`. 

The following command line arguments are used to properly initialize the data:
```
  -h, --help            show this help message and exit
  -f FILES [FILES ...], --files FILES [FILES ...]
                        Only load data from the HDF5 files listed after this
                        flag (rather than every HDF5 in the directory)
  --raw                 only return the raw data, with no normalization - post
                        processing applied (by default, data is normalized to 
						the gun fluence unit and nIons)
  --diff                differntial data (default is reverse-integrated)
  --fullpath, --includeFilenames
                        include the full filename
  -x, --no-load         Only load the file/histogram names into memory, not
                        the histogram data. Usefull for files with a large
                        number of histograms
  -e, --include-error   include error bars in the plot
```
 

`options` and `plot_options` handle the file and plotting attributes, respectively. 
    >>> from mreddata import options, plot_options
    >>> options.files
{: .language-python}
```
['250nmAl_250nmAl_10keV000_1.hdf5', '250nmAl_250nmAl_50keV000_19.hdf5', '250nmAl_250nmAu_10keV000_1.hdf5', '250nmAl_250nmAu_50keV000_19.hdf5', '250nmAl_250nmCo_10keV000_1.hdf5', '250nmAl_250nmCo_50keV000_4.hdf5', '250nmAl_250nmCu_10keV000_1.hdf5', '250nmAl_250nmCu_50keV000_19.hdf5', '250nmAl_250nmRu_10keV000_1.hdf5', '250nmAl_250nmRu_50keV000_19.hdf5', '250nmAl_250nmW_10keV000_1.hdf5', '250nmAl_250nmW_50keV000_19.hdf5', '250nmAu_250nmAl_10keV000_1.hdf5', '250nmAu_250nmAl_50keV000_19.hdf5', '250nmCo_250nmAl_10keV000_1.hdf5', '250nmCo_250nmAl_50keV000_19.hdf5', '250nmCo_250nmCu_50keV000_19.hdf5', '250nmCu_250nmAl_10keV000_1.hdf5', '250nmCu_250nmAl_50keV000_19.hdf5', '250nmCu_250nmCo_10keV000_2.hdf5', '250nmCu_250nmCo_50keV000_19.hdf5', '250nmCu_250nmCu_10keV000_3.hdf5', '250nmCu_250nmCu_50keV000_19.hdf5', '250nmRu_250nmAl_10keV000_1.hdf5', '250nmRu_250nmAl_50keV000_19.hdf5', '250nmW_250nmAl_10keV000_1.hdf5', '250nmW_250nmAl_50keV000_19.hdf5']
```
`md` (mreddata) has two lists of `Histogram()` objects: `md.histgorams` and `md.customHistograms`. `md.histograms` is automatically populated by Histogram objects constructed from all of the files loaded when the script is called (recall that the default behavior is to load all of the files in the current directory with the .hdf5 file extension). 
The default representation for the md object is a filesystem tree that includes the file name of every .hdf5 file loaded as well as the names of all their histograms.  

    from mreddata import mreddata as md
    md
{: .language-python}
```
---------------------------------------
250nmAl_250nmCo_10keV000_1.hdf5
  |
 +-- 0.100um_z
 +-- 0.500um_z
 +-- 1.000um_z
 +-- 100.000um_z
 +-- 195.000um_z
 +-- 200.000um_z
---------------------------------------
250nmAl_250nmAl_50keV000_19.hdf5
  |
 +-- 0.100um_z
 +-- 0.500um_z
 +-- 1.000um_z
 +-- 100.000um_z
 .
 .
 .
 ```
(This can also be viewed by running `md.displayHistograms()`)
The actual list of histogram objects is at `md.histograms`. If `options.fullpath` is set to `True`, the histogram objects list will display both the histogram name and the full directory path to the file which contains it. It is important to remember that the list is not a list of strings that contain the file/histogram names, but Histogram objects. The names are accessible as keys in `md.histogramsDict` if necessary. The histograms object list can be filtered using `md.selectHistograms(*args, exact=True/False)` and `md.dropHistograms(*args, exact=True/False)`. These methods update the histogram object list by selecting or discarding objects based on matching strings: 
    options.fullpath = True
    md.histograms
`[250nmAl_250nmAl_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmAl_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmAl_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmAl_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmAl_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmAl_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmAl_50keV000_19.hdf5 - 0.100um_z, 250nmAl_250nmAl_50keV000_19.hdf5 - 0.500um_z, 250nmAl_250nmAl_50keV000_19.hdf5 - 1.000um_z, 250nmAl_250nmAl_50keV000_19.hdf5 - 100.000um_z, 250nmAl_250nmAl_50keV000_19.hdf5 - 195.000um_z, 250nmAl_250nmAl_50keV000_19.hdf5 - 200.000um_z, 250nmAl_250nmAu_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmAu_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmAu_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmAu_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmAu_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmAu_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmAu_50keV000_19.hdf5 - 0.100um_z, 250nmAl_250nmAu_50keV000_19.hdf5 - 0.500um_z, 250nmAl_250nmAu_50keV000_19.hdf5 - 1.000um_z, 250nmAl_250nmAu_50keV000_19.hdf5 - 100.000um_z, 250nmAl_250nmAu_50keV000_19.hdf5 - 195.000um_z, 250nmAl_250nmAu_50keV000_19.hdf5 - 200.000um_z, 250nmAl_250nmCo_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmCo_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmCo_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmCo_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmCo_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmCo_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmCo_50keV000_4.hdf5 - 0.100um_z, 250nmAl_250nmCo_50keV000_4.hdf5 - 0.500um_z, 250nmAl_250nmCo_50keV000_4.hdf5 - 1.000um_z, 250nmAl_250nmCo_50keV000_4.hdf5 - 100.000um_z, 250nmAl_250nmCo_50keV000_4.hdf5 - 195.000um_z, 250nmAl_250nmCo_50keV000_4.hdf5 - 200.000um_z, 250nmAl_250nmCu_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmCu_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmCu_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmCu_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmCu_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmCu_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmCu_50keV000_19.hdf5 - 0.100um_z, 250nmAl_250nmCu_50keV000_19.hdf5 - 0.500um_z, 250nmAl_250nmCu_50keV000_19.hdf5 - 1.000um_z, 250nmAl_250nmCu_50keV000_19.hdf5 - 100.000um_z, 250nmAl_250nmCu_50keV000_19.hdf5 - 195.000um_z, 250nmAl_250nmCu_50keV000_19.hdf5 - 200.000um_z, 250nmAl_250nmRu_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmRu_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmRu_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmRu_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmRu_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmRu_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmRu_50keV000_19.hdf5 - 0.100um_z, 250nmAl_250nmRu_50keV000_19.hdf5 - 0.500um_z, 250nmAl_250nmRu_50keV000_19.hdf5 - 1.000um_z, 250nmAl_250nmRu_50keV000_19.hdf5 - 100.000um_z, 250nmAl_250nmRu_50keV000_19.hdf5 - 195.000um_z, 250nmAl_250nmRu_50keV000_19.hdf5 - 200.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 0.100um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 0.500um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 1.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 100.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 195.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 200.000um_z, 250nmAu_250nmAl_10keV000_1.hdf5 - 0.100um_z, 250nmAu_250nmAl_10keV000_1.hdf5 - 0.500um_z, 250nmAu_250nmAl_10keV000_1.hdf5 - 1.000um_z, 250nmAu_250nmAl_10keV000_1.hdf5 - 100.000um_z, 250nmAu_250nmAl_10keV000_1.hdf5 - 195.000um_z, 250nmAu_250nmAl_10keV000_1.hdf5 - 200.000um_z, 250nmAu_250nmAl_50keV000_19.hdf5 - 0.100um_z, 250nmAu_250nmAl_50keV000_19.hdf5 - 0.500um_z, 250nmAu_250nmAl_50keV000_19.hdf5 - 1.000um_z, 250nmAu_250nmAl_50keV000_19.hdf5 - 100.000um_z, 250nmAu_250nmAl_50keV000_19.hdf5 - 195.000um_z, 250nmAu_250nmAl_50keV000_19.hdf5 - 200.000um_z, 250nmCo_250nmAl_10keV000_1.hdf5 - 0.100um_z, 250nmCo_250nmAl_10keV000_1.hdf5 - 0.500um_z, 250nmCo_250nmAl_10keV000_1.hdf5 - 1.000um_z, 250nmCo_250nmAl_10keV000_1.hdf5 - 100.000um_z, 250nmCo_250nmAl_10keV000_1.hdf5 - 195.000um_z, 250nmCo_250nmAl_10keV000_1.hdf5 - 200.000um_z, 250nmCo_250nmAl_50keV000_19.hdf5 - 0.100um_z, 250nmCo_250nmAl_50keV000_19.hdf5 - 0.500um_z, 250nmCo_250nmAl_50keV000_19.hdf5 - 1.000um_z, 250nmCo_250nmAl_50keV000_19.hdf5 - 100.000um_z, 250nmCo_250nmAl_50keV000_19.hdf5 - 195.000um_z, 250nmCo_250nmAl_50keV000_19.hdf5 - 200.000um_z, 250nmCo_250nmCu_50keV000_19.hdf5 - 0.100um_z, 250nmCo_250nmCu_50keV000_19.hdf5 - 0.500um_z, 250nmCo_250nmCu_50keV000_19.hdf5 - 1.000um_z, 250nmCo_250nmCu_50keV000_19.hdf5 - 100.000um_z, 250nmCo_250nmCu_50keV000_19.hdf5 - 195.000um_z, 250nmCo_250nmCu_50keV000_19.hdf5 - 200.000um_z, 250nmCu_250nmAl_10keV000_1.hdf5 - 0.100um_z, 250nmCu_250nmAl_10keV000_1.hdf5 - 0.500um_z, 250nmCu_250nmAl_10keV000_1.hdf5 - 1.000um_z, 250nmCu_250nmAl_10keV000_1.hdf5 - 100.000um_z, 250nmCu_250nmAl_10keV000_1.hdf5 - 195.000um_z, 250nmCu_250nmAl_10keV000_1.hdf5 - 200.000um_z, 250nmCu_250nmAl_50keV000_19.hdf5 - 0.100um_z, 250nmCu_250nmAl_50keV000_19.hdf5 - 0.500um_z, 250nmCu_250nmAl_50keV000_19.hdf5 - 1.000um_z, 250nmCu_250nmAl_50keV000_19.hdf5 - 100.000um_z, 250nmCu_250nmAl_50keV000_19.hdf5 - 195.000um_z, 250nmCu_250nmAl_50keV000_19.hdf5 - 200.000um_z, 250nmCu_250nmCo_10keV000_2.hdf5 - 0.100um_z, 250nmCu_250nmCo_10keV000_2.hdf5 - 0.500um_z, 250nmCu_250nmCo_10keV000_2.hdf5 - 1.000um_z, 250nmCu_250nmCo_10keV000_2.hdf5 - 100.000um_z, 250nmCu_250nmCo_10keV000_2.hdf5 - 195.000um_z, 250nmCu_250nmCo_10keV000_2.hdf5 - 200.000um_z, 250nmCu_250nmCo_50keV000_19.hdf5 - 0.100um_z, 250nmCu_250nmCo_50keV000_19.hdf5 - 0.500um_z, 250nmCu_250nmCo_50keV000_19.hdf5 - 1.000um_z, 250nmCu_250nmCo_50keV000_19.hdf5 - 100.000um_z, 250nmCu_250nmCo_50keV000_19.hdf5 - 195.000um_z, 250nmCu_250nmCo_50keV000_19.hdf5 - 200.000um_z, 250nmCu_250nmCu_10keV000_3.hdf5 - 0.100um_z, 250nmCu_250nmCu_10keV000_3.hdf5 - 0.500um_z, 250nmCu_250nmCu_10keV000_3.hdf5 - 1.000um_z, 250nmCu_250nmCu_10keV000_3.hdf5 - 100.000um_z, 250nmCu_250nmCu_10keV000_3.hdf5 - 195.000um_z, 250nmCu_250nmCu_10keV000_3.hdf5 - 200.000um_z, 250nmCu_250nmCu_50keV000_19.hdf5 - 0.100um_z, 250nmCu_250nmCu_50keV000_19.hdf5 - 0.500um_z, 250nmCu_250nmCu_50keV000_19.hdf5 - 1.000um_z, 250nmCu_250nmCu_50keV000_19.hdf5 - 100.000um_z, 250nmCu_250nmCu_50keV000_19.hdf5 - 195.000um_z, 250nmCu_250nmCu_50keV000_19.hdf5 - 200.000um_z, 250nmRu_250nmAl_10keV000_1.hdf5 - 0.100um_z, 250nmRu_250nmAl_10keV000_1.hdf5 - 0.500um_z, 250nmRu_250nmAl_10keV000_1.hdf5 - 1.000um_z, 250nmRu_250nmAl_10keV000_1.hdf5 - 100.000um_z, 250nmRu_250nmAl_10keV000_1.hdf5 - 195.000um_z, 250nmRu_250nmAl_10keV000_1.hdf5 - 200.000um_z, 250nmRu_250nmAl_50keV000_19.hdf5 - 0.100um_z, 250nmRu_250nmAl_50keV000_19.hdf5 - 0.500um_z, 250nmRu_250nmAl_50keV000_19.hdf5 - 1.000um_z, 250nmRu_250nmAl_50keV000_19.hdf5 - 100.000um_z, 250nmRu_250nmAl_50keV000_19.hdf5 - 195.000um_z, 250nmRu_250nmAl_50keV000_19.hdf5 - 200.000um_z, 250nmW_250nmAl_10keV000_1.hdf5 - 0.100um_z, 250nmW_250nmAl_10keV000_1.hdf5 - 0.500um_z, 250nmW_250nmAl_10keV000_1.hdf5 - 1.000um_z, 250nmW_250nmAl_10keV000_1.hdf5 - 100.000um_z, 250nmW_250nmAl_10keV000_1.hdf5 - 195.000um_z, 250nmW_250nmAl_10keV000_1.hdf5 - 200.000um_z, 250nmW_250nmAl_50keV000_19.hdf5 - 0.100um_z, 250nmW_250nmAl_50keV000_19.hdf5 - 0.500um_z, 250nmW_250nmAl_50keV000_19.hdf5 - 1.000um_z, 250nmW_250nmAl_50keV000_19.hdf5 - 100.000um_z, 250nmW_250nmAl_50keV000_19.hdf5 - 195.000um_z, 250nmW_250nmAl_50keV000_19.hdf5 - 200.000um_z]`
    md.selectHistograms("250nmAl_250nmW")
    md.histograms
{: .language-python}
`[250nmAl_250nmW_10keV000_1.hdf5 - 0.100um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 0.500um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 1.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 100.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 195.000um_z, 250nmAl_250nmW_10keV000_1.hdf5 - 200.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 0.100um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 0.500um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 1.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 100.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 195.000um_z, 250nmAl_250nmW_50keV000_19.hdf5 - 200.000um_z]`

The object list is updated, and the filetree view (`md` or `md.displayHistograms()`) shows the resulting structure. The original files/histograms can be reloaded at any time: `md.resetHistograms()` 
Histograms can be combined to form new histograms using `md.combineHistograms(newHistName)`. This method will take the optional argument of `histograms=[]`, and by default attempts to combine all of the histograms in the current working histogram object list (`md.histograms`). These custom histograms are then added to the object list `md.customHistograms`, which can then be plotted separately or in conjunction with the normal histograms.  

For files with a large amount of data, loading every histogram into memory is not always smartest option. Passing the flag `--no-load` when running the script will still populate the histogram object list, but will not pull any of the actual histogram data from the file -- only the names, which can then be explored and down selected using the above mentioned filters. Once you're ready to load the actual data from the histograms selected in `md.histograms`, use `mred.getHistograms()`.

`md.plot()` uses the default settings stored in the plot_options object; standard matplotlib commands can be passed directly to the function as well, overriding `plot_options`.

The mu and degree symbols are available for import as well, as these are often used in plotting legends, titles, etc.: `from mreddata import mu, deg`
