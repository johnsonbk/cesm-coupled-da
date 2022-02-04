##########
Reanalysis
##########

The coupler history files from the Reanalysis are available both from:

- NCAR's `Research Data Archive <https://rda.ucar.edu/datasets/ds345.0/>`_ and
- on Campaign storage.

To access the files on Campaign storage you'll need to log onto either Casper 
or the data-access nodes. Campaign storage is not mounted on Cheyenne.

.. code-block::

   ssh <user>@data-access.ucar.edu
   cd /gpfs/csfs1/cisl/dares/Reanalyses/f.e21.FHIST_BGC.f09_025.CAM6assim.011/cpl/hist

Each ensemble member's coupler history files are stored in their own
subdirectories, ``0001``, ``0002``, ``0003``, ... ``0080``.

