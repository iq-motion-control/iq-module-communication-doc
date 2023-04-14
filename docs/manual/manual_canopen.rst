.. include:: ../text_colors.rst
.. toctree::

.. _manual_canopen:

***********************************************
CANOpen
***********************************************
Vertiq's CANOpen implementation is based on the CAN in Automation (CiA) 301 standard. All Vertiq modules connect to the CANBUS as a Slave Node with a bitrate of 500kbps. The bitrate is fixed.

Module Support
================
Speed Modules
**************

.. table:: Speed Module Support for CANOpen

	+-------------+------------------------------------+
	| Module      | CANOpen Support                    |
	+-------------+------------------------------------+
	| Vertiq 8108 | .. centered:: |:x:|                |
	+-------------+------------------------------------+
	| Vertiq 4006 | .. centered:: |:x:|                |
	+-------------+------------------------------------+
	| Vertiq 2306 | .. centered:: |:x:|                |
	+-------------+------------------------------------+

Servo Modules
**************

.. table:: Servo Module Support for CANOpen

	+-----------------+------------------------------------------+
	| Module          | CANOpen Support                          |
	+-----------------+------------------------------------------+
	| Vertiq 8108     | .. centered:: |:x:|                      |
	+-----------------+------------------------------------------+
	| Vertiq 4006     | .. centered:: |:x:|                      |
	+-----------------+------------------------------------------+
	| Vertiq 2306     | .. centered:: |:x:|                      |
	+-----------------+------------------------------------------+
	| Vertiq Fortiq42 | .. centered:: |:white_check_mark:|       |
	+-----------------+------------------------------------------+


CANOpen Implementation
=======================
Vertiq supports standard message objects EMCY, PDO, SDO, NMT, as well as LSS. 

Object Dictionary
******************
Vertiq’s manufacturer specific Object Dictionary objects are described in the following table:

.. table:: Vertiq Manufacturer Specific Object Dictionary

	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| OD Index | Name / Subindex   | Data Name           | Units     | Data Type | Access     | Description                                                          |
	+==========+===================+=====================+===========+===========+============+======================================================================+
	| 0x2000   | Go to Position    |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | Position            | rad       | float     | SDO RW     | Move to a desired location, specified in radians, away from 0 rad    |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2001   | Control PWM       |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | PWM                 | N/A       | float     | SDO RW     | Spin the motor at a specified PWM [-1, 1]                            |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2002   | Control Volts     |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | Voltage             | Volts     | float     | SDO RW     | Spin the motor at a specified Voltage [-Vbat, Vbat]                  |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2003   | Control Velocity  |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | Velocity            | rad / s   | float     | SDO RW     | Spin the motor at a specified Velocity                               |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2004   | Trajectory        |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | No. Entries         | N/A       | uint8     |            | The number of entries in the Trajectory Object Directory Entry       |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x01              | Displacement        | rad       | float     | SDO RW     | The desired ending position of the trajectory                        |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x02              | Angular Velocity    | rad / s   | float     | SDO RW     | The maximum velocity with which to perform the trajectory            |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x03              | Acceleration        | s         | float     | SDO RW     | The acceleration with which to reach the average trajectory velocity |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x04              | Trajectory Duration | rad / s   | float     | SDO RW     | The length of time it should take to complete the trajectory         |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x05              | Average Velocity    | rad / s^2 | float     | SDO RW     | The average velocity with which to complete the trajectory           |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2005   | Stop Command      |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | No. Entries         | N/A       | uint8     | SDO RW     | The number of entries in the Stop Command Object Directory Entry     |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x01              | Coast               | N/A       | uint8     | SDO RW     | A command to coast the motor                                         |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x02              | Brake               | N/A       | uint8     | SDO RW     | A command to brake the motor                                         |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2006   | Max Velocity      |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | Velocity            | rad / s   | float     | SDO RW     | The maximum velocity the motor may reach                             |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2007   | Observed Angle    |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | Observed Angle      | rad       | float     | SDO/PDO RO | The current observed angle                                           |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	| 0x2008   | Observed Velocity |                     |           |           |            |                                                                      |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+
	|          | 0x00              | Observed Velocity   | rad / s   | float     | SDO/PDO RO | The current observed velocity                                        |
	+----------+-------------------+---------------------+-----------+-----------+------------+----------------------------------------------------------------------+

