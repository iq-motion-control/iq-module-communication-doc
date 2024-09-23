Anticogging
-----------

Anticogging is the process of electronically canceling out cogging torque of a motor. Each motor is loaded
with its unique cog information. This class allows enabling and disabling the anticogging process. Though
this class can also manipulate the cog information it is not recommended to manipulate or erase this data
as it is unrecoverable.

Arduino
~~~~~~~

To use the Anticogging in Arduino, ensure iq_module_communication.hpp is included. This allows the
creation of a AnticoggingClient object. See the Message Table below for available messages. All message objects use the
Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the AnticoggingClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    IqSerial ser(Serial2);
    
    BrushlessDriveClient mot(0);
    AnticoggingClient cog(0);
    
    void setup() {
        ser.begin();
        ser.set(mot.drive_spin_volts_,0.0f);
    }

    void loop() {
        // Spin the motor with your hand and feel Anticogging turning on and off
        ser.set(cog.is_enabled_,(uint8_t)1);
        delay(2000);
        ser.set(cog.is_enabled_,(uint8_t)0);
        delay(2000);
    }

C++
~~~

To use the Anticogging client in C++, include anticogging client.hpp. This allows the creation of an 
AnticoggingClient object. See the Message Table below for available messages. All message objects use the Short Name with a
trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the AnticoggingClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: C++

    #include "generic_interface.hpp"
    #include "anticogging_client.hpp"
    
    void main(){
        // Make a communication interface object
        GenericInterface com;
        
        // Make an Anticogging Client object with obj_id 0
        AnticoggingClient cog(0);
        
        // Use theAnticogging Client object
        cog.is_enabled_.set(com, 1);
        
        // Insert code for interfacing with hardware here
    
    }

Matlab
~~~~~~

To use the Anticogging client in Matlab, all Vertiq communication code must be included in your path. This
allows the creation of a AnticoggingClient object. See the Message Table below for available messages. All message strings
use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the AnticoggingClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);

    % Make an AnticoggingClient object with obj_id 0
    Anticogging = AnticoggingClient(’com’,com);

    % Use the AnticoggingClient object
    Anticogging.set(’is_enabled’,1);    

Python
~~~~~~

To use the Anticogging Client in Python, import ``iqmotion`` and create a module that has the Anticogging Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Anticogging Client is:

.. code-block:: python
    :substitutions:

    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0, firmware="servo") |module_name_comment|
    
    |variable_name|.set("anticogging", "is_enabled", 1)  # Turns on Anticogging


Message Table
~~~~~~~~~~~~~

Type ID 71 | Anticogging

    .. table:: Anticogging
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+---------------+----------------+-----------+------------------------------+-----------------------------------------------------------------------------------------------------------+
        | Sub ID |  Short Name   |     Access     | Data Type |      Unit                    |                                                   Note                                                    |
        +========+===============+================+===========+==============================+===========================================================================================================+
        | 0      | table_size    | get            | uint16    |                              | Size of the anticogging table                                                                             |
        +--------+---------------+----------------+-----------+------------------------------+-----------------------------------------------------------------------------------------------------------+
        | 1      | is_data_valid | get            | uint8     | :math:`\text{bool}`          | Indicates if the cog information is valid.  is_enabled must be called first to check the cog information. |
        +--------+---------------+----------------+-----------+------------------------------+-----------------------------------------------------------------------------------------------------------+
        | 2      | is_enabled    | get, set, save | uint8     | :math:`\text{bool}`          | Indicates if anticogging is running.  This will stay 0/false if the is_data_valid field is 0/false.       |
        +--------+---------------+----------------+-----------+------------------------------+-----------------------------------------------------------------------------------------------------------+
        | 3      | erase         | set            |           |                              | Erases the cog information.  This is not recommended.                                                     |
        +--------+---------------+----------------+-----------+------------------------------+-----------------------------------------------------------------------------------------------------------+
        | 4      | left_shift    | get, set, save | uint8     | :math:`* 2^x`                | Anticog multiplier.  Modification is not recommended.                                                     |
        +--------+---------------+----------------+-----------+------------------------------+-----------------------------------------------------------------------------------------------------------+
