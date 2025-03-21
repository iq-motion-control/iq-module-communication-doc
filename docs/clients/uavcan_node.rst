UAVCAN Node
---------------------
This client is used to configure the DroneCAN interface on a Vertiq module.

Arduino
~~~~~~~

To use the UAVCAN Node in Arduino, ensure uavcan_node_client.hpp is included. This
allows the creation of a UavcanNodeClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the UavcanNodeClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    UavcanNodeClient uavcanNode(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int uavcanNodeId = 0;
        if(ser.get(uavcanNode.uavcan_node_id_, uavcanNodeId))
        Serial.println(uavcanNodeId);
    }

C++
~~~

To use the UAVCAN Node client in C++, include uavcan_node_client.hpp. This allows the
creation of an UavcanNode object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the UavcanNodeClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "uavcan_node_client.hpp"

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a UAVCAN Node object with obj_id 0
        UavcanNodeClient uavcanNode(0);

        // Use the UAVCAN Node Client
        uavcanNode.uavcan_node_id_.get(com);

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the UAVCAN Node client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a UavcanNodeClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the UavcanNodeClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a UavcanNodeClient object with obj_id 0
    UavcanNode = UavcanNodeClient(’com’,com);

    % Use the UavcanNodeClient object
    uavcanNodeId = UavcanNode.get(’uavcan_node_id’);

Python
~~~~~~

To use the UAVCAN Node Client in Python, import ``iqmotion`` and create a module that has the Arming Handler Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the UAVCAN Node Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    uavcan_node_id = |variable_name|.get("uavcan_node", "uavcan_node_id") 
    print(f"UAVCAN Node ID: {uavcan_node_id}")

Message Table
~~~~~~~~~~~~~

Type ID 80 | UAVCAN Node
    .. table:: UAVCAN Node
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name              | Access         | Data Type | Unit                            | Note                                                                                                                                                                                                                                                                                                                                                                                                            |
        +========+=========================+================+===========+=================================+=================================================================================================================================================================================================================================================================================================================================================================================================================+
        | 0      | uavcan_node_id          | get, set, save | uint32    | :math:`\text{ID [1, 127]}`      | This ID is used by the module to uniquely identify itself on the DroneCAN bus. The module must be power cycled in order for a new ID to take effect. Each module on the bus must have a unique ID. While the IDs can be a number between 1 and 127, it is recommended to avoid using 1 as this is commonly used by flight controllers.                                                                          |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 1      | uavcan_esc_index        | get, set, save | uint32    | :math:`\text{Index [0, 19]}`    | This is used to identify the module when ESC commands are sent on the DroneCAN bus. Each ESC/module on the bus requires a unique index to identify which commands are intended for it. The ESC indexes range from 0 to 19, with the first module starting at index 0.                                                                                                                                           |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | zero_behavior           | get, set, save | uint32    | :math:`\text{Enum}`             | This determines how the module reacts to receiving a zero setpoint from a DroneCAN Raw Command when DroneCAN is bypassing arming. 0 \= coast, 1 \= Brake, 2 \= Normal Controller                                                                                                                                                                                                                                |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | last_error_code         | get            | uint8     | :math:`\text{Error Code}`       | This represents the error code of the last CAN failure. 0 means no error.                                                                                                                                                                                                                                                                                                                                       |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 4      | receive_error_counter   | get            | uint8     | :math:`\text{Number of errors}` | This counter increments with each CAN receive error.                                                                                                                                                                                                                                                                                                                                                            |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 5      | transmit_error_counter  | get            | uint8     | :math:`\text{Number of errors}` | This counter increments with each CAN transmit error.                                                                                                                                                                                                                                                                                                                                                           |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 6      | bus_off_flag            | get            | uint8     | :math:`\text{Bool}`             | This flag indicates if the bus turned itself off due to errors. 1 means the bus is off.                                                                                                                                                                                                                                                                                                                         |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 7      | error_passive_flag      | get            | uint8     | :math:`\text{Bool}`             | This flag indicates if the error limit to enter passive mode has been reached. 1 means the module is past the passiv error limit.                                                                                                                                                                                                                                                                               |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 8      | error_warning_flag      | get            | uint8     | :math:`\text{Bool}`             | This flag indicates if the error warning limit has been reached. 1 means there has been enough errors to trigger a warning.                                                                                                                                                                                                                                                                                     |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 9      | telemetry_frequency     | get, set, save | uint32    | :math:`\text{Hz}`               | This represents the frequency with which telemetry will be sent to the DroneCAN bus.                                                                                                                                                                                                                                                                                                                            |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 10     | bit_rate                | get, set, save | uint32    | :math:`\frac{\text{Bit}}{s}`    | This represents the bitrate used by the DroneCAN bus.                                                                                                                                                                                                                                                                                                                                                           |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 11     | bypass_arming           | get, set, save | uint8     | :math:`\text{Bool}`             | This setting allows the module to bypass arming with DroneCAN throttle messages. DroneCAN messages will not be impacted by the arming state, will not cause arming state transistions, and will use the zero behavior setting.                                                                                                                                                                                  |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 12     | arming_by_arming_status | get, set, save | uint8     | :math:`\text{Bool}`             | This setting allows the module to arm and disarm based on the DroneCAN ArmingStatus message. Flight controllers are responsible for transmitting the ArmingStatus message, the module is a listener only. If this value is true, your module will arm based on the ArmingStatus message. Otherwise, normal arming applies. You must set :ref:`manual_arming_throttle_source` when arming with ArmingStatus      |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 13     | telemetry_style         | get, set, save | uint8     | :math:`\text{Bool}`             | Setting this to 0 will output the ESC Status and :ref:`ESC Status Extended <status_extended>` as telemetry. Setting this to 1 will outputs the ESC Status and Device Temperature as telemetry.                                                                                                                                                                                                                  |
        +--------+-------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+