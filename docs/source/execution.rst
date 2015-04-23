License Server
==============

The license file can be placed in the same directory along side the input files or its location can be defined using the environment variables below

.. code-block:: bash
	
	export RLM_LICENSE=port@server

or

.. code-block:: bash

	export RLM_LICENSE=/path/to/license/file

In the case that the system running the simulation cannot access the internet a local license server will be required. See http://www.reprisesoftware.com/admin/software-licensing.php for details.


Execution
=========

CommandLine
-----------

The command line environment is initialised by

.. code-block:: bash

	source /INSTALL_LOCATION/zCFD-version/bin/activate

This sets up the environment to enable execution of the specific version. 

To deactivate returning the environment to the previous state use

.. code-block:: bash
	
	zdeactivate

The *smartlaunch.bsh* script provided with zCFD automatically detects the processor type and the presence of any accelerators like Nvidia GPUs or Intel Xeon Phi's
This information is used to calculate the number of OpenMP threads to set per MPI task and to perform NUMA binding to specific cores and memory banks.

The optimum number of execution tasks should match the total number of sockets (usually two per node) **not** the number of cores. This configuration will also work for 
systems with accelerators present.

It is also recommended to always use any computational nodes in exclusive mode to achieve the best performance.

.. code-block:: bash
	
    # PROBLEM_NAME is the name of the hdf5 file containing the mesh
    # CASE_NAME is the name of the python control 
    
    mpiexec -n $NTASKS smartlaunch.bsh $PROBLEM_NAME -c $CASE_NAME

Job Scheduler
-------------

zCFD is distributed with MyCluster that provides a simple way to interface with a range of job schedulers.

To use MyCluster initialise the commandline environment as described above then use the mycluster interface to generate, submit and monitor your job.

Example usage:

.. code-block:: bash

	mycluster --create=my-job.job --jobname=my-job --project=my-project --jobqueue=my-job-queue \
	          --ntasks=2 --taskpernode=2 --script=mycluster-zcfd.bsh

	(export PROBLEM=my-test.h5;export CASE=my-test.py; mycluster --submit=my-job.job)

Note that the 'mycluster-zcfd.bsh' script is included (along with scripts for a number of other CFD codes) in the MyCluster distribution in the 'share' directory

The number of tasks per node should match the number of compute devices per node (CPU sockets, Xeon Phi devices or Nvidia GPU devices)

A list of available job queues can be obtained using

.. code-block:: bash

	mycluster -q

For a complete description of arguments use

.. code-block:: bash

	mycluster --help