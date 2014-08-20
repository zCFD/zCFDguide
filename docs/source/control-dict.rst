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

.. seealso:: See Initial Conditions 

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

.. seealso:: See Initial Conditions

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

    'time marching' : { 
    .
    .
    },

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

    'IC_1' : {
    .
    .
    },
    'IC_2' : {
    .
    .
    },
    'IC_3' : {
    .
    .
    },


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

The eddy viscosity ratio :math:`(\mu_t/\mu)` varies depending type of flow

For external flows this ratio varies from  0.1 to 1 (wind tunnel 1 to 10)

For internal flows there is greater dependence on Reynolds number

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
                #'surface layer height' : -1.0,
                #'Monin-Obukhov length' : -1.0,
                 'TKE' : 0.928,
                 'z0'  : -0.75,
                             },
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

    'BC_1' : {
    .
    .
    },
    'BC_2' : {
    .
    .
    },


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

For no slip walls  and low Reynolds number (y+ < 1) RANS meshes use

.. code-block:: python

    'kind' : 'noslip',

For no slip wall with automatic wall functions use

.. code-block:: python

    'kind' : 'wallfunction',

Roughness specification

.. code-block:: python

    'roughness' : {
                    # Constant roughness length
                    'scalar' : 0.001,
                    # Roughnes length field specified as a VTK file
                    'field' : 'bolund_roughness.vtp',
                  },

.. note:: Roughness field
    
    The roughness length at each boundary face is set by finding the nearest point on the supplied VTK file

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
    condition this refers to. See Initial Conditions.

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
    condition this refers to. See Initial Conditions.

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

    Variable name:  Qcriterion  - alias: Qcriterion
    Variable name:  T  - alias: temperature
    Variable name:  V  - alias: velocity
    Variable name:  cell_velocity  - alias: cell_velocity
    Variable name:  centre  - alias: centre
    Variable name:  cf  - alias: cf
    Variable name:  cp  - alias: cp
    Variable name:  density  - alias: density
    Variable name:  eddy  - alias: eddy
    Variable name:  frictionforce  - alias: frictionforce
    Variable name:  frictionmoment  - alias: frictionmoment
    Variable name:  gauge_pressure  - alias: gauge_pressure
    Variable name:  m  - alias: mach
    Variable name:  mach  - alias: mach
    Variable name:  mu  - alias: viscosity
    Variable name:  nu  - alias: kinematicviscosity
    Variable name:  p  - alias: pressure
    Variable name:  parent  - alias: parent
    Variable name:  pressure  - alias: pressure
    Variable name:  pressureforce  - alias: pressureforce
    Variable name:  pressuremoment  - alias: pressuremoment
    Variable name:  pressuremomentx  - alias: pressuremomentx
    Variable name:  pressuremomenty  - alias: pressuremomenty
    Variable name:  pressuremomentz  - alias: pressuremomentz
    Variable name:  resvar_1  - alias: resvar_1
    Variable name:  resvar_2  - alias: resvar_2
    Variable name:  resvar_3  - alias: resvar_3
    Variable name:  resvar_4  - alias: resvar_4
    Variable name:  resvar_5  - alias: resvar_5
    Variable name:  resvar_6  - alias: resvar_6
    Variable name:  resvar_7  - alias: resvar_7
    Variable name:  resvar_8  - alias: resvar_8
    Variable name:  resvar_9  - alias: resvar_9
    Variable name:  resvar_10  - alias: resvar_10
    Variable name:  resvar_11  - alias: resvar_11
    Variable name:  resvar_12  - alias: resvar_12
    Variable name:  rho  - alias: density
    Variable name:  roughness  - alias: roughness
    Variable name:  t  - alias: temperature
    Variable name:  temperature  - alias: temperature
    Variable name:  ti  - alias: turbulenceintensity
    Variable name:  ut  - alias: ut
    Variable name:  v  - alias: velocity
    Variable name:  var_1  - alias: var_1
    Variable name:  var_2  - alias: var_2
    Variable name:  var_3  - alias: var_3
    Variable name:  var_4  - alias: var_4
    Variable name:  var_5  - alias: var_5
    Variable name:  var_6  - alias: var_6
    Variable name:  var_7  - alias: var_7
    Variable name:  var_8  - alias: var_8
    Variable name:  var_9  - alias: var_9
    Variable name:  var_10  - alias: var_10
    Variable name:  var_11  - alias: var_11
    Variable name:  var_12  - alias: var_12
    Variable name:  velocity  - alias: velocity
    Variable name:  viscosity  - alias: viscosity
    Variable name:  vorticity  - alias: vorticity
    Variable name:  walldist  - alias: walldist
    Variable name:  walldistancezone  - alias: walldistancezone
    Variable name:  yplus  - alias: yplus
    Variable name:  zone  - alias: zone

