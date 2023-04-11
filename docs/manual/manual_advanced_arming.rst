.. include:: ../text_colors.rst
.. toctree::

.. _manual_advanced_arming:

***********************************************
Advanced Arming
***********************************************
Vertiq modules can support an advanced arming feature, allowing the user to control the armed state of the module with throttle commands or manually 
and to configure specific behaviors to occur at armed state transitions. Modules will not react to throttle messages until they have armed, providing improved safety. 
The configurable behaviors on armed state transitions allow users to easily integrate advanced behaviors into their setup just by controlling the throttle messages they send, 
simplifying flight controller integration. 

The :ref:`arming_module_support` section below details which Vertiq Advanced Speed modules currently support.

.. _arming_module_support:

Module Support
===============
.. table:: Module Support for Advanced Arming

	+-------------+------------------------------------+
	| Module      | IQUART Support                     |
	+-------------+------------------------------------+
	| Vertiq 8108 | .. centered:: |:white_check_mark:| |
	+-------------+------------------------------------+
	| Vertiq 4006 | .. centered:: |:white_check_mark:| |
	+-------------+------------------------------------+
	| Vertiq 2306 | .. centered:: |:white_check_mark:| |
	+-------------+------------------------------------+

Armed States
===============
Modules can be in one of two armed states at any time:

* **Armed**: In this state, the module will spin when it receives a :ref:`throttle command <manual_throttle>`, so it is not safe to be near a module with an attached propeller when the module is armed.
* **Disarmed**: In this state, the module will NOT spin when it receives a :ref:`throttle command <manual_throttle>`. If using IQUART to control the modules, it is still possible that the module can be commanded to spin by other commands, such as by setting the “ctrl_volts” of the Propeller Motor Controller client. Because of this, if you are using IQUART messages besides the typical throttle commands to control the motor you should approach it with caution even when it is disarmed. 

The process of transitioning between armed states is covered in the :ref:`arming_state_transitions` section.

Reboot Armed State
*******************
**After a reboot, regardless of what armed state the motor was in previously, it will start up in the disarmed state.** Users must transition the 
motor from disarmed to armed to begin spinning again following a reboot.

Armed Throttle Source Lockout
==============================
**When armed, the module will always choose one throttle source as its armed throttle source, and reject incoming throttle commands from all other throttle sources.** 
For an explanation on what is a throttle source, see the :ref:`throttle_sources` section. These rejected throttle commands will not affect how the module is spinning 
and will not trigger disarming transitions. For example, if DroneCAN was the armed throttle source, throttle commands received over DroneCAN would be treated 
normally, and throttle commands received over Hobby PWM would be rejected.

How the module determines the armed throttle source depends on how the arming transition is triggered. See the individual sections on types of arming transitions 
in :ref:`arming_state_transitions` for more details.

.. _arming_state_transitions:

Armed State Transitions
==========================
There are two types of armed state transitions:

* **Arming**: A transition from disarmed to armed. See :ref:`advanced_arming_behavior` for more details on what occurs during and after an arming transition.
* **Disarming**: A transition from armed to disarmed. See :ref:`advanced_disarming_behavior` for more details on what occurs during and after a disarming transition.

There are multiple ways to trigger armed state transitions covered in the sections below.

Throttle Regions
****************
It is possible to transition between armed states using only the throttle commands sent to the module. This can be accomplished by specifying arming throttle regions and 
disarming throttle regions. 

A **throttle region is a range of throttle commands specified by an upper bound and a lower bound.** Any throttle command that falls within that range or on one of the 
boundaries is considered to be in that throttle region. Throttle commands that are in the arming throttle region will contribute towards transitioning the module 
to the armed state, and throttle commands that are in the disarming throttle region contribute towards transitioning the module to the disarmed state.

Arming With the Arming Throttle Regions
########################################

