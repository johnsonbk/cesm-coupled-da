#########
Compiling
#########

Since DART is designed to minimize dependencies and maximize cross-platform
compatibility, compiling DART on Cheyenne is trivial.

.. code-block::

   cd /glade/work/$USER
   git clone https://github.com/NCAR/DART

Since Cheyenne is a Linux system with Intel processors, there is already a 
``mkmf.template`` that works. Move it into place.

.. code-block::

   cd DART/build_templates
   mv mkmf.template.intel.linux mkmf.template

Build the ``lorenz_63`` model to ensure the setup is correct.

.. code-block::

   cd ../models/lorenz_63/work
   ./quickbuild.csh
   [...]
   Success: All single task DART programs compiled.
   Script is exiting after building the serial versions of the DART programs.

