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

.. _standard_dronecan_support:

Standard DroneCAN Support
================================
This section details the standard DroneCAN messages supported across all Vertiq DroneCAN modules currently. The structure and contents of these messages are defined by the 
DroneCAN specification, this section just provides details on how exactly Vertiq modules support them and some supplementary information. 

Details on the DroneCAN protocol can be found on the `DroneCAN specification <https://dronecan.github.io/Specification/1._Introduction/>`_. For a full listing of all standard messages 
specified by DroneCAN, see the `List of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/>`_ in the DroneCAN specification.

Broadcast Messages
********************
These are broadcast messages that are sent to or from the motors. Since they are broadcast, there is no response message sent. These messages typically make up the majority of 
communication on the bus during operation.

uavcan.protocol.NodeStatus (Data Type ID = 341)
################################################
The DroneCAN protocol requires that all nodes on a DroneCAN network periodically publish their status using the Node Status message. Vertiq modules support this behavior to conform 
to the standard. A uavcan.protocol.NodeStatus message is published at 1 Hz during operation, reporting its health, current mode, and uptime.

In normal operation a Vertiq module should always report its Health as OK, and its mode as Operational. No sub-modes or vendor specific status codes are currently supported.

Refer to the `uavcan.protocol.NodeStatus section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#nodestatus>`_ in the specification 
for more details.

.. _dronecan_support_esc_status:

uavcan.equipment.esc.Status (Data Type ID = 1034)
##################################################
This message is published periodically and provides telemetry updates on the state of the motor and its inputs. Specifically it contains information on:

* **Error Count**: This is a counter of CAN bus errors, specifically it details the number of transmit errors the motor has encountered.
* **Voltage**: The input voltage to the motor in volts
* **Current**: The current draw of the motor in amps
* **Temperature**: The temperature of the motors coils in Kelvin
* **RPM**: The current speed of the motor in RPM
* **Power Rating Percentage**: The PWM duty cycle percentage being applied to the motor, from 0% to 100%. Maximum power draw occurs at 100% duty cycle
* **ESC Index**: The ESC index of the motor that is broadcasting this update

The frequency that this message is published at is determined by the :ref:`dronecan_support_telemetry_frequency` configuration parameter.

Refer to the `uavcan.equipment.esc.Status section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#status-2>`_ in the specification 
for more details

.. _dronecan_support_device_temperature:

uavcan.equipment.device.Temperature (Data Type ID = 1110)
##########################################################
This message is published periodically and provides updates on the temperature of the microcontroller used on the controller. The fields contained in this message are:

* **Device ID**: The ESC index of the motor sending this broadcast
* **Temperature**: The temperature of the microcontroller in Kelvin. Note that this is different from the temperature in the ESC status message, as that is the temperature of the coils.
* **Error Flags**: Indicates if the motor is overheating

The frequency that this message is published at is determined by the :ref:`dronecan_support_telemetry_frequency` configuration parameter.

Refer to the `uavcan.equipment.device.Temperature section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#temperature>`_ in the specification 
for more details


.. _dronecan_messages_raw_command:

uavcan.equipment.esc.RawCommand (Data Type ID = 1030)
######################################################
Used to control the speed and direction of the motor. This message should be sent from the flight controller to the motors. The payload consists of a list of up to 20 values, 
with each value interpreted as a 14 bit signed integer. Each entry in the list corresponds to a motor with the given ESC index. E.g. The first entry in the list of raw commands 
will be read by the motor that has been assigned ESC index 0, the second entry will be read by the motor with ESC index 1, and so on. 

The values represent the speed and direction to command the motor to, normalized over a range of [-8192, 8191], with -8192 being full speed in reverse and 8191 being full speed forward.

Refer to the `uavcan.equipment.esc.RawCommand section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#rawcommand>`_ in the specification 
for more details

Service Requests
*****************
Service requests are messages sent to a specific target node from another node, and to which the sending node expects to receive a response message.

