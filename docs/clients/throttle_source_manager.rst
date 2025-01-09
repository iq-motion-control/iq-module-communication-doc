Throttle Source Manager
-------------------------

This client is used to configure your module's redundant throttle settings.

Arduino
~~~~~~~

To use the Throttle Source Manager in Arduino, ensure throttle_source_manager.hpp is included. This
allows the creation of a ThrottleSourceManager object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the ThrottleSourceManager is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    ThrottleSourceManager throttleSourceManager(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int dronecan_prio = 0;
        if(ser.get(throttleSourceManager.dronecan_priority_, dronecan_prio))
        Serial.println(dronecan_prio);
    }

C++
~~~

To use the Throttle Source Manager in C++, include throttle_source_manager.hpp. This allows the
creation of an ThrottleSourceManager object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the ThrottleSourceManager is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "throttle_source_manager.hpp"

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Throttle Source Manager object with obj_id 0
        ThrottleSourceManager throttleSourceManager(0);

        // Use the Throttle Source Manager Client
        throttleSourceManager.dronecan_priority_.get(com);

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the Throttle Source Manager client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a ThrottleSourceManager object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the ThrottleSourceManager is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make a ThrottleSourceManager object with obj_id 0
    SourceManager = ThrottleSourceManager(’com’,com);

    % Use the ThrottleSourceManager object
    dronecan_throttle_prio = SourceManager.get(dronecan_priority);

Python
~~~~~~

To use the Throttle Source Manager Client in Python, import ``iqmotion`` and create a module that has the Throttle Source Manager Client within its firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Throttle Source Manager Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    dronecan_prio = |variable_name|.get("throttle_source_manager", "dronecan_priority") 
    print(f"DroneCAN Priority: {dronecan_prio}")

Message Table
~~~~~~~~~~~~~

Type ID 104 | Throttle Source Manager
    .. table:: Throttle Source Manager
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name             | Access         | Data Type | Unit                            | Note                                                                                                                                                                              |
        +========+========================+================+===========+=================================+===================================================================================================================================================================================+
        | 0      | throttle_timeout       | get, set, save | float     | Seconds                         | Defines the amount of time, in seconds, that the Throttle Source Manager will wait for a new message from the highest received priority before checking the next lowest priority. |
        +--------+------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 1      | dronecan_priority      | get, set, save | uint8     |                                 | The relative priority of DroneCAN throttles against hobby and IQUART throttles. Must be a value [0, 3] where 0 means DroneCAN is ignored, and 3 is the highest priority.          |
        +--------+------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | hobby_priority         | get, set, save | uint8     |                                 | The relative priority of hobby throttles against DroneCAN and IQUART throttles. Must be a value [0, 3] where 0 means hobby is ignored, and 3 is the highest priority.             |
        +--------+------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | iquart_priority        | get, set, save | uint8     |                                 | The relative priority of IQUART throttles against DroneCAN and hobby throttles. Must be a value [0, 3] where 0 means IQUART is ignored, and 3 is the highest priority.            |
        +--------+------------------------+----------------+-----------+---------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
       