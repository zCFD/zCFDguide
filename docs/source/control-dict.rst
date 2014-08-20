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

Initialisation
--------------

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

Time Marcher
------------

.. code-block:: python

    'time marching' : { 

Time accurate simulation control

.. code-block:: python

    'unsteady' : {
                   # Total time in seconds
                   'total time' : 1.0,
                   # Time step in seconds
                   'time step' : 1.0,
                   # Time accuracy (options: first or second)
                   'order' : 'second',
                   # Number of pseudo time cycles to run before starting time accurate simulation
                   'start' : 3000, 
                 },

Solver scheme

.. code-block:: python

    'scheme' : {
                 # 
                 'name' : 'runge kutta',
                 # Number of RK stages 
                 'stage': 5,
               },

Multigrid

.. code-block:: python

    # Maximum number of meshes (including fine mesh)
    'multigrid' : 10, 
    # Number of multigrid cycles before solving on fine mesh only
    'multigrid cycles' : 5000,
    # Prologation factor
    'prolong factor' : 0.75,
    # Prolongation factor for transported quantities
    'prolong transport factor' : 0.3,

CFL

.. code-block:: python

    # Default CFL number for all equations 
    'cfl': 2.5,
    # CFL number for transported quantities 
    'cfl transport' : 1.5,
    # CFL number for coarse meshes
    'cfl coarse' : 2.0,

Cycles

.. code-block:: python

    # Number of pseudo time cyles 
    'cycles' : 5000,


.. code-block:: python

    },

Equations
---------

.. code-block:: python

    'equations' : 'RANS',

Options

.. code-block:: python

  'euler' : {
              # Spatial accuracy (options: first, second)
              'order' : 'second',
              # MUSCL limiter (options: vanalbada)
              'limiter' : 'vanalbada',
              # Use low speed mach preconditioner
              'precondition' : 'true',                                          
            },

.. code-block:: python

    'viscous' : {
                  # Spatial accuracy (options: first, second)
                  'order' : 'second',
                  # MUSCL limiter (options: vanalbada)
                  'limiter' : 'vanalbada',
                  # Use low speed mach preconditioner                                            
                  'precondition' : 'true',                                          
                },


.. code-block:: python

    'RANS' : {
                # Spatial accuracy (options: first, second, euler_second)
                'order' : 'second',
                # MUSCL limiter (options: vanalbada)
                'limiter' : 'vanalbada',
                # Use low speed mach preconditioner 
                'precondition' : 'true', 
                # Turbulence                                        
                'turbulence' : {
                                  # turbulence model (options: 'sst') 
                                  'model' : 'sst',
                                  # betastar turbulence closure constant
                                  'betastar' : 0.09,
                                },
               },

Material Specification
----------------------

.. code-block:: python

    'material' : 'air',

Options

.. code-block:: python

    'air' : {
              'gamma' : 1.4,
              'gas constant' : 287.0,
              'Sutherlands const': 110.4,
              'Prandtl No' : 0.4,
              'Turbulent Prandtl No' : 0.9,
            },




