####################################
Forcing ocean and sea ice components
####################################

Runs with active ocean and sea ice components that are forced with prescribed
atmospheric fluxes can be customized by editing the data atmosphere namelist
and streams files.

.. note::

   The data atmosphere fields can come from any source as long as they contain
   the requisite forcing fields. The easiest way to generate a "correct" set of
   atmospheric forcing fields is to use coupler history files from a CESM run
   in which CAM is active, but forcing from other reanalyses and data products
   such as JRA-55 and GISS are also acceptable.

The CIME documentation does contain `instructions for configuring data atmosphere 
files <https://esmci.github.io/cime/versions/master/html/users_guide/cime-change-namelist.html#customizing-data-model-input-variable-and-stream-files>`_.
However, the instructions contained in that document are vague:

   "Edit the user_datm.streams.txt.* file."

Setting up a case to get acquainted with the files
==================================================

Setting up a ``G`` compset case is the simplest way to become familiar with the
data atmosphere stream files (``datm.streams*``).

.. code-block::

   cd <cesm_root>/cime/scripts/
   ./create_newcase --case /glade/work/${USER}/cases/G.fosi.f09_g17.001 --compset G --res f09_g17 --project PXXXXXXXX --run-unsupported 
   cd /glade/work/${USER}/cases/G.fosi.f09_g17.001
   ./case.setup
   ./preview_namelists

The ``preview_namelists`` script will fill the ``CaseDocs`` with the namelist
and data streams files necessary to build a forced ocean/sea ice (FOSI) run.

.. code-block::

   ls CaseDocs/
   atm_modelio.nml                   datm.streams.txt.presaero.clim_2000   ice_in           seq_maps.rc
   cpl_modelio.nml                   drof_in                               ice_modelio.nml  wav_in
   datm_in                           drof.streams.txt.rof.diatren_ann_rx1  lnd_modelio.nml  wav_modelio.nml
   datm.streams.txt.CORE2_NYF.GISS   drv_in                                ocn_modelio.nml
   datm.streams.txt.CORE2_NYF.GXGXS  esp_modelio.nml                       pop_in
   datm.streams.txt.CORE2_NYF.NCEP   glc_modelio.nml                       rof_modelio.nml

There are four ``datm.streams*`` files. They contain a lists of all of the
forcing fields from coupler history files that are necessary to conduct a FOSI
run.

Structure of a data atmosphere stream file
==========================================

The data atmosphere stream files are XML files that specify a domain file, a
variable file and a time offset.

.. code-block::

   vim CaseDocs/datm.streams.txt.CORE2_NYF.GISS

These are the XML nodes in a ``datm.streams*`` file:

.. code-block::

   <?xml version="1.0"?>
   <file id="stream" version="1.0">
     <dataSource>
        GENERIC
     </dataSource>
     <domainInfo>
       <variableNames>
         time    time
         lon      lon
         lat      lat
         area    area
         mask    mask
       </variableNames>
       <filePath>
         /glade/p/cesmdata/cseg/inputdata/atm/datm7/NYF
       </filePath>
       <fileNames>
         nyf.giss.T62.051007.nc
       </fileNames>
     </domainInfo>
     <fieldInfo>
       <variableNames>
         lwdn  lwdn
         swdn  swdn
         swup  swup
       </variableNames>
       <filePath>
         /glade/p/cesmdata/cseg/inputdata/atm/datm7/NYF
       </filePath>
       <fileNames>
         nyf.giss.T62.051007.nc
       </fileNames>
       <offset>
         0
       </offset>
     </fieldInfo>
   </file>

The ``dataSource`` node typically specifies where the data come from. While
this GISS file merely says ``GENERIC``, the dataSource node from the `CAM6
reanalysis <https://github.com/NCAR/DART/blob/main/models/POP/shell_scripts/cesm2_1/user_datm.streams.txt.CPLHISTForcing.Solar_template>`_ is more descriptive, ``CAM6-DART Ensemble Reanalysis (NCAR
RDA ds345.0)``.

The ``domainInfo`` node specifies a netCDF domain file and the variables it
contains, ``time``, ``lon``, ``lat``, ``area``, ``mask``. In this example, the
forcing fields are specified on the ``T62`` spectral grid.

The ``fieldInfo`` node specifies a netCDF variable file and the variables it
contains. There are two strings in the ``variableNames`` entry because the
data source might name a field differently than what the coupler is expecting.
For example, in ``CaseDocs/datm.streams.txt.CORE2_NYF.GXGXS``, the
precipitation variable is declared in this manner:

.. code-block::

   <variableNames>
      prc  prec
   </variableNames>

The pair of strings translate between the variable key in the GXGXS source file
(Large and Yeager, 2004) [1]_ which specifies this field as ``prc`` and the
key expected by the coupler, which is ``prec``.

Time offset and axis mode
=========================

The time offset, ``offset``, and time axis mode, ``taxmode``, are the trickiest
aspects of the ``datm.streams*`` files to get right.

The best explanation of these settings is in the CLM `Customizing the DATM
namelist <https://escomp.github.io/ctsm-docs/versions/release-clm5.0/html/users_guide/setting-up-and-running-a-case/customizing-the-datm-namelist.html>`_ documentation.

Specified fields
================

These are the fields that should be specified for a FOSI run:

- ``lwdn`` downwelling longwave radiation
- ``swdn`` downwelling shortwave radiation
- ``swup`` upwelling shortwave radiation
- ``prec`` precipitation
- ``dens`` density
- ``pslv`` sea level pressure
- ``shum`` specific humidity
- ``tbot`` 10-meter temperature
- ``u`` 10-meter zonal velocity
- ``v`` 10-meter meridional velocity

References
==========

.. [1] Large, W. G., and S. G. Yeager, 2004: Diurnal to decadal global forcing
       for ocean and sea-ice models: The data sets and flux climatologies.
       NCAR Tech. Note NCAR/TN-460+STR, 111 pp.
