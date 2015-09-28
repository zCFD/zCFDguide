Control Dictionary
==================

The zCFD control file is a python file that is executed at runtime. This provides a flexible way of specifying key parameters enabling user defined functions and leverage of rich python libraries.

A python dictionary called 'parameters' provides the main interface to controlling zCFD

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

The 'metis' partitioner is used to split the computational mesh up so that the job can be run in parallel.  Other partitioners will be added in future code releases

.. code-block:: python

    'partitioner' : 'metis',

Safe Mode
^^^^^^^^^

Safe mode turns on extra checks and provides diagnosis in the case of solver stability issues

.. code-block:: python

    # Safe mode
    'safe' : 'false',

Initialisation
--------------

Initial conditions are used to set the flow variable values in all cells at the start of a simulation.  The initial conditions default to the reference state (above) if not specified

.. code-block:: python

    # Initial conditions
    'initial' : 'IC_2',

.. seealso:: See `Initial Conditions`_

To restart from a previous solution

.. code-block:: python

    # Restart from previous solution
    'restart' : 'true',

Low Mach number preconditioner settings (Optional)

The preconditioner is a mathematical technique for improving the speed of the solver when the fluid flow is slow compared to the speed of sound.  Use of the preconditioner does not alter the final converged solution produced by the solver.  The preconditioner factor is used to improve the robustness of the solver in regions of very low speed.

.. code-block:: python

    # Advanced: Preconditoner factor
    'preconditioner' : {
                        'factor' : 0.5,
                       },

Time Marcher
------------

.. code-block:: python

    'time marching' : {....},

Time accurate (unsteady) simulation control

