###############
Troubleshooting
###############

Problems can arise if the DART executables weren't built with the same
compilers that were used to compile the netCDF library.

.. code-block::

   vim /glade/scratch/damrhein/GNYF.f09_g17_e80/run/da.log.2810502.chadmin1.ib0.cheyenne.ucar.edu.220209-231033

.. error::

   Wed Feb  9 23:21:33 MST 2022 -- BEGIN FILTER /glade/scratch/damrhein/GNYF.f09_g17_e80/bld/filter: error while loading shared libraries: libhwloc.so.15:
   cannot open shared object file: No such file or directory MPT ERROR: could not run executable.
   HPE MPT 2.19  02/23/19 05:31:12

Rebuilding the DART executables
===============================

As a first attempt at troubleshooting, try rebuilding the DART executables.

.. code-block::

   cd /glade/work/johnsonb/git/DAN_DART_2021-11-17/models/POP/work
   ./quickbuild.csh
   ./setup_CESM_hybrid_ensemble.csh
   Building cesm with output to /glade/scratch/johnsonb/GNYF.f09_g17_e3/bld/cesm.bldlog.220214-161400
   Time spent not building: 1286.737632 sec
   Time spent building: 1622.594832 sec
   MODEL BUILD HAS FINISHED SUCCESSFULLY
   cd /glade/work/johnsonb/cases/GNYF.f09_g17_e3
   ./case.submit -M begin,end
   [...]
   Wait for run to finish
   [...]
   ./CESM_DART_config.csh
   ./case.submit -M begin,end
   [...]
   Wait for run to finish
   [...]
   vim /glade/scratch/johnsonb/GNYF.f09_g17_e3/run/GNYF.f09_g17_e3.pop.dart_log.2010-01-04-00000.out

This rebuilt case ran successfully. Check to see if the DART state output is
present.

.. code-block::

   cd /glade/scratch/johnsonb/GNYF.f09_g17_e3/run
   ls *prior*
   GNYF.f09_g17_e3.pop.output_priorinf_mean.2010-01-04-00000.nc
   GNYF.f09_g17_e3.pop.output_priorinf_sd.2010-01-04-00000.nc
   GNYF.f09_g17_e3.pop.preassim_priorinf_mean.2010-01-04-00000.nc
   GNYF.f09_g17_e3.pop.preassim_priorinf_sd.2010-01-04-00000.nc
   input_priorinf_mean.nc
   input_priorinf_sd.nc
   ls *output*
   GNYF.f09_g17_e3.pop.output_mean.2010-01-04-00000.nc
   GNYF.f09_g17_e3.pop.output_priorinf_mean.2010-01-04-00000.nc
   GNYF.f09_g17_e3.pop.output_priorinf_sd.2010-01-04-00000.nc
   GNYF.f09_g17_e3.pop.output_sd.2010-01-04-00000.nc

This output matches the ``stages_to_write`` entry in ``input.nml``.

