UAVCAN Node
-----------

UAVCANv0 (https://legacy.uavcan.org/) is a standard application level protocol commonly used for controlling drones. 
UAVCAN can be used to control and configure the motors over a CAN bus. While actual UAVCAN messages are sent over a CAN bus 
and not a serial connection, the serial connection can be used to configure certain important 
UAVCAN bus settings, such as the node ID and the ESC index of the motor.

Arduino
~~~~~~~

To use UAVCAN Node in Arduino, ensure iq module communication.hpp is included. This allows the creation
of a UAVCAN Node object. See Table for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the UAVCANNodeClient is:

.. code-block:: Arduino

    [TODO]

C++
~~~

To use UAVCAN Node in C++, include hobby input client.hpp. This allows the creation of a UAVCANNodeClient
object. See Table for available messages. All message objects use the Short Name with a trailing underscore.
All messages use the standard Get/Set/Save functions.

A minimal working example for the UAVCANNodeClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    [TODO]

Matlab
~~~~~~

To use UAVCAN Node in Matlab, all IQ communication code must be included in your path. This allows the
creation of a UAVCAN Node object. See Table for available messages. All message strings use the Short
Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the UAVCANNodeClient is:

.. code-block:: Matlab

    [TODO]

Python
~~~~~~

To use the UAVCAN Node Client in Python, include ``iqmotion`` and create a module that has the UAVCAN Node Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the UAVCAN Node Client is:

.. code-block:: Python

    [TODO]

.. :substitutions:

.. import iqmotion as iq

.. com = iq.SerialCommunicator("/dev/ttyUSB0")
.. |variable_name| = iq.|module_name|(com, 0|module_firmware|)

.. | variable_name | .set("hobby_input", "allowed_protocols", 4)   # Set the protocol to MultiShot |
.. | variable_name | .save("hobby_input", "allowed_protocols")     # Save the protocol             |


Message Table
~~~~~~~~~~~~~

Type ID 80 | UAVCAN Node

.. rst-class:: tight-table

+--------+------------------------+--------+-----------+------+------+
| Sub ID |       Short Name       | Access | Data Type | Unit | Note |
+========+========================+========+===========+======+======+
| 0      | uavcan_node_id         |        | uint32    |      |      |
+--------+------------------------+--------+-----------+------+------+
| 1      | uavcan_esc_index       |        | uint32    |      |      |
+--------+------------------------+--------+-----------+------+------+
| 2      | zero_behavior          |        | uint32    |      |      |
+--------+------------------------+--------+-----------+------+------+
| 3      | last_error_code        |        | uint8     |      |      |
+--------+------------------------+--------+-----------+------+------+
| 4      | receive_error_counter  |        | uint8     |      |      |
+--------+------------------------+--------+-----------+------+------+
| 5      | transmit_error_counter |        | uint8     |      |      |
+--------+------------------------+--------+-----------+------+------+
| 6      | bus_off_flag           |        | uint8     |      |      |
+--------+------------------------+--------+-----------+------+------+
| 7      | error_passive_flag     |        | uint8     |      |      |
+--------+------------------------+--------+-----------+------+------+
| 8      | error_passive_flag     |        | uint8     |      |      |
+--------+------------------------+--------+-----------+------+------+