uavcan.protocol.GetNodeInfo (Data Type ID = 1)
################################################
This request has no payload fields. The response message from the receiving node should include the status of the node as defined by the NodeStatus message, the 
software and hardware version of the node, and the name of the node. For Vertiq modules, the name of each node is currently “iq_motion.esc”.

Refer to the `uavcan.protocol.GetNodeInfo section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#getnodeinfo>`_ in the specification 
for more details

uavcan.protocol.param.GetSet (Data Type ID = 11)
##################################################
Used to get or set the value of a configuration parameter using either the index or the name of the parameter. The configuration parameters currently available on standard 
Vertiq modules are listed in the :ref:`dronecan_configuration_parameters` section below. The request message should contain the index of the parameter or the name of the parameter depending 
on how you are accessing it, and it should contain a value to set the parameter to if you are setting it. **Parameter indices are not guaranteed to remain consistent across 
firmware versions. Indices should only be used for parameter discovery, when accessing the parameter directly it is recommended to always use the name**

The response message will contain the value of the parameter, information about its default, minimum, and maximum values, and the name of the parameter.

Refer to the `uavcan.protocol.param.GetSet section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#getset>`_ in the specification 
for more details

uavcan.protocol.RestartNode (Data Type ID = 5)
###############################################
This request has one field, its “magic number”. This field is an unsigned 40 bit integer. The magic number is used to identify that this is an intentional restart request, 
and we have also extended it to allow for two different types of reboots. One magic number performs a normal reboot that simply restarts into the normal application firmware, 
and the other reboots the device into the ST bootloader, which allows it to be programmed with new firmware. The reboot-into-bootloader can be useful for firmware updates. 
The numbers for each are:

* Normal Reboot Magic Number = 0xACCE551B1E
* Reboot to Bootloader Magic Number = 0xBEEFCAFE

The response to this request contains one boolean field. This field will be true if the number received was a valid magic number and the motor is restarting, and false if the 
number received was not one of the valid numbers.

Refer to the `uavcan.protocol.RestartNode section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#restartnode>`_ in the specification 
for more details.

uavcan.protocol.file.BeginFirmwareUpdate (Data Type ID = 40)
#############################################################
On modules that support DroneCAN firmware updates, this command causes the module to enter into upgrade mode, and begin attempting to firmware update over DroneCAN. 
The request includes a source node ID and a file path on that source node. The module will attempt to get information on the file from the file server, and then begin reading the data from the upgrade file.

On modules that do not support DroneCAN firmware updates, the module will respond with an INVALID_MODE error code to this request.

Refer to the `uavcan.protocol.file.BeginFirmwareUpdate section of Standard Data Types <https://dronecan.github.io/Specification/7._List_of_standard_data_types/#beginfirmwareupdate>`_ in the 
specification for more details.

.. _dronecan_configuration_parameters:

Configuration Parameters
**************************

Node ID
########

.. table::

	+----------------+----------+
	| **Name**       | **Type** |
	+----------------+----------+
	| uavcan_node_id | Integer  |
	+----------------+----------+

This parameter defines the Node ID of the module on the UAVCAN network. This ID is how the node identifies itself when sending and receiving messages. No two nodes should have 
the same Node ID. ID 0 is typically reserved for the flight controller. A reboot is typically required after changing this parameter for the device to use the new Node ID on the network.

This parameter can also be changed through the IQ Control Center if you wish to change this without using DroneCAN.

.. _dronecan_bitrate_parameter:

Bitrate
########
.. table::

	+----------------+----------+
	| **Name**       | **Type** |
	+----------------+----------+
	| bit_rate       | Integer  |
	+----------------+----------+

This parameter determines the DroneCAN bitrate that the module will use in bit/s. This parameter takes effect immediately when changed, so if this is changed it 
will most likely be necessary to reconnect to the bus as at the new bitrate to continue communicating with the module. Note that most Vertiq modules do not currently 
support this parameter, support will be added in future releases. If this parameter is not supported, the module’s bitrate is fixed at 500000 bit/s.

