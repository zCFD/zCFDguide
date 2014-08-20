Overview
=================

What is zCFD
------------
zCFD makes use of the MPI protocol to manage execution over a set of compute nodes. The execution process is completely parallel from the start to finish including all the Input/Output (IO) functionality.
To support this scalable implementation the mesh input is provided in hdf5 format which is portable and self describing. Filters are provided to convert the most frequently used formats to this internal format.
zCFD uses a single control dictionary to specify all the parameters. This is python file containing a python dictionary with certain mandatory keys. The use of python for the control dictionary enables the user to add custom functions that populate the dictionary at runtime.

