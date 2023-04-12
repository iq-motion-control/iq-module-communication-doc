.. include:: ../text_colors.rst
.. toctree::

.. _manual_dronecan:

***********************************************
DroneCAN
***********************************************

DroneCAN (previously known as UAVCAN or UAVCANv0), is an open protocol for communication over a CAN bus. DroneCAN is supported by both PX4
and Ardupilot flight controllers. Refere to the `DroneCAN documentation <https://dronecan.github.io/>`_ for more information on the protcol and the standard.

Module Support
===============

Speed Modules
**************

.. table:: Speed Module Support for DroneCAN

	+-------------+------------------------------------+
	| Module      | DroneCAN Support                   |
	+-------------+------------------------------------+
	| Vertiq 8108 | .. centered:: |:white_check_mark:| |
	+-------------+------------------------------------+
	| Vertiq 4006 | .. centered:: |:white_check_mark:| |
	+-------------+------------------------------------+
	| Vertiq 2306 | .. centered:: |:x:|                |
	+-------------+------------------------------------+

Servo Modules
**************
Servo modules do not support DroneCAN, as shown in the table below.

.. table:: Servo Module Support for DroneCAN

	+-------------+------------------------------------+
	| Module      | DroneCAN Support                   |
	+-------------+------------------------------------+
	| Vertiq 8108 | .. centered:: |:x:|                |
	+-------------+------------------------------------+
	| Vertiq 4006 | .. centered:: |:x:|                |
	+-------------+------------------------------------+
	| Vertiq 2306 | .. centered:: |:x:|                |
	+-------------+------------------------------------+

Standard DroneCAN Support
================================
Vertiq modules that support DroneCAN support a standard set of messages, requests, and configuration parameters. For information on the DroneCAN messages, 
requests, and parameters supported by Vertiq modules, see the `Standard DroneCAN Support documentation <https://www.vertiq.co/s/Preliminary-Standard-DroneCAN-UAVCANv0-Support.pdf>`_ 
on Vertiq’s website.

Flight Controller Integration
================================
For guidance on integrating Vertiq modules with flight controllers using DroneCAN, see 
the `Vertiq DroneCAN Setup Guide with PX4 <https://www.vertiq.co/s/Preliminary-Vertiq-DroneCAN-aka-UAVCANv0-Setup-Guide-with-PX4.pdf>`_ on Vertiq’s website.

.. _dronecan_arming_and_bypass:

Arming and Arming Bypass
================================
DroneCAN can use the same advanced arming procedure as all other throttle sources. The details of this arming procedure are covered in the :ref:`manual_advanced_arming` section.

Older versions of the firmware for Vertiq modules did not include support for arming over DroneCAN. To maintain backwards compatibility, it is possible for users to toggle 
arming integration with DroneCAN on or off. This is called “arming bypass”. When Vertiq modules have arming bypass turned on for DroneCAN they will spin on any :ref:`DroneCAN throttle command <throttle_sources_dronecan>`, 
regardless of armed state. Additionally, when arming bypass is on DroneCAN throttle commands will not cause :ref:`arming or disarming transitions <arming_state_transitions>`. 
The advanced arming features are completely ignored by DroneCAN when the arming bypass is on.

Arming bypass for DroneCAN can be controlled with the *DroneCAN Bypass Arming* parameter in the Advanced tab of the IQ Control Center, as shown below. When this parameter is set to *Bypass Arming*, 
arming bypass is on for DroneCAN, and when it is set to *Normal Arming*, DroneCAN will use the advanced arming feature like all other throttle sources.

.. figure:: ../_static/manual_images/dronecan/dronecan_bypass_arming.png
    :align: center
    :width: 60%
    :alt: DroneCAN Bypass Arming Parameter

    DroneCAN Bypass Arming Parameter in IQ Control Center