########################
Data assimilation cycles
########################

The ``DATA_ASSIMILATION_CYCLES`` setting within CESM denotes how many model
integration, filter and inflation cycles will be attempted within a single CESM
job submission. This a typical setting for CAM:

.. code-block::

   STOP_N: 6
   STOP_OPTION: nhours
   DATA_ASSIMILATION_CYCLES=4

Within a single CESM ``./case.submit`` job, the model will run for 24 hours
with filter and inflation running at 6-hour intervals.

This is a typical setting for POP:

.. code-block::

   STOP_N: 1
   STOP_OPTION: ndays
   DATA_ASSIMILATION_CYCLES=5

So within a single CESM ``./case.submit`` job, the model will run for 5 days
with filter and inflation running daily.

.. note::

   The CESM ``RESUBMIT`` setting is distinct from ``DATA_ASSIMILATION_CYCLES``.
   The value that ``RESUBMIT`` is set to denotes how many times
   ``./case.submit`` will be invoked. Each submitted job must be completed
   within the period denoted by ``JOB_WALLCLOCK_TIME``.

