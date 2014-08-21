Control Dictionary
==================

The zCFD control file is a python file that is executed a runtime. This provides a flexible way of specifying key parameters enabling user defined functions and the leverage of rich python libraries.

A python dictionary called parameters provides the main interface to controlling zCFD

.. code-block:: python

    parameters = {....}

Reference Settings
------------------

Define units  
^^^^^^^^^^^^

.. code-block:: python

    # units for dimensional quantities
    'units' : 'SI',

.. topic:: Options

    'SI' - SI units

Reference state
^^^^^^^^^^^^^^^

.. code-block:: python

    # reference state
    'reference' : 'IC_1',

.. seealso:: See `Initial Conditions`_ 

Partitioner
^^^^^^^^^^^

.. code-block:: python

    'partitioner' : 'metis',


Initialisation
--------------

Intial conditions. Defaults to reference state if not specified

.. code-block:: python

    # Initial conditions
    'initial' : 'IC_2',

.. seealso:: See `Initial Conditions`_

To restart from a previous solution

.. code-block:: python

    # Restart from previous solution
    'restart' : 'true',

Low Mach number preconditioner settings (Optional)

.. code-block:: python

    # Advanced: Preconditoner factor
    'preconditioner' : {
                        'factor' : 0.5,
                       },

Time Marcher
------------

.. code-block:: python

    'time marching' : {....},

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
^^^^^^^^^^^^^
.. code-block:: python

    'scheme' : {
                 # 
                 'name' : 'runge kutta',
                 # Number of RK stages 
                 'stage': 5,
               },

Multigrid
^^^^^^^^^
.. code-block:: python

    # Maximum number of meshes (including fine mesh)
    'multigrid' : 10, 
    # Number of multigrid cycles before solving on fine mesh only
    'multigrid cycles' : 5000,
    # Advanced: Prologation factor
    'prolong factor' : 0.75,
    # Advanced: Prolongation factor for transported quantities
    'prolong transport factor' : 0.3,

CFL
^^^
.. code-block:: python

    # Default CFL number for all equations 
    'cfl': 2.5,
    # Optional: CFL number for transported quantities 
    'cfl transport' : 1.5,
    # Optional: CFL number for coarse meshes
    'cfl coarse' : 2.0,

Cycles
^^^^^^
.. code-block:: python

    # Number of pseudo time cyles 
    'cycles' : 5000,



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

Initial Conditions
------------------

The intial condition properties are defined using consecutively numbered blocks like

.. code-block:: python

    'IC_1' : {....},
    'IC_2' : {....},
    'IC_3' : {....},

Each block can contain the following options 

.. code-block:: python

    # Static temperature in Kelvin
    'temperature': 293.0,
    # Static pressure in Pascals
    'pressure':101325.0,

.. code-block:: python

    # Fluid velocity
    'V': {
            # Velocity vector
            'vector' : [1.0,0.0,0.0],
            # Optional: specifies velocity magnitude  
            'Mach' : 0.20,
          },

Define dynamic viscosity at the static temperature previously specified.
This can be specified either as a dimensional quantity or by a Reynolds number and reference length

.. code-block:: python
  
    # Dynamic viscosity in dimensional units 
    'viscosity' : 1.83e-5,

or

.. code-block:: python

    # Reynolds number
    'Reynolds No' : 5.0e6,
    # Reference length 
    'Reference Length' : 1.0, 

Turbulence intensity is defined as the ratio of velocity fluctuations :math:`u^{'}` to the mean flow velocity. A turbulence intensity of
1% is considered low and greater than 10% is considered high.   

.. code-block:: python

    # Turbulence intensity %
    'turbulence intensity': 0.01,

The eddy viscosity ratio :math:`(\mu_t/\mu)` varies depending type of flow.
For external flows this ratio varies from  0.1 to 1 (wind tunnel 1 to 10)

For internal flows there is greater dependence on Reynolds number. Typical values are:

+------+-------+------+--------+--------+--------+-----------+
| Re   |  3000 | 5000 | 10,000 | 15,000 | 20,000 | > 100,000 |
+------+-------+------+--------+--------+--------+-----------+
| eddy |  11.6 | 16.5 | 26.7   | 34.0   | 50.1   |  100      |
+------+-------+------+--------+--------+--------+-----------+

.. code-block:: python

    # Eddy viscosity ratio
    'eddy viscosity ratio': 0.1,

.. code-block:: python

    'profile' : {
                 'ABL' : {
                           'roughness length' : 0.0003,
                           'friction velocity' : 0.4,
                           'surface layer height' : -1.0,
                           'Monin-Obukhov length' : -1.0,
                           'TKE' : 0.928,
                           'z0'  : -0.75,
                          },
                },

Certain conditions are specified relatively

.. code-block:: python

    'reference' : 'IC_1',
    # total pressure/reference static pressure
    'total pressure ratio' : 1.0,
    # total temperature/reference static temperature
    'total temperature ratio' : 1.0,

.. code-block:: python

    'reference' : 'IC_1',
    # static pressure/reference static pressure
    'static pressure ratio' : 1.0,


Boundary Conditions
-------------------

Boundary condition properties are defined using consecutively numbered blocks like

.. code-block:: python

    'BC_1' : {....},
    'BC_2' : {....},

Wall
^^^^

.. code-block:: python

    # Zone type tag
    'ref' : 3,
    # Optional: Specific zone boundary condition overide
    'zone' : [0,1,2,3],
    # Boundary condition type
    'type' : 'wall',

There are three kind of wall boundaries that can be specified.

For slip walls use

.. code-block:: python

    'kind' : 'slip',