ESC Index
##########
.. table::

	+----------------+----------+
	| **Name**       | **Type** |
	+----------------+----------+
	| esc_index      | Integer  |
	+----------------+----------+

This parameter defines the ESC index of the module. The ESC index is used when a Raw Command message is received to determine which value in the Raw Command should be read by the module.

This parameter can also be changed through the IQ Control Center if you wish to change this without using DroneCAN.

Zero Behavior
##############
.. table::

	+----------------+----------+
	| **Name**       | **Type** |
	+----------------+----------+
	| zero_behavior  | Integer  |
	+----------------+----------+

This parameter defines the behavior of the motor when it receives a zero setpoint in a Raw Command message, **but only if the module is** :ref:`bypassing arming on DroneCAN<dronecan_arming_and_bypass>`. 
If the module is using :ref:`normal arming <manual_advanced_arming>` with DroneCAN, the Zero Behavior is not used, instead the :ref:`disarm behavior <advanced_disarming_behavior>` provides
a similar functionality that integrates with arming and disarming.

There are three possible behaviors: Coast, Brake, and Normal Controller. By changing the integer value of Zero Behavior, the user can determine which of these behaviors is used.

Descriptions of each behavior and the integer value of the Zero Behavior parameter 
corresponding to them are given below:

* **Coast (Value = 0)**: The module will stop trying to drive the motor, releasing control and letting it simply coast to a stop.
* **Brake (Value = 1)**: The motor will brake to stop itself, so the motor will stop suddenly on a zero setpoint instead of coasting gently to a stop. After stopping, it will then coast.
* **Normal Controller (Value = 2)**: The normal controller continues to run, and will try to drive the motor at 0% throttle. This can cause vibrating or twitching in velocity mode if the PID gains are high.

This parameter can also be changed through the IQ Control Center if you wish to change this without using DroneCAN.

.. _dronecan_support_telemetry_frequency:

Telemetry Frequency
####################
.. table::

	+------------------+----------+
	| **Name**         | **Type** |
	+------------------+----------+
	| telem_frequency  | Integer  |
	+------------------+----------+

This parameter defines the frequency in Hz with which the telemetry messages (:ref:`uavcan.equipment.esc.Status <dronecan_support_esc_status>` 
and :ref:`uavcan.equipment.device.Temperature <dronecan_support_device_temperature>`) are broadcast by the module. 
For example, if this value were set to 10, the telemetry would be sent at a rate of 10 Hz. If this parameter is set to 0, no telemetry messages will be sent.

This parameter can also be changed through the IQ Control Center if you wish to change this without using DroneCAN.

Motor Armed
##############
.. table::

	+------------------+----------+
	| **Name**         | **Type** |
	+------------------+----------+
	| motor_armed      | Integer  |
	+------------------+----------+

This parameter is available on modules that support :ref:`manual_advanced_arming`. It can be used to query and control the armed state of the module. See the 
section on :ref:`user arming commands over DroneCAN <arming_user_command_dronecan>` for more information.

Motor Stowed
##############
.. table::

	+------------------+----------+
	| **Name**         | **Type** |
	+------------------+----------+
	| motor_stowed     | Integer  |
	+------------------+----------+

This parameter is available on modules that support :ref:`manual_stow_position`. It can be used to command and release stow on modules. See the section on
:ref:`manual stow over DroneCAN <trigger_manual_stow_dronecan>` for more information.

Stow Status
############
.. table::

	+------------------+----------+
	| **Name**         | **Type** |
	+------------------+----------+
	| stow_status      | Integer  |
	+------------------+----------+

This parameter is available on modules that support :ref:`manual_stow_position`. It reports on the current state of the stow feature on the module. See the
:ref:`stow_status` section for more information.

Stow Result
############
.. table::

	+------------------+----------+
	| **Name**         | **Type** |
	+------------------+----------+
	| stow_result      | Integer  |
	+------------------+----------+

This parameter is available on modules that support :ref:`manual_stow_position`. It reports on how the previous stow attempt ended. See the
:ref:`stow_result` section for more information.

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