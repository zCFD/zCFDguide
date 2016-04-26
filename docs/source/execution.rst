Execution
=========

zCFD is run from the command line using a customised environment that automatically sets up all of the paths and file locations correctly.  To make the execution of parallel jobs easier, we also provide the free tool :ref:`mycluster` that simplifies the interaction with a range of job schedulers.

.. _`zcfd-command`:

(zCFD) Command Line Environment
-------------------------------

To run zCFD, the *zCFD command line environment* must be active. The zCFD command line environment is initialised from a terminal window by sourcing the *activate* script in the zCFD installation directory.  It does not matter where this directory is - all of the executable paths and file locations will be set automatically:

.. code-block:: bash

	> source /INSTALL_LOCATION/zCFD-version/bin/activate

This sets up the environment to enable execution of the specific version. When this environment is active, you should see a prefix to your command prompt, such as:

.. code-block:: bash

    (zCFD) >

To deactivate the command line environment, returning the environment to the previous state use

.. code-block:: bash
	
    (zCFD) > zdeactivate

Your command prompt should return to its normal appearance.

Smartlaunch
-----------

zCFD makes very effective use of a range of processor types and computer configurations.  We provide a *smartlaunch.bsh* script with zCFD to automatically detect the processor type and the presence of any accelerators (like Nvidia GPUs or Intel Xeon Phi's).

Smartlaunch uses this information to calculate the number of OpenMP threads to set per MPI task and to perform NUMA binding to specific cores and memory banks.

The optimum number of execution tasks ($NTASKS below) should match the total number of sockets (usually two per node) **not** the number of cores.  This configuration will also work for systems with accelerators present.

It is also recommended to always use any computational nodes in exclusive mode to achieve the best performance. TODO - how do we ensure this?

.. code-block:: bash
	
    # PROBLEM_NAME is the name of the hdf5 file containing the mesh
    # CASE_NAME is the name of the python control 
    
    run_zcfd --ntask 10 -p $PROBLEM_NAME -c $CASE_NAME

Alternatively, :ref:`mycluster` is a powerful tool for setting up, running and monitoring zCFD jobs on a range of schedulers.  We recommend this tool, and it is provided as part of the zCFD environment.

