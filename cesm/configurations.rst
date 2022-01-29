###########
CESM basics
###########

Community Earth System Model (CESM) installation instructions are available via
the `README <https://github.com/ESCOMP/CESM>`_ on the GitHub repository. The
cube sphere grid is available as of CESM2.2.0.

Cloning and installing
======================

.. important::

   CESM has already been ported and should work "out of the box" on most of the
   supercomputers that are widely used in the geosciences community, including
   Pleiades. When compiling the model, ensure to set the machine command line
   option, ``--mach`` to match the supercomputer you are working on.

.. code-block::

   cd <installation_directory>
   git clone https://github.com/ESCOMP/CESM.git cesm2_1_3
   cd cesm2_1_3
   git checkout release-cesm2.1.3
   ./manage_externals/checkout_externals

Commonly used grids
===================

The CESM documentation includes a `comprehensive list of grids
<https://www.cesm.ucar.edu/models/cesm2/config/grids.html>`_. Typically,
grids have descriptive long names when they are defined such as ``fv0.9x1.25``
-- which is the atmospheric finite volume ~1° grid -- or ``gx1v7`` -- which is
the seventh version of the oceanic displaced Greenland pole ~1° grid.

These long names are shortened when the atmospheric/land grids are coupled to
the ocean/sea ice grid. Instaed of ``fv0.9x1.25`` and ``gx1v7``, the shortened
name becomes ``f09_g17``.

Atmospheric grids
-----------------

The workhorse atmospheric grid is the ~1° finite-volume f09 grid, which is
used for CMIP experiments and the Large Ensemble. Other grids used for iHESP
are the ``f05`` ~0.5° finite volume grid and the ``f02`` ~0.25° finite volume
grid.

Cube sphere
~~~~~~~~~~~

As of CESM2.2.0, CAM supports a spectral element dynamical core (CAM-SE) on
cube-sphere grids. Lauritzen et al. (2017) [1]_ list the available cube-sphere
grids in their Table 1. A subset of their table is reproduced here.

+------------------------+--------------------------+-------------------------+
| Grid name              | Average node spacing     | Model timestep          |
+========================+==========================+=========================+
| ne16np4                | ∼208 km                  | 1,800 s                 |
+------------------------+--------------------------+-------------------------+
| ne30np4                | ∼111 km                  | 1,800 s                 |
+------------------------+--------------------------+-------------------------+
| ne60np4                | ∼56 km                   | 900 s                   |
+------------------------+--------------------------+-------------------------+
| ne120np4               | ∼28 km                   | 450 s                   |
+------------------------+--------------------------+-------------------------+
| ne240np4               | ∼14 km                   | 225 s                   |
+------------------------+--------------------------+-------------------------+

The analog to the ~1.0° f09 finite volume grid is the ne30 ~1.0° spectral
element grid.

.. note::

   In addition to supporting the spectral element dynamical core on these 
   grids, CAM also supports GFDL's FV3 dynamical core on the ~1.0° C96 grid. 
   For more information, see the `CAM developmental compsets documentation
   <https://ncar.github.io/CAM/doc/build/html/users_guide/atmospheric-configurations.html#cam-developmental-compsets>`_.

Oceanic grids
-------------

Low-resolution
~~~~~~~~~~~~~~

The workhorse oceanic grid is the ~1° displaced Greenland pole grid. There
are two configurations of it that CESM2.1.3 is compatible with, ``gx1v6 (g16)``
and ``gx1v7 (g17)``. These grids are identical, except in ``g17``, the Caspian
Sea has been removed from the ocean/sea ice domain and inserted into the land
domain.

===== =====
g16   g17
----- -----
|g16| |g17|
===== =====

High-resolution
~~~~~~~~~~~~~~~

The eddy-resolving grid is ~0.1° Poseidon Tripole grid. Again, just like with
the low-resolution grid, there are two configurations of it that CESM2.1.3 is 
compatible with, ``tx0.1v2 (t12)`` and ``tx01.v3 (t13)``. These grids are
identical, except in ``t13``, the Caspian Sea has been removed from the
ocean/sea ice domain and inserted into the land domain.

