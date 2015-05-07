Mesh Conversion Utilities
=========================

The unstructured mesh for zCFD is encapslated using the `hdf5 <http://www.hdfgroup.org/HDF5/>`_ specification, that provides a flexible, self describing format which also supports parallel I/O that can be tuned per filesystem type (e.g NFS, GPFS, Lustre etc).

OpenFOAM-2.1.x
--------------

We provide a range of converters for popular mesh types via a special version of the open-source CFD code OpenFOAM, available freely for download `here <https://github.com/zenotech/OpenFOAM-2.1.x>`_

Once downloaded and installed, this version of OpenFOAM will convert a range of file formats to the zCFD format. Prior to running any of these, you will need to activate your OpenFOAM environment using

.. code-block:: python

    source $FOAM_INST_DIR/OpenFOAM-2.1.x/etc/bashrc

OpenFOAM Mesh Converter
-----------------------

To convert from an OpenFOAM format mesh to zCFD, run the following command in the MESH directory.

.. code-block:: python

    foamTozCFD

This will create a new folder called *zInterface/* containing the hdf5 file.

Fluent Mesh Converter
---------------------

The converter for Fluent files is a utility called *fluent_convert*.  This is included in the path set by activating the zCFD command line environment.

To run the utility, use

.. code-block:: python

    fluent_convert.py <filename>.msh <new_filename>.h5

or

.. code-block:: python

    fluent_convert.py <filename>.cas <new_filename>.h5

The will produce 2 files: the zCFD hdf5 file and the *name_zone.py* python file which holds the fluid and boundary zone names.

Star CCM+ Mesh Converter
------------------------

There are a number of different file formats for Star CCM+ meshes.  The *<filename>.sim* file is in a propietary format and at this stage cannot be converted.  The *<filename>.ccm* can be converted using

.. code-block:: python

    ccmconvert <filename>.ccm <new_filenam>.h5

This produces the zCFD hdf5 file.

CGNS Mesh Converter
-------------------

This utility will convert an unstructured, non-chimera, hexahedral mesh in CGNS (ref) format.  The converter will handle meshes with element types (HEXA_8):

.. code-block:: python

    cgnsconvert <filename>.cgns <new_filenam>.h5

This produces the zCFD hdf5 file. If you require support for additional element types please contact us: mailto:zcfd@zenotech.com.

UGRID Mesh Converter
--------------------

UGRID is a NASA format for meshes (ref). You must follow the naming convention (ref) for UGRID files, which describes the format and *endian-ness* of the file data ("ascii8", "b8", or "r8") depending upon which language (C or FORTRAN) was used to create the file.

.. code-block:: python

    ugridconvert <filename>.[ascii8, b8, r8].ugrid <new_filename>.h5

This produces the zCFD hdf5 file.

DLR Tau Mesh Converter
----------------------

Tau is a CFD solver produced by the DLR (ref).  Note that Tau is a node-based solver so flow field variables are calculated for and stored at the mesh nodes (or vertices) rather than at cell centres (the approach used by Star CCM+, OpenFOAM, CFX and zCFD). Thus simply converting the file format will not guarantee a good result.

.. code-block:: python

    tauconvert <filename>.XXX <new_filename>.h5

This produces the zCFD hdf5 file.


(note - look at sphinxtr git repo)