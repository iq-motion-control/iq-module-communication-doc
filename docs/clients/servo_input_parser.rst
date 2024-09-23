.. _servo_input_parser:

Servo Input Parser
------------------

The Servo Input Parser is an interface between the Multi Turn Position Controller and the PWM based
inputs like 1-2ms, OneShot, MultiShot, and DShot. This parser allows the user to control how ratiomatic
values from the PWM input are translated. Inputs can be mapped to PWM control, voltage control, velocity
control, and position control. Values are mapped between a minimum value and a maximum value, while
their units are interpreted based on the mapping.

Arduino
~~~~~~~

To use Servo Input Parser in Arduino, ensure iq_module_communication.hpp is included. This allows the
creation of a ServoInputParserClient object. See the Message Table below for available messages. All message objects use the
Short Name with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the ServoInputParserClient is:

.. code-block:: Arduino

    #include <iq_module_communication.hpp>
    
    IqSerial ser(Serial2);
    ServoInputParserClient svo(0);
    
    void setup() {
        ser.begin();
        ser.set(svo.mode_,(uint8_t)1); // Set to Voltage mode
        ser.save(svo.mode_);
    }

    void loop() {
    }

C++
~~~

To use Servo Input Parser in C++, include servo input parser client.hpp. This allows the creation of a
ServoInputParserClient object. See the Message Table below for available messages. All message objects use the Short Name
with a trailing underscore. All messages use the standard Get/Set/Save functions.

A minimal working example for the ServoInputParserClient is:

.. include:: ../notes/c-full-code.rst

.. code-block:: c++

    #include "generic_interface.hpp"
    #include "servo_input_parser_client.hpp"
    
    void main(){
        // Make a communication interface object
        GenericInterface com;

        // Make a Servo Input Parser object with obj_id 0
        ServoInputParserClient svo(0);

        // Use the Servo Input Parser object
        svo.mode_.set(com, 3); // Position control mode
        svo.mode_.save(com);
        svo.unit_min_.set(com, -PI);
        svo.unit_min_.save(com);
        svo.unit_max_.set(com, PI);
        svo.unit_max_.save(com);

        // Insert code for interfacing with hardware here
    
    }

Matlab
~~~~~~

To use Servo Input Parser in Matlab, all Vertiq communication code must be included in your path. This allows
the creation of a ServoInputParserClient object. See the Message Table below for available messages. All message strings use
the Short Names. All messages use the standard Get/Set/Save functions.

A minimal working example for the ServoInputParserClient is:

.. code-block:: Matlab

    % Make a communication interface object
    com = MessageInterface(’COM18’,115200);
    % Make a ServoInputParserClient object with obj_id 0
    ServoInputParser = ServoInputParserClient(’com’,com);
    % Use the ServoInputParserClient object
    ServoInputParser.set(’mode’, 3); // Position control mode
    ServoInputParser.save(’mode’);
    ServoInputParser.set(’unit_min’, -pi);
    ServoInputParser.save(’unit_min’);
    ServoInputParser.set(’unit_max’, pi);
    ServoInputParser.save(’unit_max’);

Python
~~~~~~

To use the Servo Input Parser Client in Python, import ``iqmotion`` and create a module that has the Servo Input Parser Client within its firmware. 
See the Message Table below for available messages. All message strings use the Short Names. 
All messages use the standard Get/Set/Save functions.

A minimal working example for the Servo Input Parser Client is:

.. code-block:: Python
    :substitutions:
    
    import iqmotion as iq
    import math

    com = iq.SerialCommunicator("/dev/ttyUSB0")
    |variable_name| = iq.|module_name|(com, 0, firmware="servo") |module_name_comment|
    
    # Set Servo Limits
    |variable_name|.set("servo_input_parser", "mode", 3)             # Position Control Mode 
    |variable_name|.set("servo_input_parser", "unit_min", -math.pi)  # Min position: -pi
    |variable_name|.set("servo_input_parser", "unit_max", math.pi)   # Max position:  pi  

    # Save Servo Limits
    |variable_name|.save("servo_input_parser", "mode")
    |variable_name|.save("servo_input_parser", "unit_min")
    |variable_name|.save("servo_input_parser", "unit_max") 

.. _servo_input_parser_table:

Message Table
~~~~~~~~~~~~~

Type ID 78 | Servo Input Parser
    .. table:: Servo Input Parser
        :widths: 8 18 15 10 5 50
        :class: tight-table

        +--------+------------+----------------+-----------+----------------------+--------------------------------------------------+
        | Sub ID | Short Name |     Access     | Data Type |  Unit                |                       Note                       |
        +========+============+================+===========+======================+==================================================+
        | 0      | mode       | get, set, save | uint8     | :math:`\text{Enum}`  | 0 = PWM, 1 = Voltage, 2 = Velocity, 3 = Position |
        +--------+------------+----------------+-----------+----------------------+--------------------------------------------------+
        | 1      | unit_min   | get, set, save | float     | :math:`\text{(mode)}`| Minimum value.  Unit determined by mode.         |
        +--------+------------+----------------+-----------+----------------------+--------------------------------------------------+
        | 2      | unit_max   | get, set, save | float     | :math:`\text{(mode)}`| Maximum value.  Unit determined by mode.         |
        +--------+------------+----------------+-----------+----------------------+--------------------------------------------------+
