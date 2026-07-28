.. include:: ../text_colors.rst
.. toctree::

.. _vspin_transition_guide:

#################################
VSpin Transition Guidance
#################################
This documentation is meant to help users who are transitioning from using the stock firmware to the VSpin firmware. It highlights key considerations that may affect you as you test VSpin if you have already set up your modules with the stock firmware.

.. note:: 
    Before using Control Center with VSpin, we recommend updating your Control Center to v1.11.0 or later for full support
    
.. _vspin_tuning_changes:

******************************************************
VSpin Velocity and Position Gain Tuning Changes
******************************************************
The :ref:`closed loop velocity controller <manual_velocity_control_mechanisms>` and :ref:`closed loop position controller <manual_angle_control_mechanisms>` now output a motor current command instead of a voltage command. This means that the units of the controllers’ gains have changed, and so the specific gains used for any tuning will be different. 
**It is important to understand this change. Gains that worked well for the previous stock firmwares will not provide the same performance if you change to the VSpin firmware.** If you are using velocity or position setpoints, we recommend repeating the tuning process when transitioning to the VSpin firmware to find appropriate gains.

.. note::
    If you are transitioning from using a module that was previously configured on the stock speed or servo firmware, you may want to consider :ref:`resetting to defaults <reset_to_defaults_manual>`. This resets your module’s gains to the default settings as a starting point, but would also reset any other parameters that were previously configured, such as arming ranges, 
    so it is recommended to take note of your existing configuration to be able to replicate any parameters that you do want to carry over before resetting to defaults.

.. _vspin_api_transition:

******************************************************
VSpin API Usage
******************************************************
The VSpin firmware eliminates the :ref:`Brushless Drive client <brushless_drive>` and splits its functionality among several new IQUART clients. This documentation is meant to provide guidance on how to transition from using Brushless Drive to these new clients when moving from the stock speed or servo firmware to the VSpin firmware.

Python API Usage
============================================================
Make sure you have the latest version of the `Python API <https://pypi.org/project/iqmotion/>`_. The Python API can be used as normal, but when creating the module object, a `VSpinG2Module <https://github.com/iq-motion-control/iq-module-communication-python/blob/master/iqmotion/iq_devices/vspin_g2_module.py>`_ should be used to allow access to these new clients.

Drive Control Interface
============================================================
The Drive Control Interface is a newly introduced client that allows for direct control of the underlying drive. This client allows for sending commands directly to the drive in the same way that it was previously possible to directly command the :ref:`Brushless Drive client <brushless_drive>`, so it can serve to simplify the transition to 
VSpin if you were previously commanding Brushless Drive. This interface also makes it possible to send the module torque and motor current commands for modules that use VSpin. For more information on what entries are available for the Drive Control Interface, see the :ref:`Drive Control Interface message table <drive_control_interface_api>`.

Querying Motor Model and State Information With VSpin
============================================================
With the removal of the :ref:`Brushless Drive client <brushless_drive>`, several API entries that could be used to gather basic information on the module's state will no longer be available. This includes information on the module's current velocity and details about its model's tuning. Substitutes for these entries can generally be found in the newly added :ref:`Motor Model client <motor_model_api>`, :ref:`Current Safeties client <current_safeties_api>`, and :ref:`Motor Driver client <motor_driver_api>`.

The tables below provides guidance on how to transition some of the most commonly used entries from Brushless Drive:

Motor Velocity
--------------------------------------
.. table:: Motor Velocity API Entries for VSpin
    
        ==============  ===============  ====================
        Firmware Style  Client           Entry               
        ==============  ===============  ====================
        Stock           brushless_drive  obs_velocity          
        VSpin           motor_model      mechanical_velocity   
        ==============  ===============  ====================

Drive Voltage
--------------------------------------
.. table:: Drive Voltage API Entries for VSpin
    
        ==============  ===============  ====================
        Firmware Style  Client           Entry               
        ==============  ===============  ====================
        Stock           brushless_drive  drive_volts          
        VSpin           motor_driver     stator_magnitude   
        ==============  ===============  ====================

Estimated Motor Current
--------------------------------------
.. table:: Estimated Motor Current API Entries for VSpin
    
        ==============  ===============  ====================
        Firmware Style  Client           Entry               
        ==============  ===============  ====================
        Stock           brushless_drive  est_motor_amps          
        VSpin           motor_driver     estimated_motor_amps
        ==============  ===============  ====================

Drive Mode
--------------------------------------
.. table:: Drive Mode API Entries for VSpin

        ==============  ===============  ====================
        Firmware Style  Client           Entry               
        ==============  ===============  ====================
        Stock           brushless_drive  drive_mode          
        VSpin           motor_driver     driver_mode
        ==============  ===============  ====================
