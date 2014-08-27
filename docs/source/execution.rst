Execution
=========

The *smartlaunch.bsh* script provided with zCFD automatically detects the processor type and the presence of any accelerators like Nvidia GPUs or Intel Xeon Phi's
This information is used to calculate the number of OpenMP threads to set per MPI task and to perform NUMA binding to specific cores and memory banks.

The optimum number of execution tasks should match the total number of sockets (usually two per node) **not** the number of cores. This configuration will also work for 
systems with accelerators present.

It is also recommended to always use any computational nodes in exclusive mode to achieve the best performance.

.. code-block:: bash
	
    # PROBLEM_NAME is the name of the hdf5 file containing the mesh
    # CASE_NAME is the name of the python control 
    
    mpiexec -n $NTASKS smartlaunch.bsh $PROBLEM_NAME -c $CASE_NAME