===== =====
t12   t13
----- -----
|t12| |t13|
===== =====

Building a case
===============

The scripts for building cases within CESM are part of a software collection
known as the Common Infrastructure for Modeling the Earth (CIME). This software
supports both NCAR models and those developed within the Department of Energy's
Energy Exascale Earth System Model (E3SM) collection. Thus the build scripts to
create a new case are contained within the ``cime`` subdirectory.

.. code-block::

   cd <installation_directory>/cesm2_1_3/cime/scripts
   ls 
   create_clone    create_test        fortran_unit_testing  query_config     tests
   create_newcase  data_assimilation  lib                   query_testlists  Tools

The ``create_newcase`` script is invoked and passed command line arguments to
build a new case.

+-----------------------+-----------------------------------------------------------------+
| Command line option   | Meaning                                                         |
+=======================+=================================================================+
| ``--case``            | The directory the case will be built in. It is common practice  |
|                       | to include the experiment's grid resolution and component set   |
|                       | (described below) in the name of the case so that these aspects |
|                       | can be easily identified when browsing the file system later.   |
+-----------------------+-----------------------------------------------------------------+
| ``--compset``         | The component set of the experiment, including which            |
|                       | models will be actively integrating (atmosphere, land, ocean,   |
|                       | sea ice) and what boundary forcing will be used. CESM has an    |
|                       | extensive list of `component set definitions                    |
|                       | <https://www.cesm.ucar.edu/models/cesm2/config/compsets.html>`_ |
|                       | and these instructions using the ``FHIST`` compset, which has   |
|                       | an active atmospheric component, the Community Atmosphere Model |
|                       | version 6, and historical sea surface forcing, staring in 1979. |
+-----------------------+-----------------------------------------------------------------+
| ``--res``             | The grid resolution the model will run on. Each grid includes   |
|                       | at least two parts, the atmospheric/land grid and the ocean/sea |
|                       | ice grid to which it is coupled. These instructions use a       |
|                       | low-resolution finite volume grid for the atmosphere,           |
|                       | ``fv0.9x1.25`` and couple it to a ~1° ocean/sea ice grid,       |
|                       | ``gx1v7``. These grid names are truncated into ``f09_g17``.     |
|                       | Again, CESM has an extensive list of `available grids           |
|                       | <https://www.cesm.ucar.edu/models/cesm2/config/grids.html>`_.   |
+-----------------------+-----------------------------------------------------------------+
| ``--mach``            | The upercomputer the case will be built on. These instructions  |
|                       | build a case on NCAR's Cheyenne computer, however, if you are   |
|                       | building on Pleiades, consult the table in the note below.      |
+-----------------------+-----------------------------------------------------------------+
| ``--project``         | The account code the project will be run on. When jobs from the |
|                       | experiment are run, the specified account will automatically be |
|                       | debited. Replace ``PXXXXXXXX`` with your project code.          |
+-----------------------+-----------------------------------------------------------------+
| ``--run-unsupported`` | Since the cube-sphere grid is a newly released aspect of CESM   |
|                       | that is not used in Coupled Model Intercomparison Project runs, |
|                       | it is not considered a scientifically supported grid yet. In    |
|                       | order to use it, you need to append this option.                |
+-----------------------+-----------------------------------------------------------------+

.. note::

   If you are building on ``pleiades``, the core layout per node differs based
   on which nodes you are using. These differences are alreay accounted for 
   within CESM. When specifying ``--mach`` there are four valid options:
   
   ======================  ===============================
   Compute node processor  Corresponding ``--mach`` option
   ----------------------  -------------------------------
   Broadwell               ``pleiades-bro``
   Haswell                 ``pleiades-has``
   Ivy Bridge              ``pleiades-ivy``
   Sandy Bridge            ``pleiades-san``
   ======================  ===============================

