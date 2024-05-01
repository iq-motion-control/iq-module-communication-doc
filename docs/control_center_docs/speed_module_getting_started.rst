.. include:: ../text_colors.rst
.. toctree::

.. _speed_module_start_guide:

####################################################################
Getting Started with Vertiq's Speed Firmware with IQ Control Center
####################################################################

Before completing the following getting started guide, please ensure that you have read and completed our :ref:`IQ Control Center 
guide <control_center_start_guide>`. It walks you through Control Center installation, module configuration, and basic testing options available 
to all Vertiq modules and firmware types. The following document is meant to provide additional configurations and testing options available only on Vertiq's 
speed firmware.

************************************
Flight Controller Parameters
************************************

The first set of configurations covered here are those that **must** be configured properly in order to control your module with a flight controller. 
These parameters affect how your module communicates with, and is controlled by a flight controller. 

* Mode: this parameter is available through the General tab, and defines whether your module accepts throttle commands as a voltage, a velocity, or a PWM. 
  To learn more about the Mode parameter see :ref:`manual_throttle`
* Direction: this parameter is available through the General tab, and defines the direction that your module takes to be positive, either clockwise or 
  counter-clockwise. You can also configure either 2D or 3D configuration to allow for mapping negative throttle commands. 
  For more on the Direction parameter and throttle mapping, see :ref:`throttle_direction`
* Communication: This parameter is available through the General tab, and defines the communication protocol expected by your module as sent by the 
  flight controller. This parameter can be Standard PWM, OneShot protocols, MultiShot, or DShot protocols. To learn more see :ref:`throttle_sources`
* Max Velocity and Max Volts: These parameters are available through the Tuning tab, and pair directly with the Mode parameter. This means that when your 
  mode is set to velocity, your module will only ever map a throttle command as bounded by ± *Max Velocity*. When your mode is set to voltage, your module 
  will only ever map a throttle as bounded by ± *Max Volts*. To learn more see :ref:`throttle_maximums`
* Arming regions: This parameter is available through the Tuning tab, and defines the throttle percentages to be treated as arming throttles. 
  To learn more see :ref:`throttle_regions`
* Disarming regions This parameter is available through the Tuning tab, and defines the throttle percentages to be treated as disarming throttles. 
  To learn more see :ref:`throttle_regions`
* Number of throttles for arming: This parameter is available through the Tuning tab, and defines the number of consecutively received throttle commands that 
  lie within the Arming region. To learn more see :ref:`arming_consecutive_throttles`

*******************************************************************************************
Example Module Flight Controller Configuration and ESC Testing with the Control Center
*******************************************************************************************

Configuration
===================
.. note::
    The following images are captured using IQ Control Center version 1.5.2 as connected to a Vertiq 40-06 Gen 2

Suppose that your module has the following requirements to function properly on your vehicle:

#. The module must spin clockwise at all times
#. The module must spin proportionally to a target velocity, with a maximum of 500 rad/s
#. The module must arm after 5 consecutively received throttle commands below 5% sent through a Standard [1-2]ms PWM signal

To configure your module to meet these requirements:

1. Connect with your module through IQ Control Center
2. In the General tab, find the Arm On Throttle parameter. Please note, the module used for this demonstration has been reset to factory defaults. 
   If you have previously configured your module, your values may not match identically to these images

.. image:: ../_static/control_center_pics/speed_getting_started/speed_default_arming_params.png
        
3. Now, we will configure the parameters required to meet our third requirement
   
   a. Make sure *Arm On Throttle* is set to *Arm On Throttle*
   b. Set *Arm Throttle Lower Limit* to 0
   c. Set *Arm Throttle Upper Limit* to 0.05 (5%)
   d. Set *Communication* to Standard PWM
   e. Set *Consecutive Arming Throttles to Arm* to 5
   
.. image:: ../_static/control_center_pics/speed_getting_started/speed_configured_arming_params.png

4. Still on the General tab, scroll down to find Mode

.. image:: ../_static/control_center_pics/speed_getting_started/speed_default_mode.png

5. Now, we will set the configurations to meet requirements 1 and the first part of 2

   a. Set *Mode* is set to *Velocity*
   b. Set *Motor Direction* to *2D clockwise*

.. image:: ../_static/control_center_pics/speed_getting_started/speed_configured_mode.png

6. Navigate to the Tuning tab, and find the *Max Velocity* parameter

.. image:: ../_static/control_center_pics/speed_getting_started/speed_max_velo.png

7. To meet the rest of requirement 2, set *Max Velocity* to 500

.. image:: ../_static/control_center_pics/speed_getting_started/speed_set_max_velo.png

Testing Flight Controller Configuration with IQ Control Center
===============================================================
To verify your module's configuration for flight controller control, the Control Center provides a simulated ESC Input testing option in the Testing tab.

.. image:: ../_static/control_center_pics/speed_getting_started/speed_esc_input.png