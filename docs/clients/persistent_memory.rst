Persistent Memory
-----------------

The Persistent Memory class controls the non-volatile memory. You can use this class to revert the motor
to factory defaults.

Arduino
~~~~~~~

To use Persistent Memory in Arduino, ensure iq_module_communication.hpp is included. This allows the
creation of a PersistentMemoryClient object. See the Message Table below for available messages. All message objects use
the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PersistentMemoryClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    PersistentMemoryClient mem(0);
    
    void setup() {
        ser.begin();
        ser.set(mem.revert_to_default_);
    }

    void loop() {
    }

C++
~~~

To use Persistent Memory in C++, include persistent memory client.hpp. This allows the creation of a
PersistentMemoryClient object. See the Message Table below for available messages. All message objects use the Short
Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the PersistentMemoryClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "persistent_memory_client.hpp"
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Persistent Memory object with obj_id 0
        PersistentMemoryClient mem(0);

        // Use the Persistent Memory object
        mem.revert_to_default_.set(com);

        // Insert code for interfacing with hardware here
    
    }

Matlab
~~~~~~

To use Persistent Memory in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a PersistentMemoryClient object. See the Message Table below for available messages. All message strings
use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the PersistentMemoryClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a PersistentMemoryClient object with obj_id 0
    mem = PersistentMemoryClient(’com’,com);
    
    % Use the PersistentMemoryClient object
    mem.set(’revert_to_default’);


Python
~~~~~~

To use the Persistent Memory Client in Python, import ``iqmotion`` and create a module that has the Persistent Memory Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Persistent Memory Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    |variable_name|.set("persistent_memory", "factory_default_key_1", 12345678)  # Set first key before erasing calibration data
    |variable_name|.set("persistent_memory", "factory_default_key_2", 11223344)  # Set second key before erasing calibration data
    |variable_name|.set("persistent_memory", "revert_to_default")  # erases saved values except for factory defaults


Message Table
~~~~~~~~~~~~~

Type ID 11 | Persistent Memory

+--------+-------------------+--------+-----------+------+---------------------------------------------------------------------------------------------+
| Sub ID | Short Name        | Access | Data Type | Unit | Note                                                                                        |
+========+===================+========+===========+======+=============================================================================================+
| 0      | erase             | set    |           |      | Erases all saved values including calibration data and product key. Highly not recommended. |
+--------+-------------------+--------+-----------+------+---------------------------------------------------------------------------------------------+
| 1      | revert_to_default | set    |           |      | Erases all saved values except for those set in factory.                                    |
+--------+-------------------+--------+-----------+------+---------------------------------------------------------------------------------------------+
| 2      | format_key_1      | set    |           |      | (Required) Set 12345678 to perform erase or revert_to_default.                              |
+--------+-------------------+--------+-----------+------+---------------------------------------------------------------------------------------------+
| 3      | format_key_2      | set    |           |      | (Required) Set 11223344 to perform erase or revert_to_default.                              |
+--------+-------------------+--------+-----------+------+---------------------------------------------------------------------------------------------+
