Serial Interface
----------------

The Serial client allows the user to change settings related to the serial communication interface, namely
the baud rate. The set function of the baud rate behaves as both a set then a save. This allows the user to
set and save using the initial baud rate, rather than having to disconnect and reconnect using the new baud
rate in order to send a save. For this reason, the standard save function for the baud rate is disabled.

Arduino
~~~~~~~

To use Serial Interface in Arduino, ensure iq_module_communication.hpp is included. This allows the creation
of a SerialInterfaceClient object. See the Message Table below for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the SerialInterfaceClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    SerialInterfaceClient sic(0);
    
    void setup() {
        ser.begin();
        ser.set(sic.baud_rate_,(uint32_t)9600);
    }

    void loop() {
    }

To start from 9600 baud and reset the baud back to the default 115200, use:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    SerialInterfaceClient sic(0);
    
    void setup() {
        ser.begin(9600);
        ser.set(sic.baud_rate_,(uint32_t)115200);
    }
    
    void loop() {
    }

C++
~~~

To use Serial Interface in C++, include serial interface client.hpp. This allows the 
creation of a SerialInterfaceClient object. See the Message Table below for available messages. All 
message objects use the Short Name with a trailing underscore.

A minimal working example for the SerialInterfaceClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: C++

    #include "generic_interface.hpp"
    #include "serial_interface_client.hpp"
    
    uint32_t baud_rate;
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Serial Interface object with obj_id 0
        SerialInterfaceClient serial_interface(0);

        // Use the Serial Interface object
        serial_interface.baud_rate_.get(com);

        // Insert code for interfacing with hardware here
        // baud_rate = serial_interface.baud_rate_.get_reply();

    }

Matlab
~~~~~~

To use Serial Interface in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a SerialInterfaceClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the SerialInterfaceClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a Serial Interface object with obj_id 0
    SerialInterface = SerialInterfaceClient(’com’,com);

    % Use the Serial Interface object
    oldBaud = SerialInterface.get(’baud_rate’); // should be 115200
    SerialInterface.set(’baud_rate’, 9600);

    % Note: the baud rate is now 9600. This com object uses 115200
    
    % Make a new com object at 9600 baud to continue communication
    com = MessageInterface(’COM18’,9600);
    SerialInterface = SerialInterfaceClient(’com’,com);

    newBaud = SerialInterface.get(’baud_rate’); // should be 9600 
    
Python
~~~~~~

To use the Serial Interface Client in Python, import ``iqmotion`` and create a module that has the Serial Interface Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Serial Interface Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    |variable_name|.set("serial_interface", "baud_rate", 9600)  # change baud rate to 9600

Message Table
~~~~~~~~~~~~~

Type ID 16 | Serial Interface
    .. table:: Serial Interface
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+------------+----------+-----------+---------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name |  Access  | Data Type | Unit                |                                Note                                                                                                                                                                      |
        +========+============+==========+===========+=====================+==========================================================================================================================================================================================================+
        | 0      | baud_rate  | get, set | uint32    | :math:`\text{Hz}`   | The default baud rate is 115200 and is reliable up to 2Mbps. Setting the baud rate also performs a Save. Please make sure that your serial interface device supports the baud rate that you want to set. |
        +--------+------------+----------+-----------+---------------------+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        