Arming Throttle Region Definition
----------------------------------
The arming throttle region is specified by two parameters: *Arm Throttle Upper Limit* and *Arm Throttle Lower Limit*. These two parameters can be found in the IQ Control Center 
under the General tab, as shown below. Users can configure these to specify any range of throttle commands as the arming throttle region. When a sufficient number of 
throttle commands in the arming throttle region are received, an arming transition will occur. For more details on the number of arming throttle commands required to 
trigger an arming transition, see :ref:`arming_consecutive_throttles`. When the arming transition occurs, the module will perform its arming behavior as detailed in :ref:`advanced_arming_behavior`.

.. figure:: ../_static/manual_images/arming/arming_throttle_limits.png
    :align: center
    :width: 60%
    :alt: Arming Throttle Region Limits

    Arming Throttle Region Limits in IQ Control Center

The *Arm Throttle Upper Limit* defines the upper, more positive bound of the arming throttle region. It should always be more positive than or equal to the 
*Arm Throttle Lower Limit*. The *Arm Throttle Lower Limit* defines the lower bound of the arming throttle region. Every throttle command the module receives in 
this region is considered an arming throttle and will contribute towards an arming transition. The number of arming throttles required to cause an arming 
transition is determined by the *Consecutive Arming Throttles to Arm* parameter as discussed in the :ref:`arming_consecutive_throttles` section below.

The image below illustrates where the arming throttle region would be for an example setup where the Arm Throttle Upper Limit is set to 0.3 and the Arm Throttle Lower Limit 
is set to 0.1. Any throttle command between 10% and 30% is an arming throttle.

.. figure:: ../_static/manual_images/arming/arming_example_arming_throttle_region_1.png
    :align: center
    :width: 60%
    :alt: Example Arming Throttle Region

    Example Arming Throttle Region

**There is a separate parameter that determines whether the arming throttle region exists at all. The *Arm On Throttle* parameter, shown below in 
IQ Control Center, allows users to toggle the arming throttle region on or off completely.** If this parameter is set to *Do Not Arm on Throttle*, 
then there is no arming throttle region and throttles cannot cause an arming transition. Otherwise, arming throttles can be used as previously described.

.. figure:: ../_static/manual_images/arming/arming_arm_on_throttle_parameter.png
    :align: center
    :width: 60%
    :alt: Arm on Throttle Parameter

    Arm on Throttle Parameter in IQ Control Center

.. _arming_consecutive_throttles:

Consecutive Arming Throttles
------------------------------
**Users can also specify how many consecutive arming throttles the module needs to receive before the arming transition occurs.** This can be useful 
if the module is in a high-noise situation where there is a danger of noise being interpreted as a throttle command, or as a general safety 
feature to help prevent premature arming. 

The number of consecutive arming throttles the module must receive in the arming throttle region can be configured in the General tab of the
Control Center with the *Consecutive Arming Throttles to Arm* parameter, as shown below.

.. figure:: ../_static/manual_images/arming/arming_consecutive_throttles.png
    :align: center
    :width: 60%
    :alt: Consecutive Arming Throttles to Arm Parameter

    Consecutive Arming Throttles to Arm Parameter in IQ Control Center

**Note that the number of consecutive arming throttles received by the module resets whenever a throttle command outside of the arming throttle 
region is received. The module must receive the specified number of consecutive arming throttles all in a row, with no throttle commands outside of 
the arming throttle region coming in between them.** For example, if the module's *Consecutive Arming Throttles to Arm* parameter was set to 10, and 
it received 9 throttle commands in the arming throttle region, 1 outside of the region, and finally 1 throttle command in the region, 
the module would not arm. At the end of that sequence, the module has only received 1 consecutive arming throttle, because the throttle command 
from outside of the arming region reset the count.

