Installation
============

Simply untar the downloaded file and extract it anywhere on your filesystem. Inside the bin directory you will find a run_zCFD script that is the most straightforward way of running the code:

.. code-block:: bash
	
        /INSTALL_LOCATION/bin/run_zCFD --ntask $NTASK -p $PROBLEM_NAME -c $CASE_NAME 	

Alternatively, :ref:`mycluster` is a powerful tool for setting up, running and monitoring zCFD jobs on a range of schedulers. We recommend this tool, and it is provided as part of the zCFD environment.