.. code-block:: python

    'unsteady' : {
                   # Total time in seconds
                   'total time' : 1.0,
                   # Time step in seconds
                   'time step' : 1.0,
                   # Time accuracy (options: 'first' or 'second' order)
                   'order' : 'second',
                   # Number of pseudo time (steady) cycles to run before starting time accurate simulation
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

The mesh is automatically coarsened by merging cells on successive layers in order to accelerate solver convergence.  This does not alter the accuracy of the solution on the finest (original) mesh.  Advanced users can control the number of geometric multigrid levels and the 'prolongation' of quantities from coarse to fine meshes.

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

The Courant-Friedrichs-Lewy (CFL) number controls the local pseudo time-step that the solver uses to reach a converged solution. The larger the CFL number, the faster the solver will run but the less stable it will be.  The default values should be appropriate for most cases, but for lower-quality meshes or very complex geometries it may be necessary to use lower values (e.g. CFL of 1.0).  In such cases it may also be helpful to turn off Multigrid (above) by setting the maximum number of meshes to 0.

.. code-block:: python

    # Default CFL number for all equations 
    'cfl': 2.5
    # Optional: Start small and increase the CFL each cycle by a growth factor up to the 'cfl' value
    'ramp': { 'initial': 1.0, 'growth': 1.1 },
    # Optional: Override CFL number for transported quantities 
    'cfl transport' : 1.5,
    # Optional: Override CFL number for coarse meshes
    'cfl coarse' : 2.0,

Cycles
^^^^^^

For steady-state simulations, the number of pseudo time cycles is the same as the number of steps that the solver should use to reach a converged solution.  Note that the solver uses local pseudo time-stepping (the time-step varies according to local conditions) so any intermediate solution is not necessarily time-accurate.

For unsteady (time-accurate) simulations, zCFD uses 'dual time-stepping' to advance the solution in time.  For unsteady simulations, the number of pseudo time cycles determines the number of inner iterations that the solver uses to converge each real time step.

.. code-block:: python

    # Number of pseudo time cyles 
    'cycles' : 5000,

Equations
---------

.. code-block:: python

    # Governing equations to be used (options: RANS, euler, viscous)
    'equations' : 'RANS',

Compressible Euler flow is inviscid (no viscosity and hence no turbulence).  The compressible Euler equations are appropriate when modelling flows where momentum significantly dominates viscosity - for example at very high speed. The computational mesh used for Euler flow does not need to resolve the flow detail in the boundary layer and hence will generally have far fewer cells than the corresponding viscous mesh would have.

.. code-block:: python

  'euler' : {
              # Spatial accuracy (options: first, second)
              'order' : 'second',
              # MUSCL limiter (options: vanalbada)
              'limiter' : 'vanalbada',
              # Use low speed mach preconditioner
              'precondition' : 'true',                                          
            },

The viscous (laminar) equations model flow that is viscous but not turbulent.  The Reynolds number (http://en.wikipedia.org/wiki/Reynolds_number) of a flow regime determines whether or not the flow will be turbulent. The computational mesh for a viscous flow does have to resolve the boundary layer, but the solver will run faster as fewer equations are being included.

.. code-block:: python

    'viscous' : {
                  # Spatial accuracy (options: first, second)
                  'order' : 'second',
                  # MUSCL limiter (options: vanalbada)
                  'limiter' : 'vanalbada',
                  # Use low speed mach preconditioner                                            
                  'precondition' : 'true',                                          
                },

The fully turbulent (Reynolds Averaged Navier-Stokes Equations)

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

.. code-block:: python

    'DGeuler' : {
                   # Spatial polynomial order 0,1,2,3
                   'order' : 2,
                   # Use low speed mach preconditioner
                   'precondition' : 'true',
                },

.. code-block:: python

    'DGviscous' : {
                   # Spatial polynomial order 0,1,2,3
                   'order' : 2,
                   # Use low speed mach preconditioner
                   'precondition' : 'true',
                  },

Material Specification
----------------------

The user can specify any fluid material properties by following the (default) scheme for 'air':

.. code-block:: python

    'material' : 'air',

Options

.. code-block:: python

    'air' : {
              'gamma' : 1.4,
              'gas constant' : 287.0,
              'Sutherlands const': 110.4,
              'Prandtl No' : 0.72,
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

Dynamic (shear, absolute or molecular) viscosity should be defined at the static temperature previously specified.  This can be specified either as a dimensional quantity or by a Reynolds number and reference length

.. code-block:: python
  
    # Dynamic viscosity in dimensional units 
    'viscosity' : 1.83e-5,

or

.. code-block:: python

    # Reynolds number
    'Reynolds No' : 5.0e6,
    # Reference length 
    'Reference Length' : 1.0, 

Turbulence intensity is defined as the ratio of velocity fluctuations :math:`u^{'}` to the mean flow velocity. A turbulence intensity of 1% is considered low and greater than 10% is considered high.

.. code-block:: python

    # Turbulence intensity %
    'turbulence intensity': 0.01,

The eddy viscosity ratio :math:`(\mu_t/\mu)` varies depending type of flow.
For external flows this ratio varies from  0.1 to 1 (wind tunnel 1 to 10)

For internal flows there is greater dependence on Reynolds number as the largest eddies in the flow are limited
by the characteristic lengths of the geometry (e.g. The height of the channel or diameter of the pipe). Typical values are:

======= ======= ====== ======== ======== ======== ===========
  Re      3000   5000   10,000   15,000   20,000   > 100,000  
  eddy    11.6   16.5   26.7     34.0     50.1      100       
======= ======= ====== ======== ======== ======== ===========

.. code-block:: python

    # Eddy viscosity ratio
    'eddy viscosity ratio': 0.1,

The user can also provide functions to specify a 'wall-function' - or the turbulence viscosity profile near a boundary.  For example an atmospheric boundary layer (ABL) could be specified like this:

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

Certain conditions are specified relative to a reference set of conditions

.. code-block:: python

    'reference' : 'IC_1',
    # total pressure/reference static pressure
    'total pressure ratio' : 1.0,
    # total temperature/reference static temperature
    'total temperature ratio' : 1.0,
    # Mach number
    'mach' : 0.5,

.. code-block:: python

    'reference' : 'IC_1',
    # static pressure/reference static pressure
    'static pressure ratio' : 1.0,

.. code-block:: python
  
    'reference' : 'IC_1',
    # Mass flow ratio
    'mass flow ratio' : 1.0,

.. note::

    :math:`W^* =\frac{\dot{m}\sqrt{C_pT_0}}{p_0}`

Boundary Conditions
-------------------

Boundary condition properties are defined using consecutively numbered blocks like

.. code-block:: python

    'BC_1' : {....},
    'BC_2' : {....},

.. _wall:

Wall
^^^^

zCFD will automatically detect zone types and numbers in a number of mesh formats, and assign appropriate boundary conditions. The type tags follow the Fluent convention (wall = 3, etc), and if present no further information is required.  Alternatively, the mesh format may contain explicitly numbered zones (which can be determined by inspecting the mesh).  In this case, the user can specify the list of zone numbers for each boundary condition 'type' and 'kind' (see below).

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

For no slip walls and low Reynolds number :math:`(y^{+} \leq 1)` RANS meshes use

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
                    # Roughness field specified as a VTK file
                    'field' : 'roughness.vtp',
                  },

.. note::
    
    The roughness at each boundary face is set by finding the nearest point to the face centre on the supplied VTK file with the roughness 
    value looked up in a node based scalar array called 'Roughness'

Wall velocity

The wall can itself be moving with a prescribed linear (or rotating) velocity

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

The farfield boundary condition can automatically determine whether the flow is locally an inflow or an outflow by solving a Riemann equation.

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

Options

.. code-block:: python

    'profile' : {
                  # Atmospheric Boundary Layer
                  'ABL' : {
                            # Roughness length
                            'roughness length' : 0.05,
                            # Fiction velocity
                            'friction velocity' : 2.0,
                            # Surface Layer Height
                            'surface layer height' : 1000,
                            'Monin-Obukhov length' : 2.0,
                            # Turbulent kinetic energy
                            'TKE' : 1.0,
                            # Ground Level 
                            'z0' : 0.0,
                  },
    },

.. code-block:: python
  
    'turbulence' : {
                      'length scale' : 'filename.vtp',
                      'reynolds tensor' : 'filename.vtp',
    },

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
    'kind' : 'pressure',
    # Outflow conditions
    'condition' : 'IC_3',

For massflow specified outflows use

.. code-block:: python
    
    'kind' : 'massflow',

.. note:: 

    This boundary condition is specified by a static pressure ratio or massflow ratio that needs to be defined by the 
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

Periodic
^^^^^^^^

This boundary condition needs to be specified in pairs with opposing transformations.
The transforms specified should map each zone onto each other.

.. code-block:: python

    # Specific zone index
    'zone' : [1],
    # Type of boundary condition
    'type' : 'periodic',
    # Periodic boundary condition
    'kind' : {
              # Rotated periodic settings
              'rotated' : {
                           'theta' : math.radians(120.0),
                           'axis' : [1.0,0.0,0.0],
                           'origin' : [-1.0,0.0,0.0],
                          },
             },

or

.. code-block:: python

    'zone' : [1],
    'type' : 'periodic',
    'kind' : {
              # linear periodic
              'linear' : {
                           'vector' : [1.0,0.0,0.0],
                          },
             },


Reporting
---------

In addition to standard flow field outputs (see below), zCFD can provide information at monitor points in the flow domain, and integrated forces across all parallel partitions

.. code-block:: python

    'report' : {
                  # Report frequency
                  'frequency' : 1,
                  # Extract specified variable at fixed locations
                  'monitor' : {
                                 # Consecutively numbered blocks 
                                 'MR_1' : {
                                            # Name
                                            'name' : 'mast_1',
                                            # Location
                                            'point' : [49673.0, 58826.0, 1120.0],
                                            # Variables to be extracted
                                            'variables' : ['V','ti'],
                                          },
                              },

                  # Report force coefficient in grid axis as well as using user defined transform
                  'forces' : {
                        # Consecutively numbered blocks
                        'FR_1' : {
                                     # Name 
                                     'name' : 'wall',
                                     # Zones to be included
                                     'zone' : [11,12,13,14,15,20,21,22,23,24,25,26,27,28],
                                     # Transformation function
                                     'transform' : my_transform,
                                     # Reference area
                                     'reference area' : 0.112032,
                                 },
                      },
                },

Example Transformation Function
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code-block:: python

    # Angle of attack
    alpha = 10.0
    # Transform into wind axis
    def my_transform(x,y,z):
        v = [x,y,z]
        v =  zutil.rotate_vector(v,alpha,0.0)
        return {'v1' : v[0], 'v2' : v[1], 'v3' : v[2]}

Output
------

For solver efficiency, zCFD outputs the raw flow field data (plus any user-defined variables) to each parallel partition without re-combining the output files  

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

Options

.. code-block:: python

    'format' : 'vtk'

.. code-block:: python

    'format' : 'ensight'

.. code-block:: python

    'format' : 'native'

.. code-block:: python

    'scripts' : ['paraview_catalyst1.py','paraview_catalyst2.py']


.. topic:: Output Variables

   ===================== ===================== ==================================
    Variable Name         Alias                 Definition                   
   ===================== ===================== ==================================
    temperature           T, t                                               
    pressure              p                                                  
    density               rho                                                
    velocity              V, v                                               
    cp                                          :math:`C_p=\frac{P}{0.5\rho V^2}`                            
    mach                  m                                                  
    viscosity             mu                                                 
    kinematicviscosity    nu                                                   
    gauge_pressure                                                             
    vorticity                                                                  
    Qcriterion                                                               
    turbulenceintensity   ti                                                  
    eddy                                                                     
    cell_velocity                                                            
    centre                                                                   
    walldistance                                                             
    walldistancezone                                                         
    parent                                                                   
   ===================== ===================== ==================================

  Surface only quantities

   ===================== ===================== ================================== 
    Variable Name         Alias                 Definition                   
   ===================== ===================== ==================================
    pressureforce                                                            
    pressuremoment                                                           
    pressuremomentx                                                          
    pressuremomenty                                                          
    pressuremomentz                                                          
    frictionforce                                                            
    frictionmoment                                                           
    roughness                                                                
    ut                                                                       
    yplus                                                                    
    zone                                                                     
    cf                                                                       
   ===================== ===================== ================================== 

    

