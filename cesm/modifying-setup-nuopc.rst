#################################
Modifying setup scripts for NUOPC
#################################

.. note::

   It isn't actually necessary to use a setup script to create a multi-instance
   CESM case to test NUOPC functionality. CIME's ``./create_newcase`` script
   can be invoked with the ``--ninst`` option instead.

Overview
========

The setup scripts for coupled CESM runs in the DART ``9.X.X`` releases are
already Manhattan compliant, however they invoke CESM utilities that are no
longer present in CESM2.0.

Upgrading these setup scripts to test NUOPC functionality involves removing 
references to CESM1 utilities and replacing them the analagous utilities from
CIME that CESM2 makes use of.

Example
-------

For example, setting up CESM1 made heavy use of environmental variables that
were accessed using the ``Tools/ccsm_getenv`` utility. 

The analagous functionality in CESM2 uses CIME's ``xmlquery`` utility, since
many of these variables are now stored in xml configuration files rather than 
environmental variables.

Tractable path
==============

To learn which aspects of the setup scripts need to be modified, you can diff
the existing ``cam-fv`` and ``pop`` scripts, since both CESM1 and CESM2
versions of these scripts are contained within the DART repository.

You should begin by modifying the CESM perfect model obs setup script in
``DART/models/CESM/shell_scripts/CESM1_1_1_setup_pmo``.

POP example
-----------

.. code-block::

   cd DART/models/pop/shell_scripts
   diff cesm1_x/CESM1_1_1_setup_pmo cesm2_1/setup_CESM_perfect_model.csh

CAM example
-----------

.. code-block::

   cd DART/models/cam-fv/shell_scripts
   diff cesm1_5/setup_hybrid cesm2_1/setup_hybrid

