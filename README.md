# mreddata

Version 0.3.30:

To install: `pip install mreddata`. 

Run with `--no-load` to only load the hdf5 file info, without collected the tables. Useful for large files/lots of histograms.  
`--files <select files here> ` to manually select which hdf5 files; default is all files ending with .hdf5 in the current directory. 

Note: The current version requires the following Hdf5 file attributes to be set: "gfu" and "nIons"

```python
>>> from mreddata import options, plot_options, Hdf5Data
>>> options.files
['250nmAl_250nmAl_10keV000_1.hdf5', '250nmAl_250nmAl_50keV000_19.hdf5', '250nmAl_250nmAu_10keV000_1.hdf5', '250nmAl_250nmAu_50keV000_19.hdf5', '250nmAl_250nmCo_10keV000_1.hdf5', '250nmAl_250nmCo_50keV000_4.hdf5', '250nmAl_250nmCu_10keV000_1.hdf5', '250nmAl_250nmCu_50keV000_19.hdf5', '250nmAl_250nmRu_10keV000_1.hdf5', '250nmAl_250nmRu_50keV000_19.hdf5', '250nmAl_250nmW_10keV000_1.hdf5', '250nmAl_250nmW_50keV000_19.hdf5', '250nmAu_250nmAl_10keV000_1.hdf5', '250nmAu_250nmAl_50keV000_19.hdf5', '250nmCo_250nmAl_10keV000_1.hdf5', '250nmCo_250nmAl_50keV000_19.hdf5', '250nmCo_250nmCu_50keV000_19.hdf5', '250nmCu_250nmAl_10keV000_1.hdf5', '250nmCu_250nmAl_50keV000_19.hdf5', '250nmCu_250nmCo_10keV000_2.hdf5', '250nmCu_250nmCo_50keV000_19.hdf5', '250nmCu_250nmCu_10keV000_3.hdf5', '250nmCu_250nmCu_50keV000_19.hdf5', '250nmRu_250nmAl_10keV000_1.hdf5', '250nmRu_250nmAl_50keV000_19.hdf5', '250nmW_250nmAl_10keV000_1.hdf5', '250nmW_250nmAl_50keV000_19.hdf5']
```

`data = Hdf5Data()` creates the histogram object list. (The Histogram class is accessible by `from mreddata.datatools import Histogram`)

Calling the Hdf5Data object displays the filesystem tree with histograms currenlty selected in `data.histograms` organized according to their parent file. This is useful for selecting/fitlering.

```python
from mreddata import options, plot_options, Hdf5Data
data = Hdf5Data()
>>> data
---------------------------------------
tungsten001_9.hdf5
  |
  ├── n7_sd
  ├── n6_sd
  ├── n3_sd
  ├── n10_sd
  ├── n9_sd
  ├── n8_sd
  ├── n5_sd
  ├── n4_sd
  ├── n2_sd
  ├── n1_sd
---------------------------------------
tungsten000_9.hdf5
  |
  ├── n7_sd
  ├── n6_sd
  ├── n3_sd
  ├── n10_sd
  ├── n9_sd
  ├── n8_sd
  . . . 
```

`data.selectHistograms()` accepts string arguments that filter based on the full path of the histogram (i.e. the filname AND the name of the histogram)

`data.dropHistograms()` is similar, but drops histograms from the `data.histograms` list that match the strings passed. 

`data.resetHistograms()` restores the histogram list to its original state, removing all filters. 

`data.combineHistograms(newHistName, histograms=[])` automatically combines the histograms in `data.histograms`, appending the new histogram object to `data.customHistograms` with the name passed. Alternatively, you can manually pass a list of histogram objects to the function.  

`data.attributes()` will display the file attributes for all files with histograms currently loaded in `data.histograms`. You can also pass strings to return only specific attributes (e.g.: `data.attributes("gfu", "nIons")`)

`data.plot()` will (by default) plot the 'reverse integrated' data for all histograms in the current histogram object list (`data.histograms`). Standard pandas/matplotlib plot options can be passed as keyword arguments directly, or by using `plot_options` (`from mreddata import plot_options`). 

The histogram objects themselves have attributes which handle their plotting behavior. For example:

```python
histogram = data.histograms[0]
histogram.label = 'Label text'  # defaults to the histogram.fullpath
histogram.color = 'red'
histogram.dashes = [1,4] 		#(None, None) is default
```

The pandas DataFrame object is accesible via `histogram.df`

Columns are calculated by default when loading files (unless `--no-load` selected) to include the following columns: `['x', 'y_raw', 'n', 'w', 'y_norm', 'y_int', 'y_diff']`. 
`y_norm` is normalized using the gfu and nIons file attributes, and `y_int` and `y_diff` correspond to the integrated and differential values, respectively. Any of these columns can be substituted in place of the default `y_int` when plotting using the standard pandas format for data selection: `data.plot(y='y_diff')`