**It is also important to note that the consecutive arming throttles must be from the same** :ref:`throttle source <throttle_sources>`. A change in the source of 
arming throttles will cause the count to reset. For example, if 5 arming throttles are sent over DroneCAN, and then 1 arming throttle 
is received over hobby PWM, the count of consecutive arming throttles will be 1. The change of source for the arming throttles caused a reset of the count. 

**To summarize, the module must receive a user-configurable number of consecutive throttle commands all in the arming throttle region and 
all from the same source to trigger an arming transition.**

Arming Example
----------------
An example of a possible arming setup and procedure is outlined below to clarify how arming with throttle commands works.

For this example, assume that the *Arm On Throttle* parameter is set to *Arm On Throttle*, the *Arm Throttle Upper Limit* is set 
to 0.15, the *Arm Throttle Lower Limit* is set to 0.0, and *Consecutive Arming Throttles to Arm* is set to 10. The figures below show what 
these settings look like in the IQ Control Center when applied to a module.

.. figure:: ../_static/manual_images/arming/arming_example_throttle_region_parameters.png
    :align: center
    :width: 60%
    :alt: Arm on Throttle and Arming Throttle Region Limits for Example

    Arm on Throttle and Arming Throttle Region Limits for Example

.. figure:: ../_static/manual_images/arming/arming_consecutive_throttles.png
    :align: center
    :width: 60%
    :alt: Consecutive Arming Throttles to Arm for Arming Example

    Consecutive Arming Throttles to Arm for Arming Example

An illustration of the arming throttle region defined by these parameters is shown below to demonstrate what throttle commands will be considered arming throttles.

.. figure:: ../_static/manual_images/arming/arming_example_throttle_region_2.png
    :align: center
    :width: 60%
    :alt: Arming Example Throttle Regions

    Throttle Region Illustration for Arming Example

Now that the module is properly configured we can begin sending throttle commands to it. The module is in the disarmed state initially. Also, 
for this example, we will assume there is only one source sending throttle commands. For more information on how multiple sources sending throttle 
commands affect the arming process, see :ref:`arming_consecutive_throttles`.

Imagine that we begin sending 30% throttle commands to the module. Nothing will happen in this case; the module will not spin and it will not arm. 
The module is disarmed, so throttle commands will not cause it to spin. These 30% throttle commands are outside the arming throttle region, 
so they will not cause the module to arm.

If we switch to sending -10% throttle commands, we can expect the same result. The module is still disarmed, and -10% is not in the arming throttle region.

Imagine instead that we begin sending 5% throttle commands. These throttle commands fall within the arming throttle region, so they are counted as arming 
throttles. The module will not begin an arming transition when it receives the first 5% throttle command however, as our *Consecutive Arming Throttles to Arm* 
parameter means that it must receive 10 consecutive arming throttles before it will arm. After 10 of these 5% throttle commands, the module will begin an 
arming transition. When the module arms, it will perform its arming behavior, as detailed in :ref:`advanced_arming_behavior`.


Disarming With the Disarming Throttle Region
##############################################

Disarming Throttle Region Definition
--------------------------------------

Disarming Throttle Source Requirement
--------------------------------------

Disarm Example
---------------

Throttle Region Overlap
########################

User Commands
**************

DroneCAN
#########

IQUART
#######

DSHOT Disarm
##############

Manual Arming Throttle Source
##############################

Timeout
********

Always Armed
*************

.. _advanced_arming_behavior:

Arming Behavior
================

.. _advanced_disarming_behavior:

Disarming Behavior
===================

Disarming Behavior Options
***************************

Disarming Song Playback Options
*********************************

Flight Controller Integration Examples
========================================

Arming and Disarming with Hobby PWM and PX4
********************************************

Module Setup
#############

Flight Controller Setup
########################

Arming and Disarming with DroneCAN and PX4
********************************************

Module Setup
#############

Flight Controller Setup
########################

Arming and Disarming with ArduCopter for Hobby PWM and DroneCAN
*****************************************************************

Module Setup
#############

Flight Controller Setup
########################