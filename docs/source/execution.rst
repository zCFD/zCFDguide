Execution
=========

zCFD is run from the command line using a customised environment that automatically sets up all of the paths and file locations correctly.  To make the execution of parallel jobs easier, we also provide a free tool (MyCluster) that simplifies the interaction with a range of job schedulers.

zCFD Command Line Environment
-----------------------------

To run zCFD, the *zCFD command line environment* must be active. The zCFD command line environment is initialised from a terminal window by sourcing the *activate* script in the zCFD installation directory.  It does not matter where this directory is - all of the executable paths and file locations will be set automatically:

.. code-block:: bash

	source /INSTALL_LOCATION/zCFD-version/bin/activate

This sets up the environment to enable execution of the specific version. When this environment is active, you should see a prefix to your command prompt, such as:

.. code-block:: bash

    (zCFD) >

To deactivate the command line environment, returning the environment to the previous state use

.. code-block:: bash
	
	zdeactivate

Your command prompt should return to its normal appearance.

zCFD Smartlaunch
----------------

zCFD makes very effective use of a range of processor types and computer configurations.  We provide a *smartlaunch.bsh* script with zCFD to automatically detect the processor type and the presence of any accelerators (like Nvidia GPUs or Intel Xeon Phi's).

Smartlaunch uses this information to calculate the number of OpenMP threads to set per MPI task and to perform NUMA binding to specific cores and memory banks.

The optimum number of execution tasks ($NTASKS below) should match the total number of sockets (usually two per node) **not** the number of cores.  This configuration will also work for systems with accelerators present.

It is also recommended to always use any computational nodes in exclusive mode to achieve the best performance. TODO - how do we ensure this?

.. code-block:: bash
	
    # PROBLEM_NAME is the name of the hdf5 file containing the mesh
    # CASE_NAME is the name of the python control 
    
    mpiexec -n $NTASKS smartlaunch.bsh $PROBLEM_NAME -c $CASE_NAME

MyCluster Job Scheduler
-----------------------

zCFD is distributed with the MyCluster tool, which provides a simple way to interface with a range of job schedulers.

To use MyCluster initialise the zCFD command line environment as described above then use the MyCluster interface to generate, submit and monitor your job.

Example usage:

.. code-block:: bash

	mycluster --create=my-job.job --jobname=my-job --project=my-project --jobqueue=my-job-queue \
	          --ntasks=2 --taskpernode=2 --script=mycluster-zcfd.bsh

	(export PROBLEM=my-test.h5;export CASE=my-test.py; mycluster --submit=my-job.job)

Note that the *mycluster-zcfd.bsh* script is included (along with scripts for a number of other CFD codes) in the MyCluster distribution in the *share/* directory

The number of tasks per node should match the number of compute devices per node (CPU sockets, Xeon Phi devices or Nvidia GPU devices)

A list of available job queues can be obtained using

.. code-block:: bash

	mycluster -q

For a complete description of arguments use

.. code-block:: bash

	mycluster --help