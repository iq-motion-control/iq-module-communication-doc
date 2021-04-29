Hobby Input
-----------

The Hobby Input gives the module the ability to read in a variety of hobby communication protocols.
Supported protocols are standard 1-2ms PWM, OneShot125, OneShot42, MultiShot, and DShot (150 -
1200). The protocols are autodetected by default, but can be set to accept a single specific protocol. The
values read by the Hobby Input are fed into a Parser object, such as the Servo Parser or the ESC Parser.

Arduino
~~~~~~~

To use Hobby Input in Arduino, ensure iq module communication.hpp is included. This allows the creation
of a HobbyInputClient object. See Table 7 for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the HobbyInputClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    HobbyInputClient hin(0);
    
    void setup() {
        ser.begin();
        ser.set(hin.allowed_protocols_,(uint8_t)6);
        ser.save(hin.allowed_protocols_);
    }

    void loop() {
    }

C++
~~~

To use Hobby Input in C++, include hobby input client.hpp. This allows the creation of a HobbyInputClient
object. See Table 7 for available messages. All message objects use the Short Name with a trailing underscore.
All messages use the standard Get/Set/Save functions.

A minimal working example for the HobbyInputClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "hobby_input_client.hpp"
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Hobby Input object with obj_id 0
        HobbyInputClient hin(0);

        // Use the Hobby Input object
        hin.allowed_protocols_.set(com, 3); // Set the protocol to OneShot42
        hin.allowed_protocols_.save(com);

        // Insert code for interfacing with hardware here
    
    }

Matlab
~~~~~~

To use Hobby Input in Matlab, all IQ communication code must be included in your path. This allows the
creation of a Hobby Input object. See Table 7 for available messages. All message strings use the Short
Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the HobbyInputClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a HobbyInputClient object with obj_id 0
    hin = HobbyInputClient(’com’,com);
    
    % Use the EscPropellerInputParserClient object
    hin.set(’allowed_protocols’,3);
    hin.save(’allowed_protocols’);

Python
~~~~~~

To use the Hobby Input Client in Python, include ``iqmotion`` and create a module that has the Hobby Input Client within it's firmware. 
See Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Hobby Input Client is:

.. code-block:: Python

    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    vertiq = iq.vertiq2306(com, 0)
    
    vertiq.set("hobby_input", "allowed_protocols", 4)   # Set the protocol to MultiShot
    vertiq.save("hobby_input", "allowed_protocols")     # Save the protocol 


Message Table
~~~~~~~~~~~~~

Type ID 76 | Hobby Input

+--------+-------------------+----------------+-----------+------+-----------------------------------------------------------------+
| Sub ID |    Short Name     |     Access     | Data Type | Unit |                              Note                               |
+========+===================+================+===========+======+=================================================================+
| 0      | allowed_protocols | get, set, save | uint8     | enum | Standard PWM = 1, OneShot125 = 2, OneShot42 = 3, MultiShot = 4, |
+--------+-------------------+----------------+-----------+------+-----------------------------------------------------------------+
| 1      | protocol          | get, set, save | uint8     | enum |                                                                 |
+--------+-------------------+----------------+-----------+------+-----------------------------------------------------------------+