The *Access Column* specifies how each object dictionary entry may be accessed. All spin control parameters are read/write accessible via the Service Data Object (SDO) protocol. The real time variables *Observed Angle* and *Observed Velocity* are read only accessible through either SDO or the Process Data Object (PDO) Protocol. To interact via PDO, you must have a CANOpen node capable of creating a SYNC message. Only after reception of a SYNC message will the observed angle and velocities be read via PDO. 

In the 32-bit value received on a PDO transfer, the first 4 Bytes represent the observed angle, and the last 4 represent the observed velocity. 

Node ID
********
All Fortiq modules default to a CANOpen Node-ID of 1. This value is user settable via the Layer Setting Services (LSS) protocol. Node ID changes via LSS will be saved to persistent memory, meaning you must only change the value once in your initial system setup. 

Please refer to `this document <https://us.nanotec.com/products/manual/PD4E_CANopen_EN/bus%252Fcan%252Flss.html?cHash=0bd15c1cd3340dfc546f95e5c1f85a12>`_ for further LSS service instructions.

Please note, in order to change your Node-ID, the motor must be in LSS configuration mode, all LSS commands contain 8 bytes of data, and Node ID is not part of the COB-ID.

Sample CANOpen Project with Fullmo Kickdrive
=============================================

The following are images and descriptions of a project in `Fullmo Kickdrive <https://kickdrive.de/en/index.htm>`_. Fullmo Kickdrive is used to connect with a `PCAN-USB interface <https://www.peak-system.com/PCAN-USB.199.0.html?&L=1>`_ to a CANBUS with 1 or more connected Fortiq motors. 

.. _Spinning your Module:

Spinning your Module
*********************

#. Open Fullmo Kickdrive
#. Create a new project using CANopen Nodes Easy Setup 

	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_easy_setup_screen.png

#. Connect with the PCAN-USB interface using the CAN Interface tab. Make sure Communication Port is set to “peak” and Baud Rate to 500K

	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_startInterface.png

#. Select “Start Interface,” and the bar should turn green and read “Running”

	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_running.png

#. On the top bar, select the down arrow on User Level, and select Expert. The password to move forward is *expert*

	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_user_level.png

#. If connecting more than one motor, right click on CANopen Node 01, select Copy Module

#. Right click again and select Paste Module to create another CANopen Node
	
	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_multiple_motors.png

#. Connect your motor(s) to the CANBUS as in the diagram below, and power it/them on
	
	.. image:: ../_static/manual_images/fortiq/canopen/canopen_circuit.png

#. Open the Object Editor tab from the Module list, and select Open Dict. 
	
	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_open_object_editor.png

#. Select the supplied fortiq.xdd file from `Vertiq’s website <https://www.vertiq.co/>`_

#. Set Node ID to 1 (or your current Node ID)

#. From the Object Dictionary, double click controlVelocity, it will open in the Object Editor

	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_open_od.png

#. Double Click the Value/Editor box put 10 (set up to set the velocity to 10 rad/sec)

#. Deselect the box, and reselect the whole entry so that it turns blue

#. Select Write, and your motor should begin to spin
	
#. To stop your motor, click the drop-down on stopCommand, and double click Coast

#. Set Coast to 1, and click Write

Changing your Node-ID
***********************

In order to change your module's Node-ID please first follow steps 1-8 from :ref:`Spinning your Module` to connect to the CANBUS

.. note::
	If you are connecting more than one module to the CANOpen bus, and desire independent control of each, you must set unique Node-IDs. To set unique Node-IDs, you must complete the following steps for **one motor** at a time. Connecting multiple modules to the bus and performing these steps will change both nodes' Node-ID to the same value.

#. Open *CAN Sender*
	
	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_cansender.png

#. Replace the default program with the following (a script to set a Node ID to 5), and enter your desired nodeId

	.. code-block::

			nodeId = 5
			send canid(0x7E5),8,04 canunsigned8(1) 00 00 00 00 00
			send canid(0x7E5),8,11 canunsigned8(nodeId) 00 00 00 00 00

#. Click Play

#. In CAN Monitor, you should see 2 TX messages followed by a received message

	.. image:: ../_static/manual_images/fortiq/canopen/kickdrive_change_nodeid_response.png

#. Reboot your motor, and you should see its Node ID has changed

#. You will now be able to control multiple modules independently when all are properly connected to the CANOpen bus