For no slip walls  and low Reynolds number :math:`(y^{+} \leq 1)` RANS meshes use

.. code-block:: python

    'kind' : 'noslip',

For no slip wall with automatic wall functions for meshes with variable :math:`y^{+}` use

.. code-block:: python

    'kind' : 'wallfunction',

Roughness specification

.. code-block:: python

    'roughness' : {
                    # Type of roughness specification (option: height or length)
                    'type' : 'height',
                    # Constant roughness
                    'scalar' : 0.001,
                    # Roughnes field specified as a VTK file
                    'field' : 'roughness.vtp',
                  },

.. note::
    
    The roughness at each boundary face is set by finding the nearest point to the face centre on the supplied VTK file with the roughness 
    value looked up in a node based scalar array called 'Roughness'

Wall velocity

.. code-block:: python

    'V' : {
    .
    .
    },

Options

.. code-block:: python
    
    'linear' : {
                # Velocity vector
                'vector' : [1.0,0.0,0.0],
                # Optional: specifies velocity magnitude  
                'Mach' : 0.20,
    },

or

.. code-block:: python

    'rotating' : {
                    # Rotational velocity in rad/s
                    'omega' : 2.0,
                    # Rotation axis
                    'axis' : [1.0,0.0,0.0],
                    # Rotation origin
                    'origin' : [0.0,0.0,0.0],
    },

Farfield
^^^^^^^^

.. code-block:: python

    # Zone type tag
    'ref' : 9,
    # Optional: Specific zone boundary condition overide
    'zone' : [0,1,2,3],
    # Boundary condition type
    'type' : 'farfield',
    # Kind of farfield
    'kind' : 'riemann',
    # Farfield conditions
    'condition' : 'IC_1',

Inflow
^^^^^^

.. code-block:: python

    # Zone type tag
    'ref' : 4,
    # Optional: Specific zone boundary condition overide
    'zone' : [0,1,2,3],
    # Boundary condition type
    'type' : 'inflow',
    # Kind of inflow
    'kind' : 'default',
    # Inflow conditions
    'condition' : 'IC_2',

.. note:: 

    This boundary condition is specified by a total pressure and temperature ratios that needs to be defined by the 
    condition this refers to. See `Initial Conditions`_.

Outflow
^^^^^^^

.. code-block:: python

    # Zone type tag
    'ref' : 5,
    # Optional: Specific zone boundary condition overide
    'zone' : [0,1,2,3],
    # Boundary condition type
    'type' : 'outflow',
    # Kind of outflow
    'kind' : 'default',
    # Outflow conditions
    'condition' : 'IC_3',

.. note:: 

    This boundary condition is specified by a static pressure ratio that needs to be defined by the 
    condition this refers to. See `Initial Conditions`_.

Symmetry
^^^^^^^^

.. code-block:: python

    # Zone type tag
    'ref' : 7,
    # Optional: Specific zone boundary condition overide
    'zone' : [0,1,2,3],
    # Boundary condition type
    'type' : 'symmetry',

Reporting
---------

.. code-block:: python

    'report' : {
                  # Report frequency
                  'frequency' : 1,
                  'monitor' : {
                                 'MR_1' : {
                                            'name' : 'mast_1',
                                            'point' : [49673.0, 58826.0, 1120.0],
                                            'variables' : ['V','ti'],
                                          },
                              },

                  'forces' : {
                        'FR_1' : {
                                     'name' : 'wall',
                                     'zone' : [11,12,13,14,15,20,21,22,23,24,25,26,27,28],
                                     'transform' : my_transform,
                                     'reference area' : 0.112032,
                                 },
                      },
                },

Output
------

.. code-block:: python

    'write output' : {
                      # Output format
                      'format' : 'vtk',
                      # Variables to output on each boundary type
                      'surface variables': ['V','p'],
                      # Field variables to be output
                      'volume variables' : ['V','p'],
                      # Output frequency
                      'frequency' : 100,
                    },   

.. topic:: Output Variables

  +---------------------+---------------------+-----------------------------+
  | Variable Name       | Alias               | Definition                  |
  +=====================+=====================+=============================+
  | temperature         | T, t                |                             |
  +---------------------+---------------------+-----------------------------+
  | pressure            | p                   |                             |
  +---------------------+---------------------+-----------------------------+
  | density             | rho                 |                             |
  +---------------------+---------------------+-----------------------------+
  | velocity            | V, v                |                             |
  +---------------------+---------------------+-----------------------------+
  | cp                  |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | mach                | m                   |                             |
  +---------------------+---------------------+-----------------------------+
  | viscosity           | mu                  |                             |
  +---------------------+---------------------+-----------------------------+
  | kinematicviscosity  | nu                  |                             |
  +---------------------+---------------------+-----------------------------+ 
  | gauge_pressure      |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | vorticity           |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | Qcriterion          |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | turbulenceintensity | ti                  |                             |
  +---------------------+---------------------+-----------------------------+
  | eddy                |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | cell_velocity       |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | centre              |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | walldistance        |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | walldistancezone    |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | parent              |                     |                             |
  +---------------------+---------------------+-----------------------------+

  Surface only quantities

  +---------------------+---------------------+-----------------------------+  
  | pressureforce       |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | pressuremoment      |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | pressuremomentx     |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | pressuremomenty     |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | pressuremomentz     |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | frictionforce       |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | frictionmoment      |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | roughness           |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | ut                  |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | yplus               |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | zone                |                     |                             |
  +---------------------+---------------------+-----------------------------+
  | cf                  |                     |                             |
  +---------------------+---------------------+-----------------------------+

    