To build a case using the ~1° ``f09`` finite volume grid:

.. code-block::

   ./create_newcase --case /glade/work/johnsonb/cesm_runs/FHIST.cesm2_1_3.f09_g17.001 --compset FHIST --res f09_g17 --mach cheyenne --project PXXXXXXXX --run-unsupported
   [...]
   Creating Case directory /glade/work/johnsonb/cesm_runs/FHIST.cesm2_1_3.f09_g17.001
   
The case directory has successfully been created. Change to the case directory
and set up the case.

.. code-block::

   cd /glade/work/johnsonb/cesm_runs/FHIST.cesm2_1_3.f09_g17.001
   ./case.setup

The ``case.setup`` script scaffolds out the case directory, creating the
``Buildconf`` and ``CaseDocs`` directories that you can customize. These
instructions use the default configurations and continue on to compiling the
model. On machines that don't throttle CPU usage on the login nodes, the 
``case.build`` command can be invoked. On Cheyenne, however, CPU intensive
activities are killed on the login nodes, you will need to use a build wrapper
to build the model on a shared compute node and specify a project code. Again,
replace ``PXXXXXXXX`` with your project code.

.. code-block::

   qcmd -q share -l select=1 -A PXXXXXXXX -- ./case.build

The model build should progress for several minutes. If it compiles properly,
a success message should be printed.

.. code-block::

   Time spent not building: 6.320388 sec
   Time spent building: 603.685347 sec
   MODEL BUILD HAS FINISHED SUCCESSFULLY

The model is actually built and run in a user's scratch space.

.. code-block::

   /glade/scratch/johnsonb/FHIST.cesm2_1_3.f09_g17.001/bld/cesm.exe

Submitting a job
================

To submit a job, change to the case directory and use the ``case.submit`` 
script. The ``-M begin,end`` option sends the user an email when the job starts
and stops running.

When the case is built, its default configuration is to run for five model
days. This setting can be changed to run for a single model day using 
``./xmlchange STOP_N=1``.

.. code-block::

   cd /glade/work/johnsonb/cesm_runs/FHIST.cesm2_1_3.f09_g17.001
   ./xmlchange STOP_N=1
   ./case.submit -M begin,end
   [...]
   Submitted job id is 2658061.chadmin1.ib0.cheyenne.ucar.edu
   Submitted job case.run with id 2658060.chadmin1.ib0.cheyenne.ucar.edu
   Submitted job case.st_archive with id 2658061.chadmin1.ib0.cheyenne.ucar.edu

Restart file
============

After the job completes, restart files are written to the run directory which
is also in scratch space. These restart files are written for both active and
data components. The CAM restart file contains a ``cam.r`` substring. By
default, the ``FHIST`` case begins on January 1st, 1979. Thus, the restart file
will be for January 2nd, 1979.

.. code-block::

   /glade/scratch/johnsonb/FHIST.cesm2_1_3.f09_g17.001/run/FHIST.cesm2_1_3.f09_g17.001.cam.r.1979-01-02-00000.nc

The fields in the restart file can be plotted using various langauges such as 
MATLAB or Python's matplotlib.

References
==========

.. [1] Lauritzen, P. H., and Coauthors, 2018: NCAR Release of CAM-SE in
       CESM2.0: A Reformulation of the Spectral Element Dynamical Core in
       Dry-Mass Vertical Coordinates With Comprehensive Treatment of
       Condensates and Energy. Journal of Advances in Modeling Earth Systems,
       **10**, 1537–1570,
       `doi:10.1029/2017MS001257 <https://doi.org/10.1029/2017MS001257>`_.

.. |g16| image:: /_static/g16.png
   :width: 525px
 
.. |g17| image:: /_static/g17.png
   :width: 525px

.. |t12| image:: /_static/t12.png
   :width: 525px

.. |t13| image:: /_static/t13.png
   :width: 525px

