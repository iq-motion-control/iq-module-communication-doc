ESC Propeller Input Parser
--------------------------

The ESC Propeller Input Parser is an interface between the Propeller Motor Controller and the PWM based
inputs like 1-2ms, OneShot, MultiShot, and DShot. This parser allows the user to control how ratiomatic
values from the PWM input are translated. Inputs can be mapped to PWM control, voltage control, velocity
control, and thrust control. Values can be interpreted as signed/unsigned and clockwise/counter clockwise.

Arduino
~~~~~~~

To use ESC Propeller Input Parser in Arduino, ensure iq_module_communication.hpp is included. This
allows the creation of a EscPropellerInputParserClient object. See the Message Table below for available messages. All
message objects use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the EscPropellerInputParserClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    EscPropellerInputParserClient esc(0);
    
    void setup() {
        ser.begin();
        // Initialize Serial (for displaying information on the terminal)
        Serial.begin(115200);
    }

    void loop() {
        float vel_max = 0;
        ser.get(esc.velocity_max_,vel_max);
        Serial.println(vel_max);
    }

C++
~~~

To use ESC Propeller Input Parser in C++, include esc propeller input parser client.hpp. This allows the
creation of a EscPropellerInputParserClient object. See the Message Table below for available messages. All message objects
use the Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the EscPropellerInputParserClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "esc_propeller_input_parser_client.hpp"
    
    float velocity_max = 1000.0f; // rad/s
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a ESC Propeller Input Parser object with obj_id 0
        EscPropellerInputParserClient esc(0);

        // Use the ESC Propeller Input Parser object
        esc.velocity_max_.set(com, velocity_max);

        // Insert code for interfacing with hardware here
    
    }

Matlab
~~~~~~

To use ESC Propeller Input Parser in Matlab, all Vertiq communication code must be included in your path.
This allows the creation of a EscPropellerInputParserClient object. See the Message Table below for available messages. All
message strings use the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the EscPropellerInputParserClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    
    % Make a EscPropellerInputParserClient object with obj_id 0
    esc = EscPropellerInputParserClient(’com’,com);
    
    % Use the EscPropellerInputParserClient object
    esc.set(’velocity_max’,1000);

Python
~~~~~~

To use the ESC Propeller Input Parser Client in Python, import ``iqmotion`` and create a module that has the ESC Propeller Input Parser Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the ESC Propeller Input Parser Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0)
    
    |variable_name|.set("esc_propeller_input_parser", "velocity_max", 1000) # Set max Velocity to 1000rad/s

Message Table
~~~~~~~~~~~~~

Type ID 60 | ESC Propeller Input Parser

+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| Sub ID | Short Name          | Access         | Data Type | Unit  | Note                                                                                                                                      |
+========+=====================+================+===========+=======+===========================================================================================================================================+
| 0      | mode                | get, set, save | uint8     | enum  | 0 = PWM, 1 = Voltage, 2 = Velocity, 3 = Thrust                                                                                            |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 1      | raw_value           | get, set       | float     | PU    | Input value [0, 1]                                                                                                                        |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 3      | sign                | get, set, save | uint8     | enum  | 0 = unconfigured, 1 = signed positive, 2 = signed negative, 3 = unsigned positive, 4 = unsigned negative                                  |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 4      | volts_max           | get, set, save | float     | V     | Maximum voltage to apply to motor, raw_value scaled to [-volts_max, volts_max] or [0, volts_max]                                          |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 5      | velocity_max        | get, set, save | float     | rad/s | Maximum angular velocity command, raw_value scaled to [-velocity_max, velocity_max] or [0, velocity_max]                                  |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 6      | thrust_max          | get, set, save | float     | N     | Maximum thrust command (requires kt values), raw_value scaled to [-thrust_max, thrust_max] or [0, thrust_max]                             |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 7      | safe_factor         | get, set, save | float     | PU    | Setting the Save Factor between 0.0 and 1.0 will scale down the values coming from the FC. Default value is 1.0                           |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 8      | flip_negative       | get, set, save | uint8     | bool  | Allows the FC and ESC to agree on the meaning of signals coming out of ESC                                                                |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 9      | zero_spin_throttle  | get, set, save | float     | %/100 | The throttle percentage defines what throttle command percentage the :ref:`zero spin throttle regions<zero_spin_throttle_regions>` begin. |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
| 10     | zero_spin_tolerance | get, set, save | float     | %/100 | Defines how far below the Zero Spin Throttle Percentage the zero spin throttle region will extend for positive throttle commands.         |
+--------+---------------------+----------------+-----------+-------+-------------------------------------------------------------------------------------------------------------------------------------------+
