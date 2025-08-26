Hobby Input
-----------

The Hobby Input gives the module the ability to read in a variety of hobby communication protocols.
Supported protocols are standard 1-2ms PWM, OneShot125, OneShot42, MultiShot, and DShot (150 -
1200). The protocols are autodetected by default, but can be set to accept a single specific protocol. The
values read by the Hobby Input are fed into a Parser object, such as the Servo Parser or the ESC Parser.

Arduino
~~~~~~~

To use Hobby Input in Arduino, ensure iq_module_communication.hpp is included. This allows the creation
of a HobbyInputClient object. See the Message Table below for available messages. All message objects use the Short Name
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
object. See the Message Table below for available messages. All message objects use the Short Name with a trailing underscore.
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

To use Hobby Input in Matlab, all Vertiq communication code must be included in your path. This allows the
creation of a Hobby Input object. See the Message Table below for available messages. All message strings use the Short
Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the HobbyInputClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a HobbyInputClient object with obj_id 0
    HobbyInput = HobbyInputClient(’com’,com);
    
    % Use the EscPropellerInputParserClient object
    HobbyInput.set(’allowed_protocols’,3);
    HobbyInput.save(’allowed_protocols’);

Python
~~~~~~

To use the Hobby Input Client in Python, import ``iqmotion`` and create a module that has the Hobby Input Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Hobby Input Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0) |module_name_comment|
    
    |variable_name|.set("hobby_input", "allowed_protocols", 4)   # Set the protocol to MultiShot
    |variable_name|.save("hobby_input", "allowed_protocols")     # Save the protocol 


Message Table
~~~~~~~~~~~~~

Type ID 76 | Hobby Input
    .. table:: Hobby Input
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | Sub ID | Short Name                    | Access         | Data Type | Unit                        | Note                                                                                                                                                                                                      |
        +========+===============================+================+===========+=============================+===========================================================================================================================================================================================================+
        | 0      | allowed_protocols             | get, set, save | uint8     | :math:`\text{Enum}`         | Standard PWM = 1, OneShot125 = 2, OneShot42 = 3, MultiShot = 4                                                                                                                                            |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 1      | protocol                      | get, set       | uint8     | :math:`\text{Enum}`         | Standard PWM = 1, OneShot125 = 2, OneShot42 = 3, MultiShot = 4. The currently active protocol for hobby input. Should not generally need to be set by users, sending hobby input sets this automatically. |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 2      | calibrated protocol           | get            | uint8     | :math:`\text{Enum}`         | Standard PWM = 1, OneShot125 = 2, OneShot42 = 3, MultiShot = 4. The analog hobby protocol that the module was most recently calibrated with.                                                              |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 3      | calibrated_high_ticks_us      | get, set, save | uint8     | :math:`\mu s`               | The number of microseconds of the calibrated protocol considered to be a 100% throttle command.                                                                                                           |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 4      | calibrated_low_ticks_us       | get, set, save | uint8     | :math:`\mu s`               | The number of microseconds of the calibrated protocol considered to be a 0% throttle command.                                                                                                             |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 5      | reset_calibration             | set            |           |                             | Clears calibration data                                                                                                                                                                                   |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 6      | hobby_telemetry_frequency     | get, set, save | uint16    | :math:`\text{Hz}`           | Sets the frequency at which timer based protocol telemetry will be streamed. 0Hz disables telemetry streaming.                                                                                            |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 7      | hobby_telemetry_speed_style   | get, set, save | uint8     | :math:`\text{Enum}`         | Standard RPM = 0, KISS ERPM/100 = 1. Determines how timer based protocols will report their speed in telemetry packets.                                                                                   |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
        | 8      | allow_dshot_disarming_message | get, set, save | bool      |                             | Determines whether or not the DSHOT disarming command forces a disarming transition or not.                                                                                                               |
        +--------+-------------------------------+----------------+-----------+-----------------------------+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
