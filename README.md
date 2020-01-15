# mreddata

Version 0.1.0:

Includes the Hdf5Data() object, which is used to parse histograms from the HDF5 file output in MRED to an easy-to-use pandas dataframe. Automatically excpects the HDF5 file to have file attributes _gfu_ and _nIons_ which are used to normalize the data. 

Hdf5Files() (with an instance of the object, _files_, already loaded at initialization) allows for convenient handling of HDF5 files across multiple directories
