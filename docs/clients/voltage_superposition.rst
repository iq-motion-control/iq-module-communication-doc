Voltage Superposition
---------------------
TODO

Arduino
~~~~~~~

To use the Voltage Superposition in Arduino, ensure voltage_superposition_client.hpp is included. This
allows the creation of a VoltageSuperpositionClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save
functions. 

A minimal working example for the VoltageSuperpositionClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    VoltageSuperpositionClient voltageSuperposition(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }
    
    void loop() {
        int zeroAngle = 0;
        if(ser.get(voltageSuperposition.zero_angle_, zeroAngle))
        Serial.println(zeroAngle);
    }

C++
~~~

To use the Voltage Superposition client in C++, include voltage_superposition_client.hpp. This allows the
creation of an VoltageSuperposition object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the VoltageSuperpositionClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "voltage_superposition_client.hpp"

    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Voltage Superposition object with obj_id 0
        VoltageSuperpositionClient voltageSuperposition(0);

        // Use the Voltage Superposition Client
        voltageSuperposition.zero_angle_.get(com)

        // Insert code for interfacing with hardware here  
    }

Matlab
~~~~~~

To use the Voltage Superposition client in Matlab, all Vertiq communication code must be included in your
path. This allows the creation of a VoltageSuperpositionClient object. See the Message Table below for available messages.
All message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the VoltageSuperpositionClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a VoltageSuperpositionClient object with obj_id 0
    voltageSuperposition = VoltageSuperpositionClient(’com’,com);
    % Use the VoltageSuperpositionClient object
    zeroAngle = voltageSuperposition.get(’zero_angle’);

Python
~~~~~~

To use the Voltage Superposition Client in Python, include ``iqmotion`` and create a module that has the Arming Handler Client within it's firmware. 
See the table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Voltage Superposition Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    zero_angle= |variable_name|.get("voltage_superposition", "zero_angle") 
    print(f"Zero angle: {zero_angle}")

Message Table
~~~~~~~~~~~~~

Type ID 74 | Voltage Superposition

+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name                    | Access         | Data Type | Unit          | Note                                                                                                                                                                                                                      |
+========+===============================+================+===========+===============+===========================================================================================================================================================================================================================+
| 0      | zero_angle                    | get, set, save | float     | rad           | If True, handler will be automatically armed.                                                                                                                                                                             |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1      | frequency                     | get, set, save | uint8     | 1/rev         | If True, throttle will be used to arm.                                                                                                                                                                                    |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2      | phase                         | get, set       | float     | rad           | Upper limit for throttle to arm.                                                                                                                                                                                          |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3      | amplitude                     | get, set       | float     | V             | Lower limit for throttle to arm.                                                                                                                                                                                          |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4      | voltage                       | get            | float     | V             | If True, throttle will be used to disarm.                                                                                                                                                                                 |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5      | max_allowed_amplitude         | get            | float     | V             | Upper limit for throttle to disarm.                                                                                                                                                                                       |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6      | velocity_cutoff               | get, set, save | float     | rad/s         | Lower limit for throttle to disarm.                                                                                                                                                                                       |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7      | poly_coeff_zero               | get, set, save | float     | V             | Number of consecutive throttles before arming.                                                                                                                                                                            |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8      | poly_coeff_one                | get, set, save | float     | V * (s/rad)   | The state that determines how the module will try to come to a stop and what that final drive mode will be. See row below for each state:                                                                                 |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 9      | poly_coeff_two                | get, set, save | float     | V * (s/rad)^2 | The state that determines if an dhow many times the module will play its disarm song after coming to a stop. See row below for each state:                                                                                |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 10     | poly_coeff_three              | get, set, save | float     | V * (s/rad)^3 | The throttle source used as its armed throttle source for manual arming. The default manual throttle source is Unknown. In order to use manual arming, a throttle source must be set first. See row below for each state: |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 11     | phase_lead_time               | get, set, save | float     |               | Returns True if motor is armed.                                                                                                                                                                                           |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 12     | phase_lead_angle              | get            | float     | rad           | Returns True if motor is armed.                                                                                                                                                                                           |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 13     | phase_act                     | get            | float     | rad           | Returns True if motor is armed.                                                                                                                                                                                           |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 14     | amplitude_act                 | get            | float     | V             | Returns True if motor is armed.                                                                                                                                                                                           |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 15     | sample_mechanical_zero        | set            |           | V             | Returns True if motor is armed.                                                                                                                                                                                           |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 16     | propeller_torque_offset_angle | get, set, save | float     | V             | Returns True if motor is armed.                                                                                                                                                                                           |
+--------+-------------------------------+----------------+-----------+---------------+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
