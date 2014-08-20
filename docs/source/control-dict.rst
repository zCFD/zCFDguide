Control Dictionary
==================

The zCFD control file is a python file that is executed a runtime. This provides a flexible way of specifying key parameters enabling user defined functions and the leverage of rich python libraries.

A python dictionary called parameters provides the main interface to controlling zCFD

.. code-block:: python

    parameters = {....}

Reference Settings
------------------

Define units  

.. code-block:: python
    
    # units for dimensional quantities
    'units' : 'SI',

.. topic:: Options
    
    'SI' - SI units

Reference state

.. code-block:: python
   
    # reference state
    'reference' : 'IC_1',

.. seealso:: See Initial Conditions 

Intialisation

Intial conditions. Defaults to reference state if not specified

.. code-block:: python
   
    # Initial conditions
    'initial' : 'IC_2',

.. seealso:: See Initial Conditions

To restart from a previous solution

.. code-block:: python

    # Restart from previous solution
    'restart' : 'true',



Low Mach number preconditioner settings (Optional)

.. code-block:: python
    
    # Preconditoner factor
    'preconditioner' : {
                        'factor' : 0.5,
                       },




