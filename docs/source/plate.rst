Overview
^^^^^^^^

This case exercises the turbulence model through comparing the turbulent boundary layer profile 
directly with theory given the absence of a pressure gradient. This test is applied to a series 
of meshes with increasing :math:`y^{+}`` in order to stress the automatic wall function implementation

Conditions
^^^^^^^^^^

.. figure:: images/plateBCpic.jpg
	:width: 75%
	:align: center
	:alt: alternate text
	:figclass: align-center

	Computational domain for the turbulent flat plate flow case.

Mesh
^^^^

Results
^^^^^^^

.. figure:: images/flat_plate_bl_profile.png
	:width: 90%
	:align: center
	:alt: alternate text
	:figclass: align-center

	Boundary layer profiles at :math:`x=0.97`

Mesh sensitivity

.. figure:: images/flat_plate_mesh_conv_profile.png
	:width: 75%
	:align: center
	:alt: alternate text
	:figclass: align-center

	Mesh convergence comparison for :math:`C_f`


References
^^^^^^^^^^

`<http://turbmodels.larc.nasa.gov/flatplate.html>`_

`<http://turbmodels.larc.nasa.gov/flatplate_val.html>`